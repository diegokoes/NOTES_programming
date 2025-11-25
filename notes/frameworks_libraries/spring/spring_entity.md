# SPRING > ENTITY
## SUMMARY
> [!summary]
> [[java_POJO|POJOS]] that represent a table stored in a database. Every instance of this entity will be a row in the table.
## THEORY
> [!danger]- NEVER FINAL
```java
@Entity /* (name="") for JPQL*/
@Table(name="tableName", schema="")
public class ClassName {
```
> [!warning]- HAS TO HAVE A **NO-ARG-CONSTRUCTOR** & A **PRIMARY KEY**
```java
	@Id
	@GeneratedValue(strategy=GenerationType.[ AUTO | TABLE | SEQUENCE | IDENTITY])
```
### VALIDATIONS



### OBJECTS

**Objetos bidireccionales**
Entidad propietaria y entidad inversa

mapped by -> inversa - 
con el join -> entidad propietaria


//TODO ver extends GrantedAuthority