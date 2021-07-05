# Object-Oriented Programming

## Principles

* Encapsulation
* Abstraction
* Inheritance
* Polymorphism

#### Encapsulation
Encapsulation is the mechanism of hiding of data implementation by restricting access to public methods. Instance variables are kept private and accessor methods are made public to achieve this.

```java
public class Employee {
    private String name;
    private Date dob;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public Date getDob() {
        return dob;
    }
    public void setDob(Date dob) {
        this.dob = dob;
    }
}
```

#### Abstraction
Abstraction is used to express the intent of a class rather than the actual implementation. A class should not need to know the inner details of another in order to use it, just the interface (API).

#### Inheritance
Inheritances expresses “is-a” and/or “has-a” relationship between two objects. Using Inheritance, In derived classes we can reuse the code of existing super classes. In Java, concept of “is-a” is based on class inheritance (using `extends`) or interface implementation (using `implements`).

#### Polymorphism
Polymorphism means that one name can take many forms. **Static polymorphism** is achieved using method overloading and **dynamic polymorphism** is achieved using method overriding.

This is closely related to inheritance--if we write code that works on the superclass, it will also work with any subclass.

## Best practices: SOLID

#### S: Single Responsibility Principle
Each class should only have one responsibility/job/purpose. Generalized classes should be avoided.

#### O: Open/Closed Principle
Software entities should be open for extension, but closed for modification.

#### L: Liskov's Substitution Principle
Derived/child classes must be substitutable for their parent classes.

#### I: Interface Segregation Principle
Interfaces should only have a single purpose, and clients should only implement interfaces relevant to them.

#### D: Dependency Inversion Principle
High-level modules/classes should not depend on low-level modules/classes; higher classes should depend upon abstraction of lower classes.

https://www.geeksforgeeks.org/best-practices-of-object-oriented-programming-oop/
