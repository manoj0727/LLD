# Liskov Substitution Principle (LSP) - Deep Dive

## Definition

> **"Objects of a superclass should be replaceable with objects of its subclasses without breaking the application."**

In simple terms: If `B` is a child of `A`, then wherever you use `A`, you should be able to use `B` instead **without any issues**.

---

## The Core Idea

```
Parent class object → Can be replaced by → Child class object
                      WITHOUT breaking behavior
```

If substitution breaks the code or causes unexpected behavior, you're violating LSP.

---

## Classic Example: Rectangle and Square

### The Problem

Mathematically, a Square IS-A Rectangle. But in code, this creates issues.

```java
class Rectangle {
    protected int width;
    protected int height;

    public void setWidth(int width) {
        this.width = width;
    }

    public void setHeight(int height) {
        this.height = height;
    }

    public int getArea() {
        return width * height;
    }
}

class Square extends Rectangle {
    // Square must have equal width and height
    @Override
    public void setWidth(int width) {
        this.width = width;
        this.height = width;  // Forces height = width
    }

    @Override
    public void setHeight(int height) {
        this.width = height;  // Forces width = height
        this.height = height;
    }
}
```

### Why This Breaks LSP

```java
void testRectangle(Rectangle r) {
    r.setWidth(5);
    r.setHeight(4);

    // Expected: 5 * 4 = 20
    assert r.getArea() == 20;  // FAILS for Square! Returns 16
}

Rectangle rect = new Rectangle();
testRectangle(rect);  // PASSES: area = 20

Rectangle square = new Square();
testRectangle(square);  // FAILS: area = 16 (4 * 4)
```

**Problem:** Substituting `Square` for `Rectangle` breaks expected behavior.

---

## Bird Example

### Violating LSP

```java
class Bird {
    public void fly() {
        System.out.println("I can fly!");
    }
}

class Sparrow extends Bird {
    // OK - sparrows can fly
}

class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguins can't fly!");
    }
}
```

```java
void makeBirdFly(Bird bird) {
    bird.fly();  // Breaks for Penguin!
}

makeBirdFly(new Sparrow());  // Works
makeBirdFly(new Penguin());  // CRASHES!
```

### Following LSP

```java
// Base class - only common behaviors
class Bird {
    public void eat() {
        System.out.println("Eating...");
    }
}

// Separate interface for flying capability
interface Flyable {
    void fly();
}

// Separate interface for swimming capability
interface Swimmable {
    void swim();
}

class Sparrow extends Bird implements Flyable {
    @Override
    public void fly() {
        System.out.println("Sparrow flying...");
    }
}

class Penguin extends Bird implements Swimmable {
    @Override
    public void swim() {
        System.out.println("Penguin swimming...");
    }
}
```

Now there's no broken substitution - each class only promises what it can deliver.

---

## Signs You're Violating LSP

| Sign | Example |
|------|---------|
| Throwing exceptions in overridden methods | `throw new UnsupportedOperationException()` |
| Empty method implementations | `public void fly() { /* do nothing */ }` |
| Type checking in client code | `if (bird instanceof Penguin)` |
| Overriding methods to do nothing | Breaking parent's contract |
| Unexpected behavior after substitution | Different results than expected |

---

## Rules to Follow LSP

### 1. Preconditions Cannot Be Strengthened

Child class should NOT require MORE than parent.

```java
// Parent
class Payment {
    void pay(double amount) {  // accepts any amount
        // process payment
    }
}

// Bad - Strengthens precondition
class CreditCardPayment extends Payment {
    void pay(double amount) {
        if (amount < 10) throw new Exception("Minimum $10");  // WRONG!
        // process
    }
}
```

### 2. Postconditions Cannot Be Weakened

Child class should deliver AT LEAST what parent promises.

```java
// Parent promises to return sorted list
class DataProcessor {
    List<Integer> process(List<Integer> data) {
        Collections.sort(data);
        return data;  // Always sorted
    }
}

// Bad - Weakens postcondition
class FastProcessor extends DataProcessor {
    List<Integer> process(List<Integer> data) {
        return data;  // NOT sorted - breaks promise!
    }
}
```

### 3. Invariants Must Be Preserved

Properties that are always true must remain true.

```java
// Invariant: balance >= 0
class BankAccount {
    protected double balance = 0;

    void withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
        }
    }
}

// Bad - Breaks invariant
class OverdraftAccount extends BankAccount {
    void withdraw(double amount) {
        balance -= amount;  // Can go negative - breaks invariant!
    }
}
```

---

## How to Fix LSP Violations

### Strategy 1: Use Composition Over Inheritance

```java
// Instead of Square extending Rectangle
class Square {
    private int side;

    public void setSide(int side) {
        this.side = side;
    }

    public int getArea() {
        return side * side;
    }
}
```

### Strategy 2: Extract Interfaces

```java
interface Shape {
    int getArea();
}

class Rectangle implements Shape {
    // Rectangle-specific implementation
}

class Square implements Shape {
    // Square-specific implementation
}
```

### Strategy 3: Redesign Hierarchy

```
Before (Wrong):
Bird
├── Sparrow
└── Penguin (can't fly!)

After (Correct):
Bird
├── FlyingBird
│   └── Sparrow
└── SwimmingBird
    └── Penguin
```

---

## Real-World Examples

### Example 1: Collection Framework

```java
List<String> list = new ArrayList<>();  // Can substitute
List<String> list = new LinkedList<>(); // Works the same way

// Both work identically for List operations
list.add("item");
list.get(0);
```

### Example 2: Database Connection

```java
interface DatabaseConnection {
    void connect();
    void query(String sql);
    void close();
}

class MySQLConnection implements DatabaseConnection { /* ... */ }
class PostgresConnection implements DatabaseConnection { /* ... */ }

// Can substitute any implementation
void executeQuery(DatabaseConnection db) {
    db.connect();
    db.query("SELECT * FROM users");
    db.close();
}
```

---

## Quick Checklist

Before creating a subclass, ask:

- [ ] Can the child do everything the parent can?
- [ ] Will substituting child for parent break any tests?
- [ ] Are you overriding methods to throw exceptions or do nothing?
- [ ] Would client code need `instanceof` checks?

If any answer is "yes" to the last two, **reconsider your design**.

---

## Summary

```
┌─────────────────────────────────────────────────────────┐
│           LISKOV SUBSTITUTION PRINCIPLE                  │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  Core Rule:                                              │
│  Child classes must be 100% substitutable for parents   │
│                                                          │
│  Violations:                                             │
│  ├── Throwing exceptions in overrides                   │
│  ├── Empty implementations                              │
│  ├── Type checking (instanceof)                         │
│  └── Breaking parent's contract                         │
│                                                          │
│  Fixes:                                                  │
│  ├── Use composition instead of inheritance             │
│  ├── Extract common interfaces                          │
│  └── Redesign class hierarchy                           │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

---

## Related Topics
- [SOLID Principles Overview](01_solid_principle.md)
- [Is-A vs Has-A Relationships](../L2/strategy_Design.md)
