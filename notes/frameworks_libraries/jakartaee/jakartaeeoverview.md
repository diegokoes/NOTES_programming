# JAKARTAEE -> OVERVIEW

## SUMMARY

> [!summary] Jakarta EE is a set of standardized enterprise Java specifications (successor to Java EE) maintained by the Eclipse Foundation. It defines portable APIs and a runtime model for server-side Java apps (web, REST, persistence, transactions, messaging), implemented by multiple vendors.
>

## THEORY

### WHAT JAKARTA EE IS

Jakarta EE is a **specification platform** (not a single library or framework). Vendors implement the specs (e.g., WildFly, Payara, OpenLiberty, TomEE). Its core purpose is to ensure **portability and standardization** across application servers.

### CORE APIS

| API | Purpose |
|-----|---------|
| Servlets | Handle web request basics |
| JAX-RS | Build REST endpoints |
| JPA | Manage persistence with EntityManager |
| CDI | Provide dependency injection and lifecycle |
| JTA | Handle transactions |
| JMS | Enable messaging |
| Bean Validation | Validate data |
| Jakarta Security | Manage security |
| EJB | Optional business components |

### RUNTIME MODEL & SERVER FEATURES

- **Container-managed lifecycle**: Handles app startup, shutdown, and resource injection (e.g., datasources, JNDI).
- **Declarative services**: Includes transactions, security, and pooling out of the box.
- **Packaging**: Apps deploy as WAR/EAR on compliant servers or lightweight runtimes with MicroProfile.

### BENEFITS

- **Vendor portability**: Standard APIs allow apps to run on any compliant server.
- **Enterprise-ready**: Built-in server-side services for scalable, managed applications.

### JAKARTA EE VS [[spring|Spring]]

| Aspect | Jakarta EE | [[spring|Spring]] |
|--------|------------|--------|
| Nature | Standards/specifications | Framework with implementations |
| DI & Programming Model | Uses CDI and standard APIs | Own container and opinionated modules |
| Deployment | Targets app servers | Favors embedded servers and convention-over-configuration |
| Ecosystem & Velocity | Focuses on multi-vendor stability | Broader, faster-evolving ecosystem |
| Use Cases | Portability and server features | Rapid microservice development |

## QUESTIONS

> [!tip]- How does selecting a specific Jakarta EE application server impact what features or APIs you can use in your application?
> Server choice determines compliance levels, vendor extensions (e.g., proprietary APIs), and runtime capabilities; unlike [[spring|Spring]]'s fixed embedded server, Jakarta allows swapping servers for portability but may limit access to non-standard features or require testing for compatibility across vendors.

> [!warning]- What are the trade-offs of using vendor-specific extensions in a Jakarta EE application?
> Extensions can provide advanced features or optimizations but reduce portability, as they may not work on other servers; always weigh against sticking to standard APIs for multi-vendor compatibility.

> [!warning]- How do you handle configuration of resources like datasources in Jakarta EE versus [[spring|Spring]]?
> In Jakarta EE, resources are often configured via JNDI on the server (e.g., in server.xml), promoting centralized management; [[spring|Spring]] uses application properties or annotations for embedded, app-specific config, offering more developer control but less server-level standardization.

> [!danger]- Describe a scenario where Jakarta EE's server selection would influence a project's architecture or performance.
> For high-performance needs, choosing a server like WildFly for its clustering might suit distributed apps, but switching to a lighter runtime like OpenLiberty could improve startup times for microservices; unlike [[spring|Spring]]'s consistent embedded Tomcat, this requires evaluating server-specific trade-offs in memory, scaling, and feature sets.
