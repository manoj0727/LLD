# Is-A vs Has-A Relationship

Understanding relationships between classes is fundamental to good object-oriented design.

---

## Quick Overview

| Relationship | Type | Keyword | Example |
|--------------|------|---------|---------|
| **Is-A** | Inheritance | `extends` | Dog IS-A Animal |
| **Has-A** | Composition/Aggregation | member variable | Car HAS-A Engine |

---

## Is-A Relationship (Inheritance)

> **"Child class IS-A type of Parent class"**

Uses the `extends` keyword to inherit properties and behaviors.

### Example

```java
class Vehicle {
    void start() {
        System.out.println("Starting...");
    }
}

class Car extends Vehicle {
    // Car IS-A Vehicle
    void drive() {
        System.out.println("Driving...");
    }
}

class Bike extends Vehicle {
    // Bike IS-A Vehicle
    void ride() {
        System.out.println("Riding...");
    }
}
```

### Hierarchy Diagram

```
        Vehicle
       /       \
     Car       Bike
      |
   SportsCar
```

- Car IS-A Vehicle
- Bike IS-A Vehicle
- SportsCar IS-A Car (and also IS-A Vehicle)

### When to Use Is-A

- When there's a clear parent-child relationship
- When child is a **specific type** of parent
- When child should inherit ALL behaviors of parent

---

## Has-A Relationship (Association)

> **"Class A HAS-A reference to Class B"**

One class contains or uses another class as a member.

### Example

```java
class Engine {
    void start() {
        System.out.println("Engine starting...");
    }
}

class Car {
    private Engine engine;  // Car HAS-A Engine

    public Car() {
        this.engine = new Engine();
    }

    void startCar() {
        engine.start();
    }
}
```

### More Examples

```
House HAS-A Room
Library HAS-A Book
Person HAS-A Address
Computer HAS-A CPU
```

---

## Types of Has-A Relationships

### 1. Association (General Term)

A general relationship where one object uses another.

```java
class Teacher {
    void teach(Student student) {
        // Teacher uses Student, but doesn't own it
    }
}
```

### 2. Aggregation (Weak Has-A)

- Objects can exist **independently**
- One object **uses** but doesn't **own** the other
- "Knows about" relationship
- Represented by **empty diamond (◇)** in UML

```java
class Department {
    private List<Employee> employees;  // Aggregation

    public void addEmployee(Employee emp) {
        employees.add(emp);
    }
}

// Employee can exist without Department
Employee emp = new Employee("John");
Department dept = new Department();
dept.addEmployee(emp);

// If department is deleted, employee still exists
```

**Real Examples:**
- Library HAS Books (books exist outside library)
- Team HAS Players (players exist outside team)
- University HAS Students (students exist outside university)

### 3. Composition (Strong Has-A)

- Child object **cannot exist** without parent
- Parent **creates and owns** the child
- Parent is responsible for child's **lifecycle**
- Represented by **filled diamond (◆)** in UML

```java
class House {
    private List<Room> rooms;  // Composition

    public House() {
        // House creates its rooms
        rooms = new ArrayList<>();
        rooms.add(new Room("Living Room"));
        rooms.add(new Room("Bedroom"));
    }
}

// Rooms cannot exist without the House
// When House is destroyed, Rooms are destroyed too
```

**Real Examples:**
- Human HAS Heart (heart can't exist without human)
- House HAS Rooms (rooms don't exist outside house)
- Order HAS OrderItems (items belong to specific order)

---

## Aggregation vs Composition

| Aspect | Aggregation (Weak) | Composition (Strong) |
|--------|-------------------|---------------------|
| **Ownership** | No ownership | Parent owns child |
| **Lifecycle** | Independent | Dependent on parent |
| **Can exist alone?** | Yes | No |
| **UML Symbol** | ◇ Empty diamond | ◆ Filled diamond |
| **Relationship** | "Uses" or "Knows" | "Owns" or "Contains" |

### Visual Comparison

```
Aggregation:
┌──────────┐      ┌──────────┐
│   Team   │◇────│  Player  │   Player can exist without Team
└──────────┘      └──────────┘

Composition:
┌──────────┐      ┌──────────┐
│  House   │◆────│   Room   │   Room cannot exist without House
└──────────┘      └──────────┘
```

---

## Is-A vs Has-A: When to Use Which?

### Use Is-A (Inheritance) When:

- There's a true "type of" relationship
- Child should have ALL parent behaviors
- You want polymorphism

```java
// Good use of Is-A
class Dog extends Animal { }  // Dog IS-A Animal ✓
class Manager extends Employee { }  // Manager IS-A Employee ✓
```

### Use Has-A (Composition) When:

- There's a "part of" or "uses" relationship
- You need flexibility to change components
- You want to avoid tight coupling

```java
// Good use of Has-A
class Car {
    Engine engine;  // Car HAS-A Engine ✓
}

class Person {
    Address address;  // Person HAS-A Address ✓
}
```

---

## Favor Composition Over Inheritance

> **"Prefer Has-A over Is-A"** - Common design principle

### Why?

| Inheritance Problems | Composition Benefits |
|---------------------|---------------------|
| Tight coupling | Loose coupling |
| Can't change at runtime | Can swap at runtime |
| Inherits unwanted behaviors | Use only what you need |
| Deep hierarchies are complex | Flat, simple structure |

### Example: Bird Problem

**With Inheritance (Problematic):**

```java
class Bird {
    void fly() { }
    void eat() { }
}

class Penguin extends Bird {
    void fly() {
        // Problem! Penguins can't fly!
        throw new UnsupportedOperationException();
    }
}
```

**With Composition (Better):**

```java
interface FlyBehavior {
    void fly();
}

class CanFly implements FlyBehavior {
    public void fly() {
        System.out.println("Flying!");
    }
}

class CannotFly implements FlyBehavior {
    public void fly() {
        System.out.println("Can't fly");
    }
}

class Bird {
    private FlyBehavior flyBehavior;  // HAS-A fly behavior

    public Bird(FlyBehavior fb) {
        this.flyBehavior = fb;
    }

    public void performFly() {
        flyBehavior.fly();
    }

    // Can change behavior at runtime!
    public void setFlyBehavior(FlyBehavior fb) {
        this.flyBehavior = fb;
    }
}

// Usage
Bird sparrow = new Bird(new CanFly());
Bird penguin = new Bird(new CannotFly());
```

---

## UML Notation Summary

```
┌─────────────────────────────────────────────────────────┐
│              UML RELATIONSHIP SYMBOLS                    │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  Inheritance (Is-A):                                     │
│  Child ────────▷ Parent                                  │
│  (solid line, empty arrow)                               │
│                                                          │
│  Interface Implementation:                               │
│  Class - - - -▷ Interface                               │
│  (dashed line, empty arrow)                              │
│                                                          │
│  Aggregation (Weak Has-A):                              │
│  Whole ◇──────── Part                                   │
│  (empty diamond)                                         │
│                                                          │
│  Composition (Strong Has-A):                            │
│  Whole ◆──────── Part                                   │
│  (filled diamond)                                        │
│                                                          │
│  Association:                                            │
│  Class ──────── Class                                   │
│  (simple line)                                           │
│                                                          │
│  Dependency:                                             │
│  Class - - - -> Class                                   │
│  (dashed line, open arrow)                               │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

---

## Quick Decision Guide

```
Ask yourself:

1. "Is B a type of A?"
   ├── Yes → Consider Is-A (inheritance)
   └── No  → Use Has-A (composition)

2. "Can B exist without A?"
   ├── Yes → Aggregation (weak has-a)
   └── No  → Composition (strong has-a)

3. "Do I need runtime flexibility?"
   ├── Yes → Has-A (composition)
   └── No  → Either might work

4. "Am I inheriting behaviors I don't need?"
   ├── Yes → Refactor to Has-A
   └── No  → Is-A is probably fine
```

---

## Summary

```
┌─────────────────────────────────────────────────────────┐
│              IS-A vs HAS-A SUMMARY                       │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  IS-A (Inheritance)                                      │
│  ├── "Type of" relationship                             │
│  ├── Uses extends/implements                            │
│  └── Example: Dog IS-A Animal                           │
│                                                          │
│  HAS-A (Association)                                     │
│  ├── Aggregation (Weak)                                 │
│  │   ├── Independent existence                          │
│  │   └── Example: Team HAS Players                      │
│  │                                                       │
│  └── Composition (Strong)                               │
│      ├── Dependent existence                            │
│      └── Example: House HAS Rooms                       │
│                                                          │
│  Best Practice:                                          │
│  └── Favor Composition over Inheritance                 │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

---

## Related Topics
- [SOLID Principles](../L1/01_solid_principle.md)
- [Liskov Substitution Principle](../L1/liskov_Substitution.md)
