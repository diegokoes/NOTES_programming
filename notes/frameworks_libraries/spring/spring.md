# SPRING FRAMEWORK
## SUMMARY
> [!summary]
> It's a [[concepts_framework|framework]] for not only web applications! And not only [[java]], it supports [[jvm]] languages like [[kotlin]] and [[groovy]]. Not the same as [[spring_boot|Spring Boot]] either!
### EXAM
#### TODO'S
> [!tip]  LEE EL ENUNCIADO, RESPIRA Y DEJA DE AGOBIARTE

> [!warning] LOOK AT THE  SQL TABLES

> [!danger] CHECK THE APPLICATION.PROPERTIES

> [!warning] @Value from ANNOTATION **NOT LOMBOK**

return "redirect:/restaurants"; TO THE CONTROLLER
#### MAPPING ENTITIES
##### ONETOMANY OR MANYTOONE? 
FK -> @JoinColumn(name= blabla_id", nullable = false)  (bd!) (usually Many)
\ other entity?
private ClassName class....
@OneToMany(mappedBy="^\_prop java en otra entity", cascade = CascadeType.ALL, orphanRemoval = true)
private List....
##### MANY TO MANY
```
│   ¿HAY TABLA INTERMEDIA?                                      │
│     → Es @ManyToMany                                            │
│     → UN lado lleva @JoinTable (propietario)                    │
@ManyToMany
    @JoinTable(
        name = "estudiante_asignatura", //Nombre tabla intermedia
        joinColumns = @JoinColumn(name = "estudiante_id"),      // FK hacia ESTA entidad
        inverseJoinColumns = @JoinColumn(name = "asignatura_id") // FK hacia la OTRA
    )
    private Set<Asignatura> asignaturas;
    
│     → El OTRO lleva mappedBy (inverso)                          │
│                                                                 │
│   mappedBy = "nombre del ATRIBUTO JAVA en la otra clase"      │
│     → NUNCA es nombre de columna de BD                          │
│                                                                 │
│   @JoinColumn name = "nombre de COLUMNA en BD"                │
│                                                                 │
│   @JoinTable name = "nombre de TABLA en BD"     
```


#### SESSION
```java
@Component
@SessionScope
@Getter
@Setter
public class ApiSessionToken{
	private String apiToken;
}
```
> luego private final ApiSessionToken apiSessionToken; y ya
#### WEB CLIENT
```java
@Configuration
public class AppConfig {

    @Value("${api.base-url}")      // http://localhost:8081/api
    private String apiUrl;

    @Value("${api.auth-url}")      // http://localhost:8081/auth
    private String authUrl;

    // WebClient para endpoints de la API
    @Bean
    public WebClient webClientAPI(WebClient.Builder builder) {
        return builder
                .baseUrl(apiUrl)
                .build();
    }

    // WebClient para autenticación
    @Bean
    public WebClient webClientAuth(WebClient.Builder builder) {
        return builder
                .baseUrl(authUrl)
                .build();
    }
}
```
##### IN SERVICES
```java
@Service
@RequiredArgsConstructor
public class RestaurantService {

    private final WebClient webClientAPI; //name of the method!!
    private final ApiAuthService apiAuthService;
```

##### CONTROLLER MVC
###### GET NO AUTH
```java
RestaurantDTO[] restaurants = webClientAPI
        .get()
        .uri("/restaurants")
        .retrieve()
        .bodyToMono(RestaurantDTO[].class)
        .block();

return Arrays.asList(restaurants);
```
###### GET PATH VARIABLE
```java
RestaurantDTO dto = webClientAPI
        .get()
        .uri("/restaurants/{id}", id)
        .retrieve()
        .bodyToMono(RestaurantDTO.class)
        .block();
```
###### POST  (AUTH + BODY)
```java
String token = apiAuthService.getToken();

RestaurantDTO dto = webClientAPI
        . post()
        . uri("/restaurants")
        .header("Authorization", "Bearer " + token)
        .bodyValue(restaurantDTO)      // ⬅️ ENVIAR objeto
        .retrieve()
        .bodyToMono(RestaurantDTO.class)  // ⬅️ RECIBIR objeto
        . block();
```



###### DELETE + AUTH
```java
String token = apiAuthService.getToken();

webClientAPI
        .delete()
        .uri("/restaurants/{id}", id)
        .header("Authorization", "Bearer " + token)
        . retrieve()
        . bodyToMono(Void.class)    // No devuelve nada
        .block();
```

#### MVC CONFIG (NO LOGIC VIEWS)
```java
@Configuration
public class MvcConfig implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("home");
        registry. addViewController("/login").setViewName("login");
        registry.addViewController("/error").setViewName("error");
    }
}
```
#### THYMELEAF
```html
<!-- Variable del model -->
th:text="${username}"
th:text="${dish.name}"
th:text="${rest.address}"

<!-- Concatenar texto -->
th:text="'Welcome, ' + ${username}"

<!-- Condicional inline en texto -->
th:text="${mode == 'create'} ? 'Create New Restaurant' : 'Edit Restaurant'"
- - - - - - - - - - - 
<!-- Obtener nombre del usuario autenticado -->
th:text="${#authentication.getName()}"

<!-- Mostrar solo si tiene rol ADMIN -->
th:if="${#authorization.expression('hasRole(''ADMIN'')')}"

<!-- Mostrar solo si NO es ADMIN -->
th:if="${! #authorization.expression('hasRole(''ADMIN'')')}"

<!-- Otras expresiones disponibles -->
th:if="${#authentication.isAuthenticated()}"
th:if="${#authentication.principal. enabled}"
th:if="${#authorization.expression('hasAuthority(''ORDER_CREATE'')')}"
- - - - - - - - - - - 
<!-- Si existe/no es null -->
th:if="${errorMessage}"
th:if="${errorMessage != null}"

<!-- Si lista está vacía -->
th:if="${#lists.isEmpty(restaurants)}"
th:if="${! #lists.isEmpty(restaurants)}"

<!-- Comparación -->
th:if="${mode == 'create'}"
th:if="${mode == 'update'}"

<!-- Negación -->
th:unless="${condition}"  <!-- Opuesto a th:if -->
- - - - - - - - - - - 
<!-- Iterar lista -->
<tr th:each="dish : ${dishes}">
    <td th:text="${dish. name}"></td>
    <td th:text="${dish. price}"></td>
</tr>

<div th:each="rest : ${restaurants}">
    <h5 th:text="${rest.name}"></h5>
</div>
- - - - - - - - - - - 
<!-- Link simple -->
th:href="@{/dashboard}"
th:href="@{/restaurants}"

<!-- Link con PathVariable -->
th:href="@{/restaurants/edit/{id}(id=${rest.id})}"
th:href="@{/restaurants/delete/{id}(id=${rest.id})}"

<!-- Link con RequestParam -->
th:href="@{/search(nombre=${filtro}, page=1)}"
<!-- Genera: /search?nombre=Pizza&page=1 -->

<!-- Action de formulario -->
th:action="@{/logout}"
th:action="@{/restaurants/create}"

<!-- Action condicional -->
th:action="${mode == 'create'} ? '/restaurants/create' : '/restaurants/update/' + ${restaurant.id}"
- - - - - - - - - - - 
<!-- Vincular objeto al form -->
<form th:object="${restaurant}" method="post">

<!-- Vincular campo del objeto (*{} = relativo al th:object) -->
<input th:field="*{name}" class="form-control">
<input th:field="*{address}" class="form-control">
<input th:field="*{phone}" class="form-control">

<!-- Campo hidden -->
<input type="hidden" th:field="*{id}">

<!-- Mostrar errores de validación -->
<div th:if="${#fields.hasErrors('phone')}" 
     th:errors="*{phone}" 
     class="text-danger"></div>
```

#### SECURITY
##### MVC
###### SECURITY CONFIG
```java
import lombok.RequiredArgsConstructor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.HttpMethod;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.AuthenticationProvider;
import org.springframework.security.authentication.dao.DaoAuthenticationProvider;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.method.configuration.EnableMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;


@Configuration
@EnableMethodSecurity // Habilita @PreAuthorize y @PostAuthorize
@RequiredArgsConstructor
//@EnableWebSecurity // No es necesario con Spring Boot 3.x / Spring Security 6.x
public class SecurityConfig {

    private final CustomUserDetailsService userDetailsService;

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return http

                /*
                CSRF (Cross-Site Request Forgery) es un tipo de ataque donde un sitio malicioso puede hacer que un navegador autenticado
                (por ejemplo, con una cookie activa) haga una petición no deseada a otro sitio en el que el usuario está logueado.
                Es un ataque clásico en aplicaciones web basadas en sesiones y cookies.
                Su objetivo es hacer que el navegador del usuario realice una acción sin su consentimiento aprovechando que ya tiene una sesión abierta.
                 */
                /*
                ¿Por qué SI necesitas CSRF en una aplicación web MVC monolítica?
                    - Sí usas cookies para la autenticación, sino tokens JWT que van en el header (normalmente en Authorization: Bearer ...).
                    - Sí hay formularios web ni sesiones HTML (como en aplicaciones MVC tradicionales).
                 */
                //.csrf(csrf -> csrf.ignoringRequestMatchers("/h2-console/**"))
                // Le dice a Spring Security cómo debe manejar las sesiones HTTP
                .headers(headers -> headers.frameOptions(frame -> frame.disable())) // permitir iframes (para H2)
                // Esto actúa antes del controlador
                .authorizeHttpRequests(auth -> auth
                                .requestMatchers("/","/login","/h2-console/**").permitAll() // pública para login/register
                                .anyRequest().authenticated() // enviar jwt
                )
                .formLogin(login ->login
                        .loginPage("/login")
                        .defaultSuccessUrl("/dashboard",true)
                        .failureUrl("/login?error=true")
                        .permitAll()
                )
                .logout(logout -> logout
                        .logoutUrl("/logout")
                        .logoutSuccessUrl("/")
                        .invalidateHttpSession(true)
                        .deleteCookies("JSESSIONID")
                        .permitAll()
                )
                .exceptionHandling(exception -> exception.accessDeniedPage("/error"))
                .build();
    }

    @Bean
    public AuthenticationProvider authenticationProvider() {
        DaoAuthenticationProvider provider = new DaoAuthenticationProvider();
        provider.setUserDetailsService(userDetailsService);
        provider.setPasswordEncoder(passwordEncoder());
        return provider;
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    // Sin este bean, Spring no sabrá cómo inyectar AuthenticationManager en tus clases.
    // Lo usamos en AuthController
    // No lo necesitas si todo el proceso de autenticación lo maneja Spring automáticamente, como cuando usas formLogin()
    // En una API REST con JWT, donde tú haces la autenticación manualmente y devuelves un token (como tú estás haciendo), sí lo necesitas.
    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration config) throws Exception {
        return config.getAuthenticationManager();
    }
}
```
##### API
###### SECURITYCONFIG
```java
@Configuration
@EnableMethodSecurity
@RequiredArgsConstructor
public class SecurityConfig {

    private final JwtFilter jwtAuthFilter;
    private final CustomUserDetailsService userDetailsService;

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        return http
                .csrf(csrf -> csrf.disable())  // ⚠️ Deshabilitado en API
                .sessionManagement(sess -> sess
                    .sessionCreationPolicy(SessionCreationPolicy.STATELESS))  // ⚠️ Sin sesiones
                .authorizeHttpRequests(auth -> auth
                        .requestMatchers("/auth/**", "/h2-console/**"). permitAll()
                        .requestMatchers(HttpMethod.GET, "/api/**").permitAll()
                        .anyRequest().authenticated()
                )
                .addFilterBefore(jwtAuthFilter, UsernamePasswordAuthenticationFilter.class)  // ⚠️ JWT Filter
                .build();
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public AuthenticationManager authenticationManager(AuthenticationConfiguration config) throws Exception {
        return config.getAuthenticationManager();
    }
}
```

###### JWT SERVICE
```java
@Service
@RequiredArgsConstructor
public class JwtService {

    @Value("${JWT_SECRET}")
    private String secret;

    @Value("${JWT_EXPIRATION}")
    private String expiration;

    private SecretKey getSigningKey() {
        byte[] keyBytes = Decoders.BASE64. decode(secret);
        return Keys.hmacShaKeyFor(keyBytes);
    }

    public String generateToken(UserDetails userDetails) {
        Map<String, Object> claims = new HashMap<>();
        Set<String> roles = userDetails.getAuthorities().stream()
                .map(GrantedAuthority::getAuthority)
                .collect(Collectors.toSet());
        claims.put("roles", roles);

        return Jwts.builder()
                .claims(claims)
                .subject(userDetails.getUsername())
                .issuedAt(new Date(System.currentTimeMillis()))
                . expiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60 * Integer.parseInt(expiration)))
                .signWith(getSigningKey(), Jwts.SIG. HS256)
                .compact();
    }

    public String extractUsername(String token) {
        return extractClaim(token, Claims::getSubject);
    }

    public boolean isTokenValid(String token, UserDetails userDetails) {
        final String username = extractUsername(token);
        return username.equals(userDetails.getUsername()) && !isTokenExpired(token);
    }

    private boolean isTokenExpired(String token) {
        return extractClaim(token, Claims::getExpiration).before(new Date());
    }

    public <T> T extractClaim(String token, Function<Claims, T> claimsResolver) {
        final Claims claims = Jwts.parser()
                .verifyWith(getSigningKey())
                .build()
                .parseSignedClaims(token)
                . getPayload();
        return claimsResolver. apply(claims);
    }
}
```

###### JWT FILTER
```java
@Component
@RequiredArgsConstructor
public class JwtFilter extends OncePerRequestFilter {

    private final JwtService jwtService;
    private final UserDetailsService userDetailsService;

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain) throws ServletException, IOException {

        final String authHeader = request.getHeader("Authorization");

        if (authHeader == null || !authHeader.startsWith("Bearer ")) {
            filterChain. doFilter(request, response);
            return;
        }

        final String token = authHeader.substring(7);
        final String username = jwtService.extractUsername(token);

        if (username != null && SecurityContextHolder.getContext().getAuthentication() == null) {
            UserDetails userDetails = userDetailsService.loadUserByUsername(username);

            if (jwtService.isTokenValid(token, userDetails)) {
                UsernamePasswordAuthenticationToken authentication = 
                    new UsernamePasswordAuthenticationToken(userDetails, null, userDetails. getAuthorities());
                authentication.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                SecurityContextHolder.getContext().setAuthentication(authentication);
            }
        }
        filterChain.doFilter(request, response);
    }
}
```
#### USER & ROLE
CustomUserDetailsSerive = SAME FOR BOTH
##### MANY TO ONE
###### USER
```java

import jakarta.persistence.*;
import lombok. Getter;
import lombok.Setter;
import org.springframework. security.core. GrantedAuthority;
import org.springframework.security.core. authority.SimpleGrantedAuthority;
import org.springframework.security. core.userdetails.UserDetails;

import java.util. Collection;
import java. util.List;

@Entity
@Table(name = "users")
@Getter
@Setter
public class User implements UserDetails {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(unique = true, nullable = false)
    private String username;

    @Column(nullable = false)
    private String password;

    private String fullName;

    private String email;

    // ⚠️ MANYTOONE: Un usuario tiene UN solo rol
    @ManyToOne
    private Role role;

    // --------------------- 5 MÉTODOS DE LA INTERFACE UserDetails -----------------

    // ⚠️ IMPORTANTE: Trabaja con UN SOLO ROL, no con Set
    @Override
    public Collection<?  extends GrantedAuthority> getAuthorities() {
        String roleName = role. getName(). toUpperCase();


        if (!roleName.startsWith("ROLE_")) {
            roleName = "ROLE_" + roleName;
        }

        return List. of(new SimpleGrantedAuthority(roleName));
    }

    @Override
    public boolean isAccountNonExpired() { return true; }

    @Override
    public boolean isAccountNonLocked() { return true; }

    @Override
    public boolean isCredentialsNonExpired() { return true; }

    @Override
    public boolean isEnabled() { return true; }
}

``` 
###### ROLE
```java

import jakarta.persistence.*;
import lombok. AllArgsConstructor;
import lombok. Getter;

import java.util.HashSet;
import java.util.Set;

@Entity
@Table(name = "roles")
@Getter
@AllArgsConstructor
public class Role {
    @Id
    @GeneratedValue(strategy = GenerationType. IDENTITY)
    private Long id;

    @Column(unique = true, nullable = false)
    private String name;

    // ⚠️ ONETOMANY: Un rol puede tener MUCHOS usuarios
    @OneToMany(mappedBy = "role")
    private Set<User> users;

    public Role() {
        users = new HashSet<User>();
    }

    public void addUser(User user) {
        users.add(user);
    }

    public void removeUser(User user) {
        users.remove(user);
    }

    @Override
    public String toString() {
        return "Role{" + "id=" + id + ", name='" + name + '\'' + '}';
    }
}
```



##### MANY TO MANY
###### USER
```java
import jakarta.persistence.*;
import lombok.Getter;
import lombok. Setter;
import org.springframework.security. core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import java. util.Collection;
import java.util. HashSet;
import java.util.Set;
import java.util. stream.Collectors;

@Entity
@Table(name = "users")
@Getter
@Setter
public class User implements UserDetails {
    @Id
    @GeneratedValue(strategy = GenerationType. IDENTITY)
    private Long id;

    @Column(unique = true, nullable = false, length = 30)
    private String username;

    @Column(nullable = false)
    private String password;

    @Column
    private String fullName;

    @Column
    private String email;

    // ⚠️ MANYTOMANY: Un usuario puede tener VARIOS roles
    // fetch = FetchType. EAGER: carga los roles siempre (Spring Security los necesita)
    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(
            name = "users_roles",
            joinColumns = @JoinColumn(name = "user_id"),
            inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles;

    public User(){
        roles = new HashSet<>();
    }

    public void addRole(Role rol) {
        roles.add(rol);
        rol.addUser(this);
    }

    public void removeRole(Role rol) {
        roles.remove(rol);
        rol. removeUser(this);
    }

    // --------------------- 5 MÉTODOS DE LA INTERFACE UserDetails -----------------

@Override
public Collection<? extends GrantedAuthority> getAuthorities() {
    return roles.stream()
            .map(rol -> (GrantedAuthority) rol::getName)
            .collect(Collectors.toSet());
}

    @Override
    public boolean isAccountNonExpired() { return true; }

    @Override
    public boolean isAccountNonLocked() { return true; }

    @Override
    public boolean isCredentialsNonExpired() { return true; }

    @Override
    public boolean isEnabled() { return true; }
}
```

###### ROLE
```java

import jakarta.persistence.*;
import lombok. AllArgsConstructor;
import lombok. Getter;

import java.util.HashSet;
import java.util.Set;

@Entity
@Table(name = "ROLES")
@Getter
@AllArgsConstructor
public class Role {
    @Id
    @GeneratedValue(strategy = GenerationType. IDENTITY)
    private Long id;

    @Column(unique = true, nullable = false)
    private String name;

    @Column(length = 100)
    private String description;

    // ⚠️ MANYTOMANY inverso: mappedBy apunta al atributo "roles" en User
    @ManyToMany(mappedBy = "roles")
    private Set<User> users;

    public Role() {
        users = new HashSet<User>();
    }

    public void addUser(User user) {
        users. add(user);
    }

    public void removeUser(User user) {
        users.remove(user);
    }

    @Override
    public String toString() {
        return "Role{" + "id=" + id + ", name='" + name + '\'' + '}';
    }
}
```
#### EXCEPTIONS

##### @REQUESTPARAMS 

> [!info] Needs @Validated at Class level

> [!warning] Throws ConstraintViolationException

##### @REQUESTBODY
> [!info] Only needs @Valid **per parameter** 

> [!warning] Throws MethodArgumentNotValidException

## INDEX
- [[spring_configuration|Configuration]]
- [[spring_entity|Entity]]
- [[spring_dto|DTO]]
- [[spring_service|Service]]
- [[spring_exceptions|Exception Handling]]
- Controllers:
	- [[spring_mvc_controller|MODEL VIEW CONTROLLER]]
	- [[spring_rest_api_controller|REST API CONTROLLER]]
- [[spring_repository|Repository]]
- [[spring_lombok|Lombok]]
- [[spring_security|Security]]
- [[spring_mapstruck|Mapstruck]]