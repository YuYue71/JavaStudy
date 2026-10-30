# Java 常用套件速查表: 分類索引

[回主目錄](../README.md) / [語法索引](Java語法速查表.md)

套件是命名空間; 類別與方法才是直接使用的功能. 例如 `java.util` 是套件, `ArrayList` 是類別, `add` 是方法.

## 依套件查詢

先依套件或類別進入模組, 再從頁內目錄定位功能. import, 方法寫法, 組合範例與注意事項都保留在對應模組內.

| 套件 | 常用類別與功能 | 使用方式 |
|---|---|---|
| java.lang | String, StringBuilder, Integer, Math | [文字, 數字與輸入](api/01-文字數字與輸入.md) |
| java.math | BigDecimal, BigInteger, RoundingMode | [文字, 數字與輸入](api/01-文字數字與輸入.md) |
| java.util | Scanner, Random | [文字, 數字與輸入](api/01-文字數字與輸入.md) |
| java.util | List, Set, Map, Deque, Arrays, Collections, Comparator | [集合與排序](api/02-集合與排序.md) |
| java.util.stream / java.util.function | Stream, Collectors, Predicate, Function, Consumer, Supplier | [Stream 與 Optional](api/03-Stream與Optional.md) |
| java.util | Optional | [Stream 與 Optional](api/03-Stream與Optional.md) |
| java.time / java.time.format / java.time.temporal | LocalDate, Instant, ZoneId, DateTimeFormatter, ChronoUnit | [日期與時間](api/04-日期與時間.md) |
| java.nio.file / java.nio.charset / java.io | Files, Path, StandardCharsets, BufferedReader, InputStream | [檔案與輸入輸出](api/05-檔案與輸入輸出.md) |
| java.util.regex / java.util | Pattern, Matcher, Objects, UUID, Base64, Properties | [正規表示式與工具](api/06-正規表示式與工具.md) |
| java.net / java.net.http / java.sql | URI, HttpClient, PreparedStatement, ResultSet | [HTTP 與 JDBC](api/07-HTTP與JDBC.md) |
| java.util.concurrent / java.util.concurrent.atomic | Executors, Future, CompletableFuture, AtomicInteger | [並行與日誌](api/08-並行與日誌.md) |
| java.util.logging | Logger, Level | [並行與日誌](api/08-並行與日誌.md) |

## 外部依賴定位

| 名稱 | 用途 | 與 JDK 的關係 |
|---|---|---|
| JUnit Jupiter | 自動化測試 | 額外依賴 |
| Jackson / Gson | JSON 與物件轉換 | 額外依賴; HttpClient 不提供物件 JSON 映射 |
| SLF4J + Logback | 日誌介面與實作 | 額外依賴; JDK 自帶 java.util.logging |
| JDBC 驅動 | 連接特定資料庫 | java.sql 是 API, 驅動另裝 |
| Spring Boot / Hibernate | 後端應用/ORM | 額外框架, 先理解 HTTP, SQL 與介面 |

本手冊不列外部套件版本或設定; 版本需依實際專案選擇.
