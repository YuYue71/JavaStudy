# 03 Stream、函式介面與 Optional

[主目錄](../../README.md) · [套件索引](../Java常用套件速查表.md) · [上一模組](02-集合與排序.md) · [下一模組](04-日期與時間.md)

## import 與資料流程

```java
import java.util.List;
import java.util.Map;
import java.util.Optional;
import java.util.Comparator;
import java.util.stream.Collectors;
import java.util.stream.IntStream;
import java.util.function.*;
```

Stream 流程是「來源 → 中間操作 → 終端操作」。中間操作通常延遲至終端操作才開始執行。每個 Stream 只能消費一次；需要再次運算時重新呼叫 source.stream()。

## 篩選、轉換與彙總

| 操作 | 寫法 | 功能 |
|---|---|---|
| filter | `stream.filter(n -> n >= 60)` | 保留成立元素 |
| map | `stream.map(String::length)` | 每個元素轉換一個值 |
| flatMap | `groups.stream().flatMap(List::stream)` | 將多個清單攤平成一個流程 |
| distinct | `stream.distinct()` | 依 equals/hashCode 去重 |
| sorted | `stream.sorted()` | 自然排序；也可傳 Comparator |
| limit / skip | `stream.skip(10).limit(5)` | 跳過 10 筆後取 5 筆 |
| count | `stream.count()` | long 筆數 |
| anyMatch / allMatch | `stream.anyMatch(n -> n < 0)` | 是否任一／全部符合；可短路 |
| findFirst | `stream.findFirst()` | Optional，可能沒有資料 |
| reduce | `stream.reduce(0, Integer::sum)` | 累積為單一值 |
| toList | `stream.toList()` | Java 17 可用；不可修改 |

```java
List<Integer> scores = List.of(50, 80, 90, 80);
List<Integer> passed = scores.stream()
        .filter(n -> n >= 60).distinct().sorted().toList(); // [80, 90]
int sum = scores.stream().mapToInt(Integer::intValue).sum(); // 300
double average = scores.stream().mapToInt(Integer::intValue)
        .average().orElse(0.0); // 75.0
int oneToFive = IntStream.rangeClosed(1, 5).sum(); // 15
// range(1, 5) 則不包含 5
```

## 收集、分組與重複鍵

```java
List<String> names = List.of("Amy", "Ann", "Bo");
String joined = names.stream().collect(Collectors.joining(", "));
Map<Integer, List<String>> byLength = names.stream()
        .collect(Collectors.groupingBy(String::length));
Map<Boolean, List<String>> partitioned = names.stream()
        .collect(Collectors.partitioningBy(s -> s.length() >= 3));
Map<Integer, Long> counts = names.stream()
        .collect(Collectors.groupingBy(String::length, Collectors.counting()));
Map<String, Integer> lengths = names.stream().collect(Collectors.toMap(
        s -> s, String::length, (oldValue, newValue) -> oldValue));
```

toMap 若鍵可能重複，需指定合併函式，否則拋例外。groupingBy 不保證 Map 的順序。需要可修改的 List 時使用 `Collectors.toCollection(java.util.ArrayList::new)`，不要依賴 toList 結果可修改。

## 函式介面

```java
Predicate<String> nonBlank = s -> !s.isBlank();
Predicate<String> valid = nonBlank.and(s -> s.length() <= 10);
Function<String, Integer> length = String::length;
Consumer<String> output = System.out::println;
Supplier<StringBuilder> factory = StringBuilder::new;
BinaryOperator<Integer> add = Integer::sum;

boolean ok = valid.test("Java");
int size = length.apply("Java");
output.accept("完成");
StringBuilder builder = factory.get();
int total = add.apply(2, 3);
```

Predicate 用 test、Function 用 apply、Consumer 用 accept、Supplier 用 get。BiFunction 接兩個輸入，UnaryOperator 是同型別輸入與輸出的 Function。

## Optional：表達可能沒有結果

```java
Optional<String> found = List.of("Amy", "Bo").stream()
        .filter(s -> s.startsWith("Z")).findFirst();
String fallback = found.orElse("查無資料");
String lazyFallback = found.orElseGet(() -> "預設文字");
found.map(String::toUpperCase).ifPresent(System.out::println);
Optional<String> nullable = Optional.ofNullable(null);
```

| 方法 | 用法 |
|---|---|
| of | 值必須非 null |
| ofNullable | null 轉為 empty |
| map / flatMap | 轉換值／串接回傳 Optional 的操作 |
| filter | 不符合時轉 empty |
| orElse | 預設值會先求值 |
| orElseGet | 只有 empty 時才呼叫 Supplier |
| orElseThrow | 沒值時拋例外，可提供自訂例外 Supplier |
| ifPresent | 有值才執行動作 |

不要未確認就 get，也不要讓 Optional 變數本身為 null。Stream 處理時避免修改來源集合或依賴外部可變狀態；parallelStream 不保證更快，forEach 不保證保持遭遇順序。
