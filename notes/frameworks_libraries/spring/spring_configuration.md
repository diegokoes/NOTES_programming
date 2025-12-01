# SPRING > CONFIGURATION
## BASIC CONFIGURATION IN `application.properties`

Add custom parameters directly in `application.properties`:

```properties
spring.application.name=primerCrud

# H2 Configuration
spring.datasource.url=jdbc:h2:file:~/tienda_practica;AUTO_SERVER=TRUE
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console

# Custom configurations
config.daw.code=666
config.daw.message=primer controlador REST
```

## ACCESSING VALUES WITH `@Value`

### METHOD 1: CLASS-LEVEL FIELDS

```java
@RestController
public class ProductoController {
    
    @Value("${config.daw.code}")
    private String code_conf;
    
    @Value("${config.daw.message}")
    private String message_conf;
    
    @GetMapping("/values-conf")
    public Map<String, String> values() {
        Map<String, String> json = new HashMap<>();
        json.put("code", code_conf);
        json.put("message", message_conf);
        return json;
    }
}
```

### METHOD 2: METHOD PARAMETERS

```java
@GetMapping("/values-conf2")
public Map<String, String> values(
    @Value("${config.daw.code}") String code,
    @Value("${config.daw.message}") String message
) {
    Map<String, String> json = new HashMap<>();
    json.put("code", code);
    json.put("message", message);
    return json;
}
```

---

## EXTERNAL PROPERTIES FILE WITH `@ConfigurationProperties` (BETTER APPROACH)

### STEP 1: CREATE EXTERNAL PROPERTIES FILE

**File:** `src/main/resources/mi-config.properties`

```properties
config.daw.code=pruebaCode
config.daw.message=Mensaje personalizado
```

### STEP 2: LOAD EXTERNAL FILE

**Option A:** In `application.properties`

```properties
spring.config.import=classpath:mi-config.properties
```

**Option B:** Using `@Configuration` class

```java
@Configuration
@PropertySource(value = "classpath:mi-config.properties", encoding = "UTF-8")
@EnableConfigurationProperties(DawConfig.class)
public class AppConfig {
    // No content needed - registers configuration
}
```

### STEP 3: CREATE RESPONSE DTO

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class DawResponseDTO {
    private String code;
    private String message;
}
```

> **Note:** Jackson requires either a no-args constructor or a constructor with all fields for serialization. Avoid using `final` fields as it complicates deserialization.

### STEP 4: CREATE CONFIGURATION CLASS

```java
@Configuration
@ConfigurationProperties(prefix = "config.daw")
@Data
@NoArgsConstructor
public class DawConfig {
    private String code;
    private String message;
}
```

### STEP 5: CREATE CONTROLLER

```java
@RestController
public class DawController {

    @Autowired
    private DawConfig dawConfig;

    @GetMapping("/values-conf")
    public DawResponseDTO values() {
        return new DawResponseDTO(dawConfig.getCode(), dawConfig.getMessage());
    }
}
```

---

## SUMMARY

|Approach|Use Case|Pros|Cons|
|---|---|---|---|
|`@Value`|Simple values|Quick and easy|Hard to maintain with many properties|
|`@ConfigurationProperties`|Multiple related properties|Type-safe, organized, reusable|Requires additional classes|