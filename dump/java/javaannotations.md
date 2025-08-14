# JAVA -> DATA TYPES -> ANNOTATIONS
## SUMMARY
> [!summary]
> Annotations provide metadata for compilers and tools, influencing how code is processed. They serve as a powerful way to embed supplementary information within Java code that can be used for documentation, compile-time checking, code generation, and runtime processing. Annotations reduce boilerplate code and enable frameworks to interpret your code according to specific rules.

## THEORY
Annotations are preceded by @ and can affect compiler behavior, generate code, or provide configuration details. They can be applied to declarations (classes, methods, fields, etc.) and, since Java 8, also to type uses.

### BASIC ANNOTATION FORMAT
```java
@Entity  // Simple annotation
@Override void myMethod() { ... }  // Common annotation
@Author(name = "Benjamin Franklin", date = "3/27/2003")  // Annotation with elements
@SuppressWarnings("unchecked")  // Single element shorthand
```

### BUILT-IN ANNOTATIONS
- **@Override**: Verifies method is overriding a superclass method
- **@Deprecated**: Marks code as obsolete with compiler warnings
- **@SuppressWarnings**: Instructs compiler to ignore specific warnings
- **@FunctionalInterface**: Ensures interface has exactly one abstract method
- **@SafeVarargs**: Suppresses warnings for potentially unsafe varargs operations

### META-ANNOTATIONS
Annotations that apply to other annotations:
- **@Retention**: Controls annotation visibility (SOURCE, CLASS, RUNTIME)
- **@Target**: Restricts where an annotation can be applied
- **@Documented**: Includes annotations in Javadoc
- **@Inherited**: Allows annotation to be inherited by subclasses
- **@Repeatable**: Enables multiple applications of the same annotation

### CREATING CUSTOM ANNOTATIONS
```java
@Documented
@interface ClassPreamble {
    String author();
    String date();
    int currentRevision() default 1;
    String[] reviewers();
}
```

### TYPE ANNOTATIONS (JAVA 8+)
```java
List<@NonNull String> list;
@NotNull String text = (@NotNull String) input;
```

### REPEATING ANNOTATIONS (JAVA 8+)
```java
@Repeatable(Schedules.class)
public @interface Schedule {
    String dayOfMonth() default "first";
    String dayOfWeek() default "Mon";
}

public @interface Schedules {
    Schedule[] value();
}

@Schedule(dayOfMonth="last")
@Schedule(dayOfWeek="Fri")
public void doPeriodicCleanup() { ... }
```

## QUESTIONS
> [!tip]- Why use annotations?
> They reduce boilerplate code, provide metadata for tools and frameworks, enable compile-time checking, support documentation, and facilitate aspect-oriented programming features without modifying actual business logic.

> [!warning]- What is a retention policy?
> It specifies whether an annotation is available at source code only (SOURCE), compiled into the class file but ignored by JVM (CLASS), or available at runtime through reflection (RUNTIME). The appropriate retention depends on annotation use case.

> [!danger]- Why create custom annotations?
> It allows domain-specific metadata for libraries and frameworks. Custom annotations can implement processing logic through annotation processors, enable aspect-oriented programming patterns, provide declarative configuration, and enable framework features through reflection at runtime.

> [!tip]- What's the difference between @Override and @Deprecated?
> @Override ensures that a method correctly overrides a superclass method (compile-time check), while @Deprecated marks elements that should no longer be used, generating compiler warnings when used.

> [!warning]- How do repeating annotations work internally?
> Repeating annotations require both the annotation itself marked with @Repeatable and a container annotation type. The compiler automatically wraps multiple annotations in the container annotation, making them accessible at runtime.

> [!danger]- How would you implement a custom annotation processor?
> Custom processors must extend AbstractProcessor, be registered via the ServiceLoader mechanism with a META-INF/services file, implement the process() method to analyze annotated elements, and use the Filer API to generate code. They run during compilation as part of the Java compiler's annotation processing tool.

- - - 
