# SOLID Principles

SOLID is an acronym for 5 design principles that make software designs more **understandable**, **flexible**, and **maintainable**.

```
S - Single Responsibility Principle
O - Open/Closed Principle
L - Liskov Substitution Principle
I - Interface Segregation Principle
D - Dependency Inversion Principle
```

---

## 1. Single Responsibility Principle (SRP)

> **"A class should have only ONE reason to change."**

Each class should have only **one job/responsibility**.

### Bad Example (Violating SRP)

```java
class Invoice {
    private Marker marker;
    private int quantity;

    public Invoice(Marker marker, int quantity) {
        this.marker = marker;
        this.quantity = quantity;
    }

    // Responsibility 1: Calculate total
    public int calculateTotal() {
        return marker.price * this.quantity;
    }

    // Responsibility 2: Print invoice (WRONG - separate concern)
    public void printInvoice() {
        System.out.println("Invoice: " + calculateTotal());
    }

    // Responsibility 3: Save to DB (WRONG - separate concern)
    public void saveToDB() {
        // save invoice to database
    }
}
```

### Good Example (Following SRP)

```java
// Class 1: Only handles invoice data
class Invoice {
    private Marker marker;
    private int quantity;

    public Invoice(Marker marker, int quantity) {
        this.marker = marker;
        this.quantity = quantity;
    }

    public int calculateTotal() {
        return marker.price * this.quantity;
    }
}

// Class 2: Only handles printing
class InvoicePrinter {
    private Invoice invoice;

    public void print(Invoice invoice) {
        System.out.println("Invoice: " + invoice.calculateTotal());
    }
}

// Class 3: Only handles database operations
class InvoiceDAO {
    public void saveToDB(Invoice invoice) {
        // save to database
    }
}
```

### Why SRP?
- Changes in printing won't affect calculation logic
- Easier to test each class independently
- Better code organization

---

## 2. Open/Closed Principle (OCP)

> **"Classes should be OPEN for extension but CLOSED for modification."**

You should be able to add new functionality **without changing existing code**.

### Bad Example (Violating OCP)

```java
class InvoiceDAO {
    public void save(Invoice invoice, String type) {
        if (type.equals("DB")) {
            // save to database
        } else if (type.equals("FILE")) {
            // save to file
        }
        // Adding new type requires modifying this class!
    }
}
```

### Good Example (Following OCP)

```java
// Interface - closed for modification
interface InvoiceDAO {
    void save(Invoice invoice);
}

// Implementation 1 - open for extension
class DatabaseInvoiceDAO implements InvoiceDAO {
    public void save(Invoice invoice) {
        // save to database
    }
}

// Implementation 2 - open for extension
class FileInvoiceDAO implements InvoiceDAO {
    public void save(Invoice invoice) {
        // save to file
    }
}

// New implementation - no modification to existing code!
class CloudInvoiceDAO implements InvoiceDAO {
    public void save(Invoice invoice) {
        // save to cloud
    }
}
```

### Why OCP?
- Adding new features doesn't break existing code
- Reduces risk of introducing bugs
- Promotes use of interfaces

---

## 3. Liskov Substitution Principle (LSP)

> **"If class B is a subtype of class A, then we should be able to replace A with B without breaking the program."**

Child classes must be **substitutable** for their parent classes.

### Bad Example (Violating LSP)

```java
class Bird {
    public void fly() {
        System.out.println("Flying...");
    }
}

class Sparrow extends Bird {
    // Works fine
}

class Penguin extends Bird {
    public void fly() {
        throw new Exception("Can't fly!"); // BREAKS LSP!
    }
}
```

### Good Example (Following LSP)

```java
class Bird {
    public void eat() {
        System.out.println("Eating...");
    }
}

interface Flyable {
    void fly();
}

class Sparrow extends Bird implements Flyable {
    public void fly() {
        System.out.println("Flying...");
    }
}

class Penguin extends Bird {
    // No fly method - doesn't violate LSP
    public void swim() {
        System.out.println("Swimming...");
    }
}
```

### Why LSP?
- Ensures inheritance is used correctly
- Prevents unexpected behavior
- Makes code more predictable

> For detailed examples, see [Liskov Substitution Deep Dive](liskov_Substitution.md)

---

## 4. Interface Segregation Principle (ISP)

> **"Clients should not be forced to implement interfaces they don't use."**

Create **smaller, specific interfaces** rather than one large interface.

### Bad Example (Violating ISP)

```java
interface Worker {
    void work();
    void eat();
    void sleep();
}

class Robot implements Worker {
    public void work() { /* OK */ }
    public void eat() { /* Robot doesn't eat! */ }  // FORCED to implement
    public void sleep() { /* Robot doesn't sleep! */ }  // FORCED to implement
}
```

### Good Example (Following ISP)

```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

interface Sleepable {
    void sleep();
}

class Human implements Workable, Eatable, Sleepable {
    public void work() { /* ... */ }
    public void eat() { /* ... */ }
    public void sleep() { /* ... */ }
}

class Robot implements Workable {
    public void work() { /* ... */ }
    // No need to implement eat() or sleep()!
}
```

### Why ISP?
- Classes only implement what they need
- Reduces code complexity
- Easier to maintain

---

## 5. Dependency Inversion Principle (DIP)

> **"Classes should depend on INTERFACES, not on concrete classes."**

High-level modules should not depend on low-level modules. Both should depend on **abstractions**.

### Bad Example (Violating DIP)

```java
class MySQLDatabase {
    public void save(String data) {
        // save to MySQL
    }
}

class UserService {
    private MySQLDatabase db = new MySQLDatabase(); // Direct dependency!

    public void saveUser(String user) {
        db.save(user);
    }
}
// Problem: Can't easily switch to MongoDB or PostgreSQL
```

### Good Example (Following DIP)

```java
// Abstraction
interface Database {
    void save(String data);
}

// Low-level module 1
class MySQLDatabase implements Database {
    public void save(String data) {
        // save to MySQL
    }
}

// Low-level module 2
class MongoDB implements Database {
    public void save(String data) {
        // save to MongoDB
    }
}

// High-level module - depends on abstraction
class UserService {
    private Database db;

    public UserService(Database db) {  // Dependency Injection
        this.db = db;
    }

    public void saveUser(String user) {
        db.save(user);
    }
}

// Usage - easy to swap implementations
UserService service1 = new UserService(new MySQLDatabase());
UserService service2 = new UserService(new MongoDB());
```

### Why DIP?
- Easy to swap implementations
- Better testability (use mock databases)
- Loosely coupled code

---

## SOLID Summary

| Principle | Key Idea | Benefit |
|-----------|----------|---------|
| **S**RP | One class = One responsibility | Easier to maintain |
| **O**CP | Extend, don't modify | Safer to add features |
| **L**SP | Subclasses must be substitutable | Correct inheritance |
| **I**SP | Small, specific interfaces | No unnecessary code |
| **D**IP | Depend on abstractions | Loosely coupled |

---

## Visual Summary

```
┌─────────────────────────────────────────────────────────┐
│                    SOLID PRINCIPLES                      │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  S - Single Responsibility                               │
│      └── One class, one job                              │
│                                                          │
│  O - Open/Closed                                         │
│      └── Add features without modifying existing code    │
│                                                          │
│  L - Liskov Substitution                                 │
│      └── Child classes can replace parent classes        │
│                                                          │
│  I - Interface Segregation                               │
│      └── Many small interfaces > one large interface     │
│                                                          │
│  D - Dependency Inversion                                │
│      └── Depend on interfaces, not implementations       │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

---

## Next Steps
- Deep dive into [Liskov Substitution Principle](liskov_Substitution.md)
- Learn about [Is-A vs Has-A Relationships](../L2/strategy_Design.md)
