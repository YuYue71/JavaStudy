# Java 速查手冊

Java 17 的模組化速查手冊. 依問題查找語法或套件, 在同一頁閱讀使用方式, 範例與邊界條件.

## 使用入口

- 已知道關鍵字: 開啟 [語法索引](docs/Java語法速查表.md) 或 [套件索引](docs/Java常用套件速查表.md).
- 只知道需求: 使用下方的 [快速找問題](#快速找問題).
- 已進入模組: 由頁首的 "本頁目錄" 跳至功能, 或使用上一模組/下一模組導覽.

## 文件結構

```text
README.md                   主目錄與範例使用方式
docs/Java語法速查表.md       依語法關鍵字查詢
docs/Java常用套件速查表.md   依套件與類別查詢
docs/syntax/                4 個語法模組
docs/api/                   8 個標準函式庫模組
```

## 主目錄

### 語法

| 模組 | 查詢內容 |
|---|---|
| [01 基礎語法與型別](docs/syntax/01-基礎語法與型別.md) | 程式骨架, 編譯, 變數, 基本型別, 轉型, 運算子, 字串, null |
| [02 流程, 陣列與方法](docs/syntax/02-流程陣列與方法.md) | if, switch, 迴圈, 跳轉, 陣列, 多維陣列, 方法, 多載, 可變參數 |
| [03 類別, 介面與資料模型](docs/syntax/03-類別介面與資料模型.md) | 封裝, 建構子, static, 繼承, 多型, 介面, enum, record, sealed |
| [04 泛型, Lambda 與例外](docs/syntax/04-泛型Lambda與例外.md) | 泛型, 萬用字元, 函式介面, 方法參考, 例外, 資源關閉, 註解標記 |

### JDK 常用套件

| 模組 | 套件與功能 |
|---|---|
| [01 文字, 數字與輸入](docs/api/01-文字數字與輸入.md) | java.lang, java.math, java.util: String, Math, BigDecimal, Scanner, Random |
| [02 集合與排序](docs/api/02-集合與排序.md) | java.util: List, Set, Map, Deque, Arrays, Collections, Comparator |
| [03 Stream, 函式介面與 Optional](docs/api/03-Stream與Optional.md) | java.util.stream, java.util.function, java.util.Optional |
| [04 日期與時間](docs/api/04-日期與時間.md) | java.time, java.time.format, java.time.temporal |
| [05 檔案與輸入輸出](docs/api/05-檔案與輸入輸出.md) | java.nio.file, java.nio.charset, java.io |
| [06 正規表示式與常用工具](docs/api/06-正規表示式與工具.md) | java.util.regex, Objects, UUID, Base64, Properties |
| [07 HTTP 與 JDBC](docs/api/07-HTTP與JDBC.md) | java.net.http, java.net, java.sql |
| [08 並行與日誌](docs/api/08-並行與日誌.md) | java.util.concurrent, java.util.concurrent.atomic, java.util.logging |

也可從 [語法分類索引](docs/Java語法速查表.md) 或 [套件分類索引](docs/Java常用套件速查表.md) 進入.

## 查詢慣例

- 以 Java 17 為準, 不啟用預覽功能. 例子只示範對應功能, 不是一組需要逐課執行的專案.
- 每個 API 模組列出 import; 程式片段若未包含類別, 放入 `main` 或其他方法內. 型別與方法宣告則按說明放在類別層級.
- 各獨立範例可能重複使用變數名稱, 請分開使用, 不要將整頁原封不動貼進同一個方法.
- 表中的示意識別字如 `list`, `map`, `value` 代表你自己的變數; 可直接操作的組合範例放在表格下方.
- JDK 內建套件不需外部依賴; JDBC 的資料庫驅動需另外安裝. 第三方框架不納入本手冊的方法清單.

## 使用程式片段

一般方法內片段可以放入下列外框, 儲存為 `QuickExample.java`. 將文件指定的 import 放在類別之前. 若範例是方法或型別宣告, 則放在類別層級或獨立檔案, 不放進 main.

```java
public class QuickExample {
    public static void main(String[] args) throws Exception {
        // 在這裡放入要查驗的方法內片段.
        System.out.println("Java 17");
    }
}
```

```powershell
javac --release 17 -encoding UTF-8 QuickExample.java
java "-Dfile.encoding=UTF-8" QuickExample
```

`throws Exception` 讓獨立查驗片段時可以直接傳遞 checked exception. 實際應用應依失敗類型捕捉或宣告具體例外, 並在擁有資源的位置負責關閉.

## 快速找問題

| 問題 | 查哪裡 |
|---|---|
| 字串相同但比較失敗, 除法沒有小數 | [基礎語法與型別](docs/syntax/01-基礎語法與型別.md) |
| 不知道用 List, Set 還是 Map | [集合與排序](docs/api/02-集合與排序.md) |
| 清單篩選, 分組, 計算總和 | [Stream 與 Optional](docs/api/03-Stream與Optional.md) |
| 日期加一天, 計算間隔, 台北時間 | [日期與時間](docs/api/04-日期與時間.md) |
| 讀文字檔, 追加, 複製檔案與走訪目錄 | [檔案與輸入輸出](docs/api/05-檔案與輸入輸出.md) |
| 驗證格式, 切割文字, 處理設定 | [正規表示式與工具](docs/api/06-正規表示式與工具.md) |
| 呼叫 API, 執行參數化 SQL | [HTTP 與 JDBC](docs/api/07-HTTP與JDBC.md) |
| 背景工作, 等待結果, 關閉執行緒池 | [並行與日誌](docs/api/08-並行與日誌.md) |

這是常用功能手冊, 並非 JDK 全部 API 的逐項清單.