---
id: m04-05-inheritance-super
module: m04-oop
part: inheritance
order: 5
title: Inheritance and super
since: java-1.0
java_version: 25
prerequisites: [m04-01-classes-constructors, m04-04-object-contract]
minutes: 15
---

> Sample topic. Short on purpose: it shows the structure, tone and exercise
> style of the course, not the final depth. Exercises are trimmed to three.

## Why it exists

Inheritance expresses an **is-a** relationship: a `EmailAlert` *is an* `Alert`,
so anywhere an `Alert` is expected, an `EmailAlert` will do. It buys two things:

- **Code reuse** — the subclass inherits the parent's fields and methods instead
  of copying them.
- **Substitutability** — code written against the parent keeps working with any
  subclass. This is what makes polymorphism (next topic) possible.

Reach for it when the subtype genuinely *is* the supertype and adds behaviour.
Reach for composition (a field that holds the other object) when it merely
*uses* it. Inheritance is the tighter coupling of the two: a change to the
parent can break every subclass.

## Syntax

```java
class Sub extends Super {   // one parent only: Java has no multiple inheritance of state
    Sub(int x) {
        super(x);           // explicit constructor invocation, first statement*
        // ...
    }

    @Override
    void method() {
        super.method();     // calls the parent's version, not this one
    }
}
```

Four rules that trip people up:

1. Every constructor calls a parent constructor. If you write no `super(...)`,
   the compiler inserts `super()` and fails when the parent has no no-argument
   constructor.
2. `super(...)` may only appear inside a constructor, and `super.method()` only
   inside an instance context (never in a `static` method).
3. `final class` cannot be extended; `final method` cannot be overridden.
   `String`, `Integer` and every record are `final`.
4. **\*Since Java 25** (JEP 513, flexible constructor bodies) statements may come
   *before* `super(...)`. They may not read fields, call instance methods or use
   `this`, because the object is not constructed yet.

## Examples

### A subclass adds behaviour and reuses the parent's

```java title="Alert.java"
public abstract class Alert {

    private final String message;

    protected Alert(String message) {
        this.message = message;
    }

    public String render() {
        return "[ALERT] " + message;
    }

    public String destination() {
        return "console";
    }
}
```

```java title="EmailAlert.java"
public final class EmailAlert extends Alert {

    private final String address;

    public EmailAlert(String message, String address) {
        super(message);          // run the parent constructor first
        this.address = address;  // then the subclass fields
    }

    @Override
    public String render() {
        return super.render() + " -> " + destination();  // reuse, then extend
    }

    @Override
    public String destination() {
        return address;
    }
}
```

```java title="Main.java"
public static void main(String[] args) {
    Alert alert = new EmailAlert("disk full", "ops@example.com");
    System.out.println(alert.render());
}
```

Output:

```text
[ALERT] disk full -> ops@example.com
```

Note `alert.render()` dispatches to the subclass even though the variable is
typed `Alert` — that is dynamic dispatch, covered in `m04-06-method-dispatch`.

### Validating before calling `super` (Java 25)

Before JEP 513 the argument had to be computed inline, or the check moved to a
static helper. Now the constructor can validate first and fail before any parent
work happens:

```java title="Percent.java"
public final class Percent extends Metric {

    public Percent(double value) {
        if (value < 0 || value > 100) {
            throw new IllegalArgumentException("percent out of range: " + value);
        }
        super("percent", value);   // legal in Java 25; was a compile error before
    }
}
```

### Initialization order

```java title="Order.java"
class Base {
    Base() { System.out.println("Base constructor"); }
}

class Derived extends Base {
    private final String field = describe();

    Derived() {
        super();
        System.out.println("Derived constructor, field=" + field);
    }

    private String describe() { return "ready"; }
}
```

Output when `new Derived()`:

```text
Base constructor
Derived constructor, field=ready
```

`super()` runs **before** the subclass field initializers, which is the source of
the next gotcha.

## Gotchas

**Calling an overridable method from a constructor.** The parent constructor runs
while subclass fields are still their default values (`null`, `0`). If it calls a
method the subclass overrides, it sees half-built state:

```java
abstract class Task {
    Task() { System.out.println("Task label: " + label()); }
    abstract String label();
}

class EmailTask extends Task {
    private final String label = "email";
    @Override String label() { return label; }
}
```

`new EmailTask()` prints `Task label: null`. The fix is to pass the value up to
the parent instead of letting the constructor call back down.

**`super()` is optional, its effect is not.** A subclass with no constructor gets
a default one that calls `super()`. If the parent only has `Alert(String)`, the
subclass stops compiling and the message names the parent constructor.

**Field hiding is not overriding.** Declaring `String message` in the subclass
hides the parent's field. `super.message` reads the parent's copy; the two are
unrelated variables and it is virtually never what you want.

**Inheritance vs. `final`.** Extending a class you do not own is a compatibility
promise you cannot retract: once a subclass overrides `render()`, you cannot
change when the parent calls it. That is why library types tend to be `final` and
why the guidance is to design for extension or forbid it.

**`protected` is wider than it looks.** It grants access to the whole package
plus subclasses. In a public API prefer `final` + `private` and expose hooks
deliberately.

## Exercises

### 1. Extend without copying

Write an abstract `Shape` with a `name` field, `area()` returning `double`, and a
`toString()` that returns `name + "(" + area() + ")"`. Then a `final Circle`
whose constructor takes a radius and calls `super("circle")`.

Expected output for `System.out.println(new Circle(2));`

```text
circle(12.566370614359172)
```

### 2. Fix the half-built object

The classes below print `Task label: null`. Change them so they print
`Task label: email` without removing the abstract method.

```java title="Broken.java"
abstract class Task {
    Task() { System.out.println("Task label: " + label()); }
    abstract String label();
}

class EmailTask extends Task {
    private final String label = "email";
    @Override String label() { return label; }
}
```

### 3. Validate before `super`

`Metric(String kind, double value)` throws if `value` is negative. Write
`Percent`, which additionally rejects values above `100`, and make sure an
invalid percent never reaches the `Metric` constructor.

Expected output:

```text
IllegalArgumentException: percent out of range: 140.0
```

## Solutions

### 1.

```java title="Shape.java"
abstract class Shape {
    private final String name;

    Shape(String name) { this.name = name; }

    abstract double area();

    @Override
    public String toString() { return name + "(" + area() + ")"; }
}

final class Circle extends Shape {
    private final double radius;

    Circle(double radius) {
        super("circle");
        this.radius = radius;
    }

    @Override double area() { return Math.PI * radius * radius; }
}
```

`area()` is called from `toString()`, not from the constructor, so the radius is
already set when it runs.

### 2.

Pass the label up so nothing is read before it exists:

```java title="Fixed.java"
abstract class Task {
    private final String label;

    Task(String label) {
        this.label = label;
        System.out.println("Task label: " + label);
    }

    String label() { return label; }
}

final class EmailTask extends Task {
    EmailTask() { super("email"); }
}
```

`label()` is no longer abstract: the parent owns the value, so subclasses cannot
disagree with it.

### 3.

```java title="Percent.java"
final class Percent extends Metric {

    Percent(double value) {
        if (value > 100) {
            throw new IllegalArgumentException("percent out of range: " + value);
        }
        super("percent", value);   // only reached for 0..100
    }
}
```

The lower bound is still `Metric`'s job; `Percent` adds only its own rule. That
keeps the invariant in one place per level.

## References

- [JLS §8.1.4 — Superclasses and Subclasses](https://docs.oracle.com/javase/specs/jls/se25/html/jls-8.html)
- [JLS §8.4.8 — Overriding and Hiding Methods](https://docs.oracle.com/javase/specs/jls/se25/html/jls-8.html)
- [JLS §8.8.7 — Constructor Bodies](https://docs.oracle.com/javase/specs/jls/se25/html/jls-8.html)
- [JLS §12.5 — Creation of New Class Instances](https://docs.oracle.com/javase/specs/jls/se25/html/jls-12.html)
- [JLS §6.6 — Access Control](https://docs.oracle.com/javase/specs/jls/se25/html/jls-6.html)
- [JEP 513 — Flexible Constructor Bodies](https://openjdk.org/jeps/513)
- [`java.lang.Object`](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/lang/Object.html)
