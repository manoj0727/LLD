# What is Low Level Design (LLD)?

## Introduction

**Low Level Design (LLD)** focuses on the internal implementation details of a system - specifically on **classes, objects, and their interactions**.

---

## Types of Design

| Level | Focus | Output |
|-------|-------|--------|
| **HLD** (High Level Design) | System architecture, components | Architecture diagrams, tech stack |
| **LLD** (Low Level Design) | Classes, objects, relationships | Class diagrams, code structure |
| **Actual Code** | Implementation | Working code |

```
HLD (What components?) → LLD (How classes interact?) → Code (Implementation)
```

---

## Goals of LLD

| Goal | Description |
|------|-------------|
| **Clean Code** | Readable, well-organized, follows conventions |
| **Flexible** | Easy to modify without breaking existing features |
| **Maintainable** | Easy to understand and update over time |
| **Testable** | Easy to write unit tests |

---

## Design Patterns Overview

Design patterns are **proven solutions** to common software design problems. Instead of reinventing the wheel, we use these battle-tested approaches.

> "Knowing patterns is good, but understanding WHEN to apply them is what matters."

---

## Three Categories of Design Patterns

### 1. Creational Patterns
**Focus:** How objects are created

| Pattern | Purpose | Use Case |
|---------|---------|----------|
| **Singleton** | Only one instance exists | Database connection, Logger |
| **Factory** | Create objects without exposing creation logic | Creating different types of objects |
| **Builder** | Build complex objects step by step | Creating objects with many parameters |
| **Abstract Factory** | Factory of factories | Creating families of related objects |
| **Prototype** | Clone existing objects | When object creation is expensive |
| **Object Pool** | Reuse expensive objects | Database connections, Thread pools |

---

### 2. Structural Patterns
**Focus:** How classes are arranged/composed together

| Pattern | Purpose | Use Case |
|---------|---------|----------|
| **Adapter** | Convert interface to another | Integrating incompatible systems |
| **Decorator** | Add behavior dynamically | Adding features without changing class |
| **Proxy** | Control access to object | Lazy loading, access control |
| **Facade** | Simple interface to complex system | Simplifying API |
| **Composite** | Treat group as single object | Tree structures, UI components |
| **Bridge** | Separate abstraction from implementation | Platform independence |
| **Flyweight** | Share common data | Memory optimization |

---

### 3. Behavioral Patterns
**Focus:** How objects interact and communicate

| Pattern | Purpose | Use Case |
|---------|---------|----------|
| **Strategy** | Swap algorithms at runtime | Payment methods, sorting algorithms |
| **Observer** | Notify multiple objects of changes | Event systems, notifications |
| **State** | Change behavior based on state | Vending machine, document workflow |
| **Command** | Encapsulate request as object | Undo/Redo, task queuing |
| **Template** | Define skeleton, let subclass fill details | Frameworks, algorithms |
| **Iterator** | Access elements sequentially | Collections, custom iterators |
| **Chain of Responsibility** | Pass request along chain | Logging, request handling |
| **Mediator** | Centralize complex communications | Chat rooms, air traffic control |
| **Memento** | Save/restore object state | Undo functionality |
| **Visitor** | Add operations without changing classes | Compiler, document export |
| **Interpreter** | Interpret grammar/language | SQL parsing, expression evaluation |
| **Null Object** | Provide default behavior | Avoid null checks |

---

## UML Diagrams

In interviews, you may need to represent your design using **UML (Unified Modeling Language)** instead of writing actual code.

### Key UML Relationships:
```
──────────────────────────────────────────────
|  Arrow Type       |  Meaning              |
──────────────────────────────────────────────
|  ───────▷         |  Inheritance (is-a)   |
|  - - - -▷         |  Implements interface |
|  ───────◇         |  Aggregation (has-a)  |
|  ───────◆         |  Composition (owns)   |
|  ─────────        |  Association          |
──────────────────────────────────────────────
```

---

## Summary

```
LLD = Classes + Objects + Their Relationships

Goals:
├── Clean Code
├── Flexible
├── Maintainable
└── Testable

Design Patterns:
├── Creational (Object Creation)
├── Structural (Class Arrangement)
└── Behavioral (Object Interaction)
```

---

## Next Steps
- Learn [SOLID Principles](../L1/01_solid_principle.md)
- Understand [Is-A vs Has-A Relationships](../L2/strategy_Design.md)
