# SPRING > MAPSTRUCK
**Interface** 
```java
@Mapper(componentModel = "spring")
```

## SUMMARY
> [!summary]
>  [[Java]] [[spring_bean|Bean]] mapper without need to write a concrete implementation, it does it at compile time
## THEORY
### WHAT'S NEEDED?
> [!info]- 1-Dependency
> ```xml
> <dependency>
>     <groupId>org.mapstruct</groupId>
>     <artifactId>mapstruct</artifactId>
>     <version>1.6.3</version>
> </dependency>
> ```

> [!info]- 2-Processor
> ```xml
> <plugin>
>    <groupId>org.apache.maven.plugins</groupId>
>   <artifactId>maven-compiler-plugin</artifactId>
>   <version>3.13.0</version>
>   <configuration>
>        <annotationProcessorPaths>
>            <path>
>                <groupId>org.mapstruct</groupId>
>                <artifactId>mapstruct-processor</artifactId>
>                <version>1.6.3</version>
>            </path>
>        </annotationProcessorPaths>
>    </configuration>
> </plugin>
> ```

### HOW TO INJECT

A very manual way would be Mappers.getMapper(NameOfTheClass.class)... 

Much better -> **[[concepts_dependency_injection|Dependency Injection]]**

In the @Mapper tag, it's needed to add:

**First**: 
```java
@Mapper(componentModel = "spring")
public interface SimpleSourceDestinationMapper {
```
**Second**:
//TODO