# Java 速查手冊

以「查到就能用」為目的，將語法、標準函式庫、使用範例與常見陷阱放在同一個模組。原教學課程、練習、記帳程式、編譯產物及執行／驗證腳本已移除。

## 主目錄

### 語法

| 模組 | 查詢內容 |
|---|---|
| [01 基礎語法與型別](docs/syntax/01-基礎語法與型別.md) | 程式骨架、編譯、變數、基本型別、轉型、運算子、字串、null |
| [02 流程、陣列與方法](docs/syntax/02-流程陣列與方法.md) | if、switch、迴圈、跳轉、陣列、多維陣列、方法、多載、可變參數 |
| [03 類別、介面與資料模型](docs/syntax/03-類別介面與資料模型.md) | 封裝、建構子、static、繼承、多型、介面、enum、record、sealed |
| [04 泛型、Lambda 與例外](docs/syntax/04-泛型Lambda與例外.md) | 泛型、萬用字元、函式介面、方法參考、例外、資源關閉、註解標記 |

### JDK 常用套件

| 模組 | 套件與功能 |
|---|---|
| [01 文字、數字與輸入](docs/api/01-文字數字與輸入.md) | java.lang、java.math、java.util：String、Math、BigDecimal、Scanner、Random |
| [02 集合與排序](docs/api/02-集合與排序.md) | java.util：List、Set、Map、Deque、Arrays、Collections、Comparator |
| [03 Stream、函式介面與 Optional](docs/api/03-Stream與Optional.md) | java.util.stream、java.util.function、java.util.Optional |
| [04 日期與時間](docs/api/04-日期與時間.md) | java.time、java.time.format、java.time.temporal |
| [05 檔案與輸入輸出](docs/api/05-檔案與輸入輸出.md) | java.nio.file、java.nio.charset、java.io |
| [06 正規表示式與常用工具](docs/api/06-正規表示式與工具.md) | java.util.regex、Objects、UUID、Base64、Properties |
| [07 HTTP 與 JDBC](docs/api/07-HTTP與JDBC.md) | java.net.http、java.net、java.sql |
| [08 並行與日誌](docs/api/08-並行與日誌.md) | java.util.concurrent、java.util.concurrent.atomic、java.util.logging |

也可從 [語法分類索引](docs/Java語法速查表.md) 或 [套件分類索引](docs/Java常用套件速查表.md) 進入。

## 查詢慣例

- 以 Java 17 為準，不啟用預覽功能。例子只示範對應功能，不是一組需要逐課執行的專案。
- 每個 API 模組列出 import；程式片段若未包含類別，放入 `main` 或其他方法內。型別與方法宣告則按說明放在類別層級。
- 各獨立範例可能重複使用變數名稱，請分開使用，不要將整頁原封不動貼進同一個方法。
- 表中的示意識別字如 `list`、`map`、`value` 代表你自己的變數；可直接操作的組合範例放在表格下方。
- JDK 內建套件不需外部依賴；JDBC 的資料庫驅動需另外安裝。第三方框架不納入本手冊的方法清單。

## 快速找問題

| 問題 | 查哪裡 |
|---|---|
| 字串相同但比較失敗、除法沒有小數 | 基礎語法與型別 |
| 不知道用 List、Set 還是 Map | 集合與排序 |
| 清單篩選、分組、計算總和 | Stream 與 Optional |
| 日期加一天、計算間隔、台北時間 | 日期與時間 |
| 讀文字檔、追加、複製目錄檔案 | 檔案與輸入輸出 |
| 驗證格式、切割文字、處理設定 | 正規表示式與工具 |
| 呼叫 API、執行參數化 SQL | HTTP 與 JDBC |
| 背景工作、等待結果、關閉執行緒池 | 並行與日誌 |

這是常用功能手冊，並非 JDK 全部 API 的逐項清單。