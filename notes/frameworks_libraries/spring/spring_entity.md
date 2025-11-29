# SPRING > ENTITY
## SUMMARY
> [!summary]
> [[java_POJO|POJOS]] that represent a table stored in a database. Every instance of this entity will be a row in the table.
## THEORY
> [!danger]- NEVER FINAL
```java
@Entity /* (name="") for JPQL*/3
@Table(name="tableName", schema="")
public class ClassName {
```
> [!warning]- HAS TO HAVE A **NO-ARG-CONSTRUCTOR** & A **PRIMARY KEY**
```java
	@Id
	@GeneratedValue(strategy=GenerationType.[ AUTO | TABLE | SEQUENCE | IDENTITY])
```
### VALIDATIONS

[[jpa]]/database contraints not jakarta or hibernate.

> [!question] VALIDATE IN BOTH DTO AND ENTITY LAYER?
> **YES, ALWAYS SRIVE TO PERFORM VALIDATION AT MULTIPLE STAGES OF ANY APPLICATION**

### MAPPING TO OTHER TABLES

Two key concepts: 
- **Owning entity**: Controls the relationship and contains the FK
- **Inverse entity**: mapped
