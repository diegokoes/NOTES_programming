# SPRING > REST API CONTROLLER
## SUMMARY
> [!summary]
> 
## THEORY
```java
@RestController
@RequestMapping("/auth")
```


reflexion for patch ? ? //TODO
### VALIDATIONS

#### @REQUESTPARAMS 

> [!info] Needs @Validated at Class level

> [!warning] Throws ConstraintViolationException

#### @REQUESTBODY
> [!info] Only needs @Valid **per parameter** 

> [!warning] Throws MethodArgumentNotValidException