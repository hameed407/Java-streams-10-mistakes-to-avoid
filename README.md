

---
## 🚀 Java Stream API – 10 Common Scenarios with Solutions

This document lists **10 practical Java Stream mistakes and best practices**. Use this as a quick reference to avoid common pitfalls and write cleaner, safer stream code.

---

## ✅ 1. Finding the First Element Starting with "A"

```java
Optional<String> res = list.stream()
    .filter(s -> s.startsWith("A"))
    .findFirst();

res.ifPresent(System.out::println);
````

🧠 Use `Optional` to avoid `NoSuchElementException`.
Avoid `.get()` unless you're 100% sure the value exists.

---

## ✅ 2. Modifying External List vs Using `.toList()`

```java
// Not recommended (mutating external variable)
List<Integer> numbers = new ArrayList<>();
Stream.of(1,2,3,4,5,6).filter(n -> n % 2 == 0).forEach(numbers::add);

// Better (pure stream usage)
List<Integer> result = Stream.of(1,2,3,4,5,6)
    .filter(n -> n % 2 == 0)
    .toList();
```

🎯 Always prefer `.toList()` to build new collections cleanly.

---

## ✅ 3. Simple Iteration vs Meaningful Stream Use

```java
// Just printing — no transformation
names.forEach(System.out::println);

// Recommended for data processing
names.stream()
    .map(String::toUpperCase)
    .filter(name -> name.length() > 3)
    .toList();
```

📌 Use regular loops for basic tasks. Use streams for filtering, mapping, etc.

---

## ✅ 4. Forgetting `.close()` on File Streams

```java
// BAD: resource leak
Stream<String> lines = Files.lines(path);
lines.forEach(System.out::println);

// GOOD: auto-close using try-with-resources
try (Stream<String> lines = Files.lines(path)) {
    lines.forEach(System.out::println);
}
```

⚠️ Always use try-with-resources with file streams to avoid leaks.

---

## ✅ 5. Using `.limit()` Before `.sorted()`

```java
// WRONG: limits unsorted data
list.stream()
    .limit(3)
    .sorted()
    .toList();

// RIGHT: sort first, then limit
list.stream()
    .sorted()
    .limit(3)
    .toList();
```

💡 If order matters, sort **before** limiting.

---

## ✅ 6. Misusing `.collect()` for Mutable Reduction

```java
// BAD: modifying external list inside stream
List<String> result = new ArrayList<>();
names.stream()
    .filter(n -> {
        result.add(n);
        return n.length() > 3;
    }).toList();

// GOOD:
List<String> result = names.stream()
    .filter(n -> n.length() > 3)
    .toList();
```

✅ Streams should not have side effects. Use `.collect()` to gather data.

---

## ✅ 7. Not Handling `null` Values

```java
// May throw NullPointerException
names.stream()
    .map(String::toUpperCase)
    .toList();

// Safe version
names.stream()
    .filter(Objects::nonNull)
    .map(String::toUpperCase)
    .toList();
```

🔐 Always check for `null` before calling methods on stream elements.

---

## ✅ 8. Insufficient Use of Intermediate Operations

```java
// Only mapping — weak logic
names.stream()
    .map(String::toUpperCase)
    .toList();

// Better: combine filter, map, and sort
names.stream()
    .filter(n -> n.length() > 3)
    .map(String::toUpperCase)
    .sorted()
    .toList();
```

✨ Build stronger pipelines with multiple intermediate operations.

---

## ✅ 9. `forEach()` is Not the Same as `map()` + `collect()`

```java
// Just prints values — doesn't store them
names.stream()
    .map(String::toUpperCase)
    .forEach(System.out::println);

// Stores result — correct way
List<String> result = names.stream()
    .map(String::toUpperCase)
    .toList();
```

🧠 Use `forEach()` for side effects like printing.
Use `map()` + `collect()` to create transformed data.

---

## ✅ 10. Overusing `parallelStream()`

```java
// Not always faster
names.parallelStream()
    .map(String::toUpperCase)
    .forEach(System.out::println);

// Better for large CPU-heavy tasks
long count = IntStream.range(1, 1_000_000).boxed()
    .parallel()
    .filter(StreamExamples::isPrime)
    .count();
```

⚠️ Use `parallel()` only for large, CPU-bound tasks. Avoid for I/O or small data.

---

### 📘 Final Tips:

* Avoid modifying external variables in streams.
* Use intermediate operations (map, filter, sorted) meaningfully.
* Prefer `.toList()` over `Collectors.toList()` for clarity and immutability (Java 16+).
* Handle nulls early to prevent crashes.
* Think functional: transform → filter → collect.

---

Happy Streaming! ☕

```

---

```
