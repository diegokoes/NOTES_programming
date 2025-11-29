# SPRING > DTO
## SUMMARY
> [!summary]
> Definition of DTO -> **[[concepts_dto|HERE]]**
## THEORY

Using [[lombok]] tags commonly used are **@Data**... 
### VALIDATIONS

Validations with annotations from [[jakartaee]]. 

|Purpose|✅ **Standard Annotation (Jakarta)**|🚫 **Hibernate Alternative**|💡 **Comment / Recommendation**|
|---|---|---|---|
|**Non-null field**|`@NotNull`|—|The most basic and universal.|
|**Non-empty string (not null nor "")**|`@NotBlank`|—|Ideal for `String`, more powerful than `@NotEmpty`.|
|**Non-empty collection / array**|`@NotEmpty`|—|Ensures there's at least one element.|
|**Length / size of string or collection**|`@Size(min, max)`|`@Length(min, max)`|Use `@Size` (it's standard). `@Length` is only for `String`.|
|**Minimum / maximum value (numeric)**|`@Min`, `@Max`|`@Range(min, max)`|Prefer `@Min/@Max`; `@Range` is not standard.|
|**Positive / negative number**|`@Positive`, `@PositiveOrZero`, `@Negative`, `@NegativeOrZero`|—|Recommended for readability.|
|**Number within decimal range**|`@DecimalMin`, `@DecimalMax`|—|Better precision for `BigDecimal`.|
|**Text pattern**|`@Pattern(regexp = "regex")`|—|Use this for regular expressions.|
|**Email**|`@Email`|—|Standard since Bean Validation 2.0.|
|**Validate a nested object**|`@Valid`|—|Allows validating fields within another object.|
|**Past / future date**|`@Past`, `@PastOrPresent`, `@Future`, `@FutureOrPresent`|—|Very useful in entities with dates.|
|**Boolean true value**|`@AssertTrue` / `@AssertFalse`|—|Ideal for custom validations.|
|**URL format**|—|`@URL`|Only exists in Hibernate Validator. Very useful for links.|
|**Valid credit card number**|—|`@CreditCardNumber`|Hibernate-specific, useful if you need it.|
|**ISBN (books)**|—|`@ISBN`|Hibernate-specific, validates ISBN-10 or ISBN-13.|
|**Unique elements in a collection**|—|`@UniqueElements`|Hibernate-specific, no standard equivalent.|
|**Valid UUID identifier**|—|`@UUID`|Hibernate-specific, useful if you handle UUIDs as text.|
|**Value not equal to another field**|—|(custom constraint)|No standard exists, you must create your own annotation.|
