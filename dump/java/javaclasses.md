# Java -> Data Types -> Classes
## Summary
> [!summary]
> Classes are blueprints for creating objects. They define state (fields) and behavior (methods).

## Theory
Classes are fundamental to OOP in Java. They can contain fields, methods, constructors, and nested classes. Inheritance allows a class to extend another, promoting code reuse.

public class Example {
    private String value;
    public Example(String value) {
        this.value = value;
    }
    public String getValue() {
        return value;
    }
}

## Questions
> [!tip]- What is the main purpose of a Java class?
> To define the structure and behavior of objects.

> [!warning]- How does inheritance work in Java?
> A subclass extends the superclass and inherits its members.

> [!danger]- Can you explain the concept of nested classes?
> Nested classes are declared within another class, making them logically grouped and potentially private.
