# JAVA -> MAVEN
## SUMMARY
> [!summary]
> Is a tool that streamlines building and managing Java projects. simplifies the build process by standardizing project structure, managing dependencies, and automating tasks like compilation, testing, packaging, and deployment. 
> 
> Maven uses an XML file ([[pom.xml]]) to define project configurations, dependencies, and plugins. Its key principles include **convention over configuration**, dependency management via central repositories, and a extensible plugin-based architecture.
## THEORY
### **MAVEN LIFECYCLE PHASES IN VS CODE**

These phases define the **build lifecycle** in Maven. When executed in VS Code (via the Maven sidebar or command palette), they trigger plugins to perform specific tasks. Phases are executed in sequence, and running a later phase automatically runs all preceding phases in the lifecycle.

**1. `clean`**

- **Function**: Deletes the `target` directory (removes compiled classes, JARs, etc.).
    
- **Command**: `mvn clean`
    
- **Use Case**: Start fresh to avoid stale build artifacts.
    

**2. `validate`**

- **Function**: Validates the project structure and `pom.xml` for correctness.
    
- **Command**: `mvn validate`
    
- **Use Case**: Verify project setup before proceeding.
    

**3. `compile`**

- **Function**: Compiles the project’s source code (from `src/main/java`) into the `target/classes` directory.
    
- **Command**: `mvn compile`
    
- **Use Case**: Check for compilation errors.
    

**4. `test-compile`**

- **Function**: Compiles test code (from `src/test/java`) into `target/test-classes`.
    
- **Command**: `mvn test-compile`
    
- **Use Case**: Prepare test classes without running tests.
    

**5. `test`**

- **Function**: Runs unit tests (using `maven-surefire-plugin`).
    
- **Command**: `mvn test`
    
- **Use Case**: Verify code functionality with tests.
    

**6. `package`**

- **Function**: Packages compiled code into a distributable format (JAR, WAR, etc.) in the `target` directory.
    
- **Command**: `mvn package`
    
- **Use Case**: Create a deployable artifact.
    

**7. `verify`**

- **Function**: Runs integration tests (via `maven-failsafe-plugin`) and checks if the package meets quality criteria.
    
- **Command**: `mvn verify`
    
- **Use Case**: Ensure the artifact is ready for deployment.
    

**8. `install`**

- **Function**: Installs the built artifact (JAR/WAR) into the **local Maven repository** (`~/.m2/repository`).
    
- **Command**: `mvn install`
    
- **Use Case**: Share the artifact with other local projects.
    

**9. `site`**

- **Function**: Generates project documentation (reports, Javadoc, etc.) in `target/site`.
    
- **Command**: `mvn site`
    
- **Use Case**: Create developer-facing documentation.
    

**10. `deploy`**

- **Function**: Deploys the artifact to a **remote repository** (e.g., Nexus, Artifactory) for sharing across teams.
    
- **Command**: `mvn deploy`
    
- **Use Case**: Publish the artifact for collaboration.

## QUESTIONS
> [!tip]- How does Maven resolve dependencies?
> Maven searches for dependencies in the **local repository** first. If not found, it downloads them from **remote repositories** (e.g., Maven Central) and stores them locally.

> [!warning]- What is the difference between Maven and Ant?
>- **Maven**: Declarative, lifecycle-driven, and convention-based.
>    
>- **Ant**: Procedural, requires explicit task definitions (e.g., writing `build.xml`).

> [!danger]- How do you add a dependency in Maven?
> Add the dependency’s `groupId`, `artifactId`, and `version` to the `<dependencies>` section of `pom.xml`:
> ```xml
><dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.13.2</version>
  <scope>test</scope>
</dependency>
>```
- - - 
#java 