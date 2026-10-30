# 04 泛型、Lambda 與例外

[主目錄](../../README.md) · [語法索引](../Java語法速查表.md) · [上一模組](03-類別介面與資料模型.md)

## 泛型類別、方法與邊界

```java
class Box<T> {
    private final T value;
    Box(T value) { this.value = value; }
    T get() { return value; }
}

class GenericTools {
    static <T> T first(java.util.List<T> items) {
        if (items.isEmpty()) throw new IllegalArgumentException("清單不可為空");
        return items.get(0);
    }
    static <T extends Number> double asDouble(T number) {
        return number.doubleValue();
    }
}
```

使用 `Box<String> box = new Box<>("Java");`、`GenericTools.first(java.util.List.of(1, 2))`。泛型只能使用參考型別，例如 Integer，不能是 int。

| 宣告 | 使用規則 |
|---|---|
| `List<?>` | 不知道元素型別；可讀為 Object，不能任意加入具體值 |
| `List<? extends Number>` | 用於讀取 Number；不能加入 Integer 等具體值 |
| `List<? super Integer>` | 可加入 Integer；讀出只能保證為 Object |
| `<T extends Number>` | 限制 T 是 Number 或其子型別 |

記法：讀來源用 extends，寫入目的地用 super。`List<Integer>` 不是 `List<Number>` 的子型別。泛型多透過型別抹除實作，不能直接 `new T()`、`new T[10]` 或 `instanceof List<String>`。

## Lambda 與方法參考

```java
import java.util.function.Predicate;
import java.util.function.Function;
import java.util.function.Consumer;
import java.util.function.Supplier;
```

```java
Predicate<String> nonBlank = s -> !s.isBlank();
Function<String, Integer> length = String::length;
Consumer<String> print = System.out::println;
Supplier<StringBuilder> create = StringBuilder::new;

boolean valid = nonBlank.test("Java");
int size = length.apply("Java");
print.accept("你好");
StringBuilder builder = create.get();
```

| 語法 | 範例 |
|---|---|
| 單一參數 | `s -> s.length()` |
| 多參數 | `(a, b) -> a + b` |
| 區塊 | `s -> { int n = s.length(); return n * 2; }` |
| 靜態方法參考 | `Integer::parseInt` |
| 特定物件方法 | `System.out::println` |
| 型別的實例方法 | `String::length` |
| 建構子參考 | `StringBuilder::new` |

Lambda 需要目標函式介面（只有一個抽象方法）。捕捉的區域變數必須為 final 或之後未重新指定；不代表被指向的物件不可修改。

## 例外：捕捉、拋出與宣告

```java
try {
    int amount = Integer.parseInt("abc");
    if (amount <= 0) throw new IllegalArgumentException("金額必須為正");
} catch (NumberFormatException e) {
    System.err.println("不是整數");
} catch (IllegalArgumentException e) {
    System.err.println(e.getMessage());
} finally {
    System.out.println("結束本次操作");
}
```

NumberFormatException 是 IllegalArgumentException 的子類別，較具體的 catch 要放前面。

| 寫法 | 用途 |
|---|---|
| `throw new ...` | 此處主動拋出例外 |
| `throws IOException` | 方法宣告可能將例外交給呼叫者處理 |
| `catch (IOException \| SQLException e)` | 多重捕捉；類型不可互為父子 |
| `new IllegalStateException("載入失敗", e)` | 包裝例外並保留原始原因 |
| finally | 通常在離開 try/catch 時執行；不要在內 return |

checked exception（如 IOException）要捕捉或宣告；RuntimeException 及子類別不強制。不要捕捉後完全忽略。程序被強制終止時 finally 不能保證執行。

## try-with-resources

以下方法放在類別內，呼叫端需處理 IOException：

```java
static String firstLine(java.nio.file.Path path) throws java.io.IOException {
    try (var reader = java.nio.file.Files.newBufferedReader(
            path, java.nio.charset.StandardCharsets.UTF_8)) {
        return reader.readLine(); // 空檔案回傳 null
    }
}
```

資源須實作 AutoCloseable；多個資源以分號隔開並依宣告反序關閉。若操作與 close 同時拋出例外，關閉例外會被保留為 suppressed exceptions。

## 常用註解標記

| 標記 | 使用位置與用途 |
|---|---|
| `@Override` | 覆寫方法上，檢查簽章 |
| `@FunctionalInterface` | 介面上，檢查是否符合函式介面 |
| `@Deprecated` | 類別／方法等，標示不建議繼續使用 |
| `@SuppressWarnings("unchecked")` | 最小必要範圍抑制已理解的警告，不會修復型別問題 |

`assert condition : "原因";` 用於內部假設，須 `java -ea ...` 才啟用；不能拿來取代使用者輸入檢查。
