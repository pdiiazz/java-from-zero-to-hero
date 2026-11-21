# Curriculum draft (for review)

Status: **approved by the user**. 14 modules, 66 topics, plus one appendix.
Baseline Java 25 LTS. Audience: programmers who already program, new to Java.
No frameworks.

## What changed in this revision

- **M10 Data access with JDBC is a new module.** JDBC was `m09-05`; it now stands
  alone as `m10-01-jdbc`, so the database seam has a home of its own.
- **Gradle moved from M00 to M01** as `m01-06-gradle`. M00 is now just running
  Java; the platform module owns the toolchain.
- **Pattern matching moved from M11 to M04** as `m04-13-pattern-matching`, next to
  polymorphism, sealed types and records. M12 keeps only the feature map.
- **M09 gained JSON and HTTP** (`m09-05-json`, `m09-06-http`). JSON is taught with
  Jackson as a documented snippet dependency; HTTP uses the JDK's
  `java.net.http.HttpClient`. Raw sockets remain out of scope.
- **M13 puts testing last.** The topic order is now SOLID, design patterns,
  logging/diagnostics, then JUnit 5.
- M04 remains one module with three `part` values: `modeling`, `inheritance`,
  `data-carriers`.
- The preview appendix stays an appendix, outside the main track.

## Topic contract

Every topic file is one Markdown file with YAML frontmatter:

```yaml
---
id: m04-03-static-final-init
module: m04-oop
part: modeling            # optional, only for modules larger than ~8 topics
order: 3
title: static, final and initialization order
since: java-1.0
java_version: 25
prerequisites: [m04-01-classes-constructors]
minutes: 30
---
```

Fixed section order in the body, so a later importer can split the file without
guessing:

`Why it exists` → `Syntax` → `Examples` → `Gotchas` → `Exercises` →
`Solutions` → `References`

Rules:

- One topic per file. No topic spans two files.
- Section headings are literal and never renamed.
- Code fences carry a language and a `title` attribute; no test harness, no CI.
- Every `since:` value names the Java version that introduced the feature. For
  topics about a library or practice rather than a language feature, it names
  the baseline (`java-25`) or the version of the JDK API used.
- `References` cites JEP, JLS section, or official API docs. No blogs.
- `part` is display metadata only. It never changes ids or file paths.

## Modules and topics

### M00 Getting started (1)

1. `m00-01-run-java` — JDK, `javac`, `java`, JShell, compact source files
   (JEP 512) and instance `main` methods

### M01 The platform (6)

2. `m01-01-design-goals` — what Java guarantees: portability, strong typing, no
   pointers, GC, backwards compatibility, naming conventions
3. `m01-02-jvm-bytecode` — JVM, bytecode, class loading, JIT, AOT cache
4. `m01-03-memory-and-gc` — stack vs heap, object headers, GC families (G1, ZGC),
   reachability, weak/soft/phantom references, `Cleaner`
5. `m01-04-releases` — editions, LTS cadence, preview lifecycle, how to read a JEP
6. `m01-05-jpms` — modules, module path, `requires`/`exports`, `ServiceLoader`,
   module import declarations (JEP 511)
7. `m01-06-gradle` — Gradle Kotlin DSL, layout, dependencies, tasks, runnable
   jars, `jlink`, `jpackage`

### M02 Language fundamentals (7)

8. `m02-01-primitives` — the 8 primitives, sizes, wrappers, boxing caches
9. `m02-02-variables-scope` — declarations, `final`, scope, shadowing, `var`
10. `m02-03-numbers` — operators, precedence, overflow, `Math`, `RandomGenerator`,
    rounding
11. `m02-04-conversions` — widening, narrowing, promotions, lossy casts
12. `m02-05-conditionals` — `if`, ternary, classic `switch`, switch expressions,
    exhaustiveness
13. `m02-06-loops` — `while`, `do`, `for`, for-each, labeled break/continue
14. `m02-07-arrays` — arrays, multidimensional arrays, `Arrays` utilities, limits

### M03 Text, numbers and time (5)

15. `m03-01-string` — immutability, interning, Unicode and code points,
    `StringBuilder`, `strip`/`lines`/`repeat`
16. `m03-02-text-blocks-formatting` — text blocks, `formatted`, `Formatter`,
    `Locale`, `NumberFormat`
17. `m03-03-bigdecimal-biginteger` — arbitrary precision, scale, rounding modes,
    why `0.1 + 0.2 != 0.3`
18. `m03-04-regex` — `Pattern`, `Matcher`, groups, named groups, catastrophic
    backtracking
19. `m03-05-datetime` — `Instant`, `LocalDate`, `ZonedDateTime`, `Duration`,
    `Period`, `DateTimeFormatter`, DST traps

### M04 Object-oriented programming (13)

**Part `modeling`**

20. `m04-01-classes-constructors` — classes, `this`, constructor chaining,
    flexible constructor bodies (JEP 513)
21. `m04-02-encapsulation-access` — access modifiers, getters/setters,
    immutability, defensive copying, `Objects` helpers
22. `m04-03-static-final-init` — `static` members, constants, class vs instance
    initialization order, static blocks
23. `m04-04-object-contract` — `equals`, `hashCode`, `toString`, `getClass`,
    identity vs equality, why `clone` is broken

**Part `inheritance`**

24. `m04-05-inheritance-super` — `extends`, `super`, constructor chaining,
    inheritance chains, `final` classes and methods. The golden sample lives at
    `04-oop/m04-05-inheritance-super.md`.
25. `m04-06-method-dispatch` — overloading vs overriding, `@Override`, covariant
    returns, static vs dynamic binding
26. `m04-07-polymorphism-casting` — upcasting, downcasting, `instanceof`,
    `ClassCastException`
27. `m04-08-abstract-vs-interface` — abstract classes, interfaces, `default`,
    `static` and `private` interface methods, multiple inheritance of type
28. `m04-09-nested-inner-anonymous` — static nested, inner, local and anonymous
    classes, why lambdas replaced most anonymous classes
29. `m04-10-sealed` — sealed interfaces and classes, `permits`, exhaustiveness
30. `m04-11-records` — record semantics, compact constructors, custom accessors,
    when not to use records
31. `m04-13-pattern-matching` — type patterns, `switch` patterns, record patterns,
    guarded patterns, unnamed variables

**Part `data-carriers`**

32. `m04-12-enums` — enum constants with state, constructors and methods,
    `EnumSet`/`EnumMap`, enum singleton, exhaustive switches

### M05 Type system (5)

33. `m05-01-generics` — generic types and methods, bounds, recursive bounds
34. `m05-02-wildcards` — variance, PECS, wildcard capture
35. `m05-03-erasure` — type erasure, bridges, heap pollution, `@SafeVarargs`,
    what generics cannot do
36. `m05-04-annotations` — declaring annotations, `@Retention`, `@Target`,
    repeating and type annotations, compile-time processing
37. `m05-05-reflection` — `Class`, `Field`, `Method`, `Constructor`, dynamic
    proxies, module access rules, cost and risk

### M06 Collections (5)

38. `m06-01-collection-contracts` — interfaces, complexity table, mutability,
    immutable factories, `Collections` wrappers
39. `m06-02-implementations` — `ArrayList`, `LinkedList`, `HashSet`,
    `LinkedHashSet`, `TreeSet`, `HashMap`, `LinkedHashMap`, `TreeMap`,
    `ArrayDeque`, concurrent maps
40. `m06-03-comparable-comparator` — natural ordering, `Comparator` chains,
    consistency with `equals`
41. `m06-04-iteration-failfast` — `Iterable`, iterators, fail-fast,
    `ConcurrentModificationException`, safe removal
42. `m06-05-optional` — `Optional` as a return type, what it is not for

### M07 Functional Java (4)

43. `m07-01-lambdas` — functional interfaces, lambdas, method references,
    effectively final captures, `java.util.function`
44. `m07-02-stream-pipelines` — sources, intermediate and terminal operations,
    laziness, short-circuiting
45. `m07-03-collectors` — `toList`, `groupingBy`, `partitioningBy`, downstream
    collectors, `teeing`, `mapMulti`
46. `m07-04-stream-parallelism` — when parallel helps and when it hurts,
    gatherers (JEP 485), stateful lambdas, side effects

### M08 Errors and resources (2)

47. `m08-01-exceptions` — hierarchy, checked vs unchecked, custom exceptions,
    wrapping, suppressed exceptions, exception design
48. `m08-02-try-with-resources` — `AutoCloseable`, close order, suppressed
    exceptions, `finally` pitfalls

### M09 I/O and files (6)

49. `m09-01-path-files` — `Path`, `Files`, walking trees, attributes,
    `WatchService`, why `File` is legacy
50. `m09-02-streams-charsets` — byte vs character streams, buffering, charsets,
    `Scanner`, standard streams, closing rules
51. `m09-03-nio` — buffers, channels, memory-mapped files, `RandomAccessFile`
52. `m09-04-serialization` — `Serializable`, `serialVersionUID`, filters, why it
    is legacy and what replaced it
53. `m09-05-json` — JSON with Jackson (`ObjectMapper`), records as DTOs, parsing
    vs binding, external dependency shown as a snippet only
54. `m09-06-http` — `HttpClient`, requests and responses, `HttpRequest.BodyPublishers`,
    `HttpResponse.BodyHandlers`, async calls, timeouts, error handling

### M10 Data access with JDBC (1)

55. `m10-01-jdbc` — connections, `PreparedStatement`, transactions, resource
    handling. JDK-only (`java.sql`); the future database track extends it.

### M11 Concurrency (6)

56. `m11-01-threads` — lifecycle, `start` vs `run`, interruption, daemon threads
57. `m11-02-memory-model` — happens-before, `volatile`, safe publication,
    visibility vs atomicity
58. `m11-03-synchronization` — `synchronized`, `ReentrantLock`, `StampedLock`,
    atomics and CAS, deadlock diagnosis
59. `m11-04-executors` — pools, `ExecutorService`, `Future`,
    `CompletableFuture`, `ForkJoinPool`, shutdown
60. `m11-05-virtual-threads` — virtual vs platform threads, pinning, when they
    help and when they do not
61. `m11-06-scoped-values` — `ThreadLocal` and its leaks, scoped values
    (JEP 506)

### M12 Language evolution (1 + appendix)

62. `m12-01-feature-map` — which feature arrived in which release; how to read
    and migrate old code

Appendix: `appendix-preview-features` — preview and incubating features worth
tracking: primitive types in patterns (JEP 507), structured concurrency
(JEP 505), stable values (JEP 502), vector API, foreign function and memory API,
class-file API. Excluded from the main track.

### M13 Design, testing and diagnostics (4)

63. `m13-01-solid` — SOLID, cohesion and coupling, dependency inversion by hand
64. `m13-02-design-patterns` — strategy, factory, builder, decorator, adapter,
    template method, DAO; when a pattern is overkill
65. `m13-03-logging-diagnostics` — `System.Logger`/`java.util.logging`, JFR,
    `jcmd`, `jstack`, reading a thread dump, heap histogram
66. `m13-04-testing` — tests, assertions, lifecycle, parameterized tests, test
    design without mocking frameworks. Testing is deliberately last, after the
    design topics it exercises.

## What I extracted from the cheat sheet

The Spanish cheat sheet has 58 sections. All of them were mapped, not read once
and forgotten:

| Cheat-sheet chapter | Verdict |
| --- | --- |
| I. Fundamentos (1–2), 32 | Folded into `m00-01`, `m01-01`, `m01-02`, `m01-06` |
| II. Tipos (3–7) | `m02-01` … `m02-04` |
| III. API básica (8–12) | Package/module model → `m01-05`; `Math` → `m02-03`; `BigInteger`/`BigDecimal` → `m03-03`; `String` → `m03-01`; `Scanner` → `m09-02` |
| IV. Control de flujo (13–14) | `m02-05`, `m02-06` |
| V. Arrays (15) | `m02-07` |
| VI. POO (16–31) | All 16 sections land in M04: 16 → module intro, 17 → 20, 18 → 21, 19 → 25, 20/21 → 22, 22/23 → 24, 24 → 23, 25 → 21, 26/27 → 26, 28/29 → 27, 30/31 → 28 |
| VII. Empaquetado y excepciones (32–33) | `m01-06`, `m08-01`, `m08-02` |
| VIII. Entrada/Salida (34–38) | `m09-01` … `m09-04` |
| IX. Genéricos y concurrencia (39–40) | `m05-01` … `m05-03`, `m11-01` |
| X. Colecciones (41) | `m06-01`, `m06-02`, `m06-04` |
| XI. Redes y BD (42–45) | Raw sockets dropped; JDBC + `PreparedStatement` + transactions → `m10-01`; HTTP → `m09-06`; JSON → `m09-05` |
| XII. Web y reflexión (46–48) | Servlets and JavaBeans dropped; reflection → `m05-05` |
| XIII. Java moderno (49–58) | SOLID → `m13-01`; patterns → `m13-02`; lambdas and method refs → `m07-01`; streams → `m07-02` … `m07-04`; `Optional` → `m06-05`; records → `m04-11`; enums → `m04-12`; annotations → `m05-04`; `Comparable`/`Comparator` → `m06-03`; pattern matching → `m04-13` |

### Gaps in the cheat sheet that this plan closes

- **Missing entirely:** build tools, testing, JPMS, generics depth (erasure),
  `java.time`, regular expressions, logging and diagnostics, GC and memory,
  `ThreadLocal` and scoped values, atomics, `default`/`static`/`private`
  interface methods beyond a mention, JSON and HTTP.
- **Missing from Java 9–25:** text blocks, switch expressions, records, sealed
  types, pattern matching, record patterns, virtual threads, stream gatherers,
  flexible constructor bodies, compact source files, module imports, unnamed
  variables.
- **Thin where it matters:** `equals` without `hashCode`, casting without
  `ClassCastException` handling, concurrency limited to `Thread` + `Runnable`.

### Corrections to carry into the English translation

- `sc.close()` on `System.in` closes standard input for the whole JVM. Teach a
  scanner that is not closed, or is scoped to a file.
- `equals` shown without `hashCode` breaks the `Object` contract. Fix in the
  translation.
- Lazy singleton in section 50 is not thread-safe. Teach the holder idiom or the
  enum singleton.
- `Math.random()` is superseded by `RandomGenerator` (Java 17+) for anything
  beyond a toy.
- `String.split` takes a regex; `"."` must be escaped. Currently unmentioned.
- `File` is legacy; `Path`/`Files` lead in this course.

## Deliberate omissions

- Raw sockets, Servlets, JavaBeans: networking and web/framework territory,
  outside the no-frameworks ceiling. The old `m09-06-sockets` ticket is closed.
- WebSockets: only a client exists in the JDK (`java.net.http.WebSocket`); a
  server requires a framework. Deferred to the Spring path.
- Spring: separate future learning path. The seam is M13 (SOLID, patterns,
  dependency inversion by hand).
- Foreign function and memory API, class-file API, vector API: listed in the
  preview/advanced appendix, not taught as core.
- Stream parallelism internals: appendix depth only.

## Open items

- Cheat-sheet translation: keep it as a separate quick-reference document that
  links each section to its topic id. (Decided; awaiting execution.)
- Remaining: work is tracked in ClickUp, parent task `1an27bp8nfr`.
