# CONCEPTS
## TABLE OF CONTENTS

1. [[concepts#1. **Networking & Communication**|Networking & Communication]]
2. [[concepts#2. **Software Architecture**|Software Architecture]]
3. [[concepts#3. **Programming Paradigms**|Programming Paradigms]]
4. [[concepts#4. **Rendering Strategies**|Rendering Strategies]]
5. [[concepts#5. **Web Concepts & Optimization**|Web Concepts & Optimization]]
6. [[concepts#6. **Security Concepts**|Security Concepts]]
7. [[concepts#7. **DevOps & Deployment**|DevOps & Deployment]]


## 1. **NETWORKING & COMMUNICATION**

*Essential protocols and communication methods that enable software systems to interact effectively.*

### **APIS**

- [API (Application Programming Interface)](api.md): Defines methods for interacting with software components.
- [REST (Representational State Transfer)](REST.md): An architectural style for designing networked applications.
- [RESTful Services](restful.md): Services adhering to REST principles.
- [SOAP (Simple Object Access Protocol)](soap.md): A protocol for exchanging structured information in web services.
- [GraphQL](graphql.md): A query language for APIs and runtime for executing those queries.
- [WebSockets](websockets.md): Enables two-way interactive communication sessions.
- [gRPC (Google Remote Procedure Call)](grpc.md): A high-performance RPC framework.

### **PROTOCOLS**

- [HTTP/2](http2.md): The second major version of the HTTP protocol.
- [HTTP/3](http3.md): The third major version, utilizing QUIC for improved performance.
- [FTP (File Transfer Protocol)](ftp.md): Standard network protocol for transferring files.
- [SMTP (Simple Mail Transfer Protocol)](smtp.md): Protocol for email transmission.
- [DNS (Domain Name System)](dns.md): Translates domain names to IP addresses.
- [TLS/SSL (Transport Layer Security)](tls_ssl.md): Secures communications over computer networks.

## 2. **SOFTWARE ARCHITECTURE**

*Architectural patterns that define the structure and organization of software systems.*

- [MVC (Model-View-Controller)](mvc.md): Architectural pattern separating application logic.
- [SPA (Single-Page Application)](SPA.md): Web applications that load a single HTML page.
- [MPA (Multi-Page Application)](MPA.md): Traditional web applications with multiple pages.
- [Microservices Architecture](microservices.md): Structures an application as a collection of services.
- [Monolithic Architecture](monolithic.md): A single unified software structure.
- [Event-Driven Architecture](event-driven.md): Systems that respond to events or changes.

## 3. **PROGRAMMING PARADIGMS**

*Different styles and methodologies used in programming to solve problems.*

### **PROCEDURAL PROGRAMMING**

*A paradigm based on the concept of procedure calls and structured programming principles.*

- [Structured Programming](structured.md): Organizing code with control structures and subroutines.
- [Modular Programming](modular.md): Breaking programs into separate modules or functions.
- [Top-Down Design](top_down.md): Decomposing problems from general to specific.

### **OBJECT-ORIENTED PROGRAMMING (OOP)**

*Key principles and concepts that facilitate modular and reusable code.*

- [SOLID Principles](solid.md): Five design principles for maintainable software.
- [Encapsulation](encapsulation.md): Bundling data with methods that operate on it.
- [Abstraction](abstraction.md): Simplifying complex reality by modeling classes.
- [Polymorphism](polymorphism.md): Ability of different objects to respond to the same method.
- [Inheritance](inheritance.md): Mechanism where one class inherits properties from another.

### **[[concepts_Functional_Programming|Functional Programming]]**



### **DECLARATIVE PROGRAMMING**

*Programming paradigms that express the logic without describing control flow.*

- [Logic Programming](logic.md): Based on formal logic and rule-based reasoning.
- [Constraint Programming](constraint.md): Solving problems by stating constraints on solutions.
- [Domain-Specific Languages](dsl.md): Languages designed for specific problem domains.

### **REACTIVE PROGRAMMING**

*Programming with asynchronous data streams and the propagation of change.*

- [Event-Driven Programming](event_programming.md): Programs that respond to events or user actions.
- [Stream Processing](stream.md): Processing continuous streams of data.
- [Observer Pattern](observer_reactive.md): Reactive implementation of the observer pattern.

### **DESIGN PATTERNS**

*Reusable solutions to common software design problems.*

- [Factory Pattern](factory.md): Creates objects without specifying the exact class.
- [Singleton Pattern](singleton.md): Ensures a class has only one instance.
- [Builder Pattern](builder.md): Constructs complex objects step by step.
- [Observer Pattern](observer.md): Defines a one-to-many dependency between objects.
- [Decorator Pattern](decorator.md): Adds behavior to objects dynamically.
- [Strategy Pattern](strategy.md): Defines a family of algorithms and makes them interchangeable.
- [Adapter Pattern](adapter.md): Allows incompatible interfaces to work together.
- [Command Pattern](command.md): Encapsulates a request as an object.
- [Proxy Pattern](proxy.md): Provides a surrogate or placeholder for another object.

## 4. **RENDERING STRATEGIES**

*Techniques for rendering web pages efficiently and effectively.*

- [CSR (Client-Side Rendering)](csr.md): Rendering done in the browser.
- [SSR (Server-Side Rendering)](ssr.md): Rendering done on the server.
- [SSG (Static Site Generation)](ssg.md): Pre-rendering pages at build time.
- [ISR (Incremental Static Regeneration)](isr.md): Updating static content incrementally.
- [Streaming Rendering](streaming.md): Sending content to the client in chunks.
- [Progressive Hydration](progressive_hydration.md): Hydrating parts of the page progressively.

## 5. **WEB CONCEPTS & OPTIMIZATION**

*Techniques and practices to enhance web performance and user experience.*
- [[html_DOM|DOM (Document Object Model)]]
- [CORS (Cross-Origin Resource Sharing)](CORS.md): Controls access to resources from different origins.
- [Lazy Loading](LazyLoad.md): Defers loading of non-critical resources.
- [HTTP](HTTP.md): Foundation of data communication on the web.
- [Caching](caching.md): Storing data for faster future access.
- [Content Delivery Network (CDN)](cdn.md): Distributes content across multiple servers.
- [Minification](minification.md): Reducing file size by removing unnecessary characters.
- [Critical Rendering Path](critical_rendering_path.md): Sequence of steps the browser takes to render a page.

## 6. **SECURITY CONCEPTS**

*Fundamental security measures and protocols to protect applications.*

- [OAuth](oauth.md): Authorization framework enabling third-party access.
- [JWT (JSON Web Token)](jwt.md): Compact, URL-safe means of representing claims.
- [CSRF (Cross-Site Request Forgery)](csrf.md): Attacks that trick users into executing unwanted actions.
- [XSS (Cross-Site Scripting)](xss.md): Injecting malicious scripts into content.
- [Encryption](encryption.md): Securing data by encoding it.
- [Hashing](hashing.md): Transforming data into a fixed-size string of characters.

## 7. **DEVOPS & DEPLOYMENT**

*Tools and practices that facilitate continuous integration, continuous deployment, and overall software delivery processes.*

### **CI/CD TOOLS**

- [Jenkins](jenkins.md): An open-source automation server for building, testing, and deploying applications.
- [GitLab CI](gitlab_ci.md): Integrated CI/CD pipelines within GitLab.
- [Travis CI](travis_ci.md): A hosted CI service used to build and test software projects hosted on GitHub.
- [CircleCI](circleci.md): A CI/CD tool that automates development processes quickly, safely, and at scale.
- - - 
#conceptsprotocols 