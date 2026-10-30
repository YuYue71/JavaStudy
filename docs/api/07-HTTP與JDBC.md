# 07 HTTP 與 JDBC

[主目錄](../../README.md) · [套件索引](../Java常用套件速查表.md) · [上一模組](06-正規表示式與工具.md) · [下一模組](08-並行與日誌.md)

## java.net.http：HTTP GET

這些範例會連線，需有效網路；POST 地址為示意，使用前換成你有權使用的服務。

```java
import java.io.IOException;
import java.net.URI;
import java.net.URLEncoder;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.nio.charset.StandardCharsets;
import java.time.Duration;
```

```java
HttpClient client = HttpClient.newBuilder()
        .connectTimeout(Duration.ofSeconds(5))
        .followRedirects(HttpClient.Redirect.NORMAL)
        .build();
HttpRequest request = HttpRequest.newBuilder(URI.create("https://example.com"))
        .timeout(Duration.ofSeconds(10))
        .header("Accept", "text/html")
        .GET().build();
try {
    HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
    int status = response.statusCode();
    if (status >= 200 && status < 300) {
        System.out.println(response.body());
    } else {
        System.err.println("HTTP 失敗：" + status);
    }
} catch (InterruptedException e) {
    Thread.currentThread().interrupt();
} catch (IOException e) {
    System.err.println("連線失敗：" + e.getMessage());
}
```

| 功能 | 使用方式 |
|---|---|
| 建立 URI | `URI.create("https://example.com")`；不合法時拋 IllegalArgumentException |
| Header | `builder.header("Authorization", "Bearer " + token)`；token 從設定取得 |
| 文字回應 | `HttpResponse.BodyHandlers.ofString()` |
| 位元組回應 | `HttpResponse.BodyHandlers.ofByteArray()` |
| 直接存檔 | `HttpResponse.BodyHandlers.ofFile(path)`；先確認輸出路徑 |
| 非同步 | `client.sendAsync(request, BodyHandlers.ofString())` 回傳 CompletableFuture |
| 查詢參數編碼 | `URLEncoder.encode(value, StandardCharsets.UTF_8)` 用於表單風格參數值，不是完整 URL |

HTTP 404/500 通常不會自動拋例外，需查狀態碼。可共用 HttpClient；Java 17 的 HttpClient 不能用 try-with-resources。

## JSON POST 請求建構

```java
HttpRequest post = HttpRequest.newBuilder(URI.create("https://example.com/api/items"))
        .timeout(Duration.ofSeconds(10))
        .header("Content-Type", "application/json; charset=UTF-8")
        .POST(HttpRequest.BodyPublishers.ofString("{\"name\":\"Java\"}", StandardCharsets.UTF_8))
        .build();
// 用上方 client.send(post, ...) 並處理例外與狀態碼。
```

固定小範例可以寫字串；動態 JSON 通常交給 Jackson/Gson 等函式庫，避免自己拼接造成跳脫錯誤。不要將認證資訊寫進原始碼或日誌。

## java.sql：JDBC 查詢

需要資料庫與對應 JDBC 驅動；JDK 只提供 API。以下方法放在類別內。URL、帳號、密碼、id 由呼叫端提供；資料表假設有 students(id, name)。

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
```

```java
static void findStudent(String url, String user, String password, int id)
        throws SQLException {
    String sql = "SELECT name FROM students WHERE id = ?";
    try (Connection connection = DriverManager.getConnection(url, user, password);
         PreparedStatement statement = connection.prepareStatement(sql)) {
        statement.setInt(1, id); // JDBC 參數從 1 開始
        try (ResultSet rows = statement.executeQuery()) {
            while (rows.next()) {
                System.out.println(rows.getString("name"));
            }
        }
    }
}
```

| 功能 | 使用方式／注意事項 |
|---|---|
| 查詢 | executeQuery 回傳 ResultSet，先 next 才能取欄位 |
| 新增／更新／刪除 | executeUpdate 回傳影響筆數 |
| 綁定值 | setString、setInt、setBigDecimal；setNull 需 SQL 型別 |
| SQL NULL | getInt 遇 NULL 會回傳 0，需 wasNull 或 getObject 配合型別 |
| 識別字 | 表名、欄位名不能靠 `?` 綁定，需固定值或白名單 |
| 資源關閉 | Connection、PreparedStatement、ResultSet 均用 try-with-resources |

## JDBC 交易

以下方法假設呼叫端傳入**專用、目前為 auto-commit 模式**的連線，並自行關閉連線；不應對既有交易使用此方法。假設 students 的 id 為唯一鍵。

```java
static void renameStudent(Connection connection, int id, String name) throws SQLException {
    if (!connection.getAutoCommit()) {
        throw new SQLException("此方法需要未啟動交易的專用連線");
    }
    connection.setAutoCommit(false);
    boolean transactionFinished = false;
    try (PreparedStatement statement = connection.prepareStatement(
            "UPDATE students SET name = ? WHERE id = ?")) {
        statement.setString(1, name);
        statement.setInt(2, id);
        if (statement.executeUpdate() != 1) throw new SQLException("找不到唯一學生");
        connection.commit();
        transactionFinished = true;
    } catch (SQLException e) {
        try {
            connection.rollback();
            transactionFinished = true;
        } catch (SQLException rollbackError) {
            e.addSuppressed(rollbackError);
        }
        throw e;
    } finally {
        // rollback 失敗時不切回 auto-commit，以免隱含提交未完成交易。
        if (transactionFinished) connection.setAutoCommit(true);
    }
}
```

正式應用需處理連線復原失敗、逾時與連線池回收規則。發生回滾失敗的連線應關閉或依連線池規則淘汰，不繼續使用。重試寫入操作前要考慮是否可能重複執行。
