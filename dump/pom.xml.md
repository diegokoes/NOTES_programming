# JAVA -> MAVEN -> POM.XML
## SUMMARY
> The **Project Object Model (POM)** file (`pom.xml`) is the core configuration file for Maven projects. It defines project metadata, dependencies, build settings, plugins, and other configurations in an XML format. The POM follows a hierarchical structure, inheriting configurations from parent POMs (including the implicit **Super POM**), and supports modular projects through `<modules>`. Key elements include project coordinates (`groupId`, `artifactId`, `version`), dependencies, build plugins, repositories, and profiles.
## THEORY

**POM File Structure**  
Below is a detailed breakdown of the POM’s structure and key elements:  

**1. Root Element**  
```xml  
<project xmlns="http://maven.apache.org/POM/4.0.0"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
  <!-- All other elements are nested here -->  
</project>  
```  
- **Required**: Must declare the XML schema for validation.  

---

**2. Basic Project Metadata**  
```xml  
<modelVersion>4.0.0</modelVersion>  
<groupId>com.example</groupId>  
<artifactId>my-project</artifactId>  
<version>1.0.0</version>  
<packaging>jar</packaging>  
```  
- **`modelVersion`**: Always `4.0.0` (defines POM schema version).  
- **Coordinates**:  
  - **`groupId`**: Organization/group identifier (e.g., `com.example`).  
  - **`artifactId`**: Project/module name (e.g., `my-project`).  
  - **`version`**: Project version (e.g., `1.0.0`).  
- **`packaging`**: Output format (`jar`, `war`, `pom`, etc.). Default: `jar`.  

---

**3. Parent POM (Inheritance)**  
```xml  
<parent>  
  <groupId>com.example</groupId>  
  <artifactId>parent-project</artifactId>  
  <version>2.0.0</version>  
  <relativePath>../parent/pom.xml</relativePath>  
</parent>  
```  
- Inherits configurations (dependencies, plugins, etc.) from a parent POM.  
- **`relativePath`**: Path to the parent POM (optional).  

---

**4. Dependencies**  
```xml  
<dependencies>  
  <dependency>  
    <groupId>junit</groupId>  
    <artifactId>junit</artifactId>  
    <version>4.13.2</version>  
    <scope>test</scope>  
  </dependency>  
</dependencies>  
```  
- **`dependency`**: Declares libraries the project depends on.  
- **`scope`**: Dependency usage context (e.g., `compile`, `test`, `runtime`, `provided`).  

---

**5. Dependency Management**  
```xml  
<dependencyManagement>  
  <dependencies>  
    <dependency>  
      <groupId>org.springframework</groupId>  
      <artifactId>spring-core</artifactId>  
      <version>5.3.10</version>  
    </dependency>  
  </dependencies>  
</dependencyManagement>  
```  
- Centralizes dependency versions (used in multi-module projects).  
- Does not add dependencies to the project; child modules must explicitly declare them.  

---

**6. Build Settings**  
```xml  
<build>  
  <plugins>  
    <plugin>  
      <groupId>org.apache.maven.plugins</groupId>  
      <artifactId>maven-compiler-plugin</artifactId>  
      <version>3.8.1</version>  
      <configuration>  
        <source>11</source>  
        <target>11</target>  
      </configuration>  
    </plugin>  
  </plugins>  
  <resources>  
    <resource>  
      <directory>src/main/resources</directory>  
      <filtering>true</filtering>  
    </resource>  
  </resources>  
</build>  
```  
- **`plugins`**: Configures build plugins (e.g., compiler, surefire, jar).  
- **`resources`**: Defines non-code files (e.g., configs, templates).  

---

**7. Properties**  
```xml  
<properties>  
  <java.version>11</java.version>  
  <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>  
</properties>  
```  
- Reusable variables to avoid hardcoding values (e.g., versions, encoding).  

---

**8. Repositories**  
```xml  
<repositories>  
  <repository>  
    <id>my-repo</id>  
    <url>https://repo.example.com/maven2</url>  
  </repository>  
</repositories>  
```  
- Specifies remote repositories to download dependencies from (besides Maven Central).  

---

**9. Profiles**  
```xml  
<profiles>  
  <profile>  
    <id>prod</id>  
    <properties>  
      <environment>production</environment>  
    </properties>  
  </profile>  
</profiles>  
```  
- Environment-specific configurations (e.g., dev, test, prod). Activated via `-Pprod`.  

---

**10. Modules (Multi-Module Projects)**  
```xml  
<modules>  
  <module>module-1</module>  
  <module>module-2</module>  
</modules>  
```  
- Lists submodules to build as part of a parent project.  

## QUESTIONS
> [!tip]-  What is the minimum required information in a POM file?
> - `modelVersion`, `groupId`, `artifactId`, and `version`.  
> - Example:  
>  ```xml  
>  <project>  
>    <modelVersion>4.0.0</modelVersion>  
>   <groupId>com.example</groupId>  
>    <artifactId>my-app</artifactId>  
>    <version>1.0.0</version>  
>  </project> 
>   ```  

> [!tip]-  How do you set the Java version in Maven?
> Configure the `maven-compiler-plugin`:  
> ```xml  
><build>  
> <plugins>  
>    <plugin>  
>    <groupId>org.apache.maven.plugins</groupId>  
>      <artifactId>maven-compiler-plugin</artifactId>  
>      <configuration>  
>        <source>11</source>  
>        <target>11</target>  
>      </configuration>  
>    </plugin>  
>  </plugins>  
></build>  
>```  

> [!warning]- What is the difference between `<dependencies>` and `<dependencyManagement>`?
>- **`<dependencies>`**: Directly adds dependencies to the project.  
>- **`<dependencyManagement>`**: Centralizes versions for dependencies; child modules must explicitly declare them.  

> [!warning]- What is the purpose of the `<scope>` tag in a dependency? 
>Defines when a dependency is included:  
>- `compile` (default): Available in all phases.  
>- `test`: Only for test compilation and execution.  
>- `provided`: Provided by the runtime environment (e.g., servlet API in a WAR).  

> [!danger]- Question
> Answer



**Q5: How do you handle resource filtering?**  
**A5**: Use the `<resources>` section with `<filtering>true</filtering>` to replace placeholders in files (e.g., `${environment}`):  
```xml  
<build>  
  <resources>  
    <resource>  
      <directory>src/main/resources</directory>  
      <filtering>true</filtering>  
    </resource>  
  </resources>  
</build>  
```  

**Q6: What is the Super POM?**  
**A6**: The default parent POM for all Maven projects. It defines default configurations (e.g., directories, plugins). Use `mvn help:effective-pom` to view the merged POM.  

**Q7: How do you create a multi-module project?**  
**A7**:  
1. Create a parent POM with `<packaging>pom</packaging>`.  
2. List submodules in `<modules>`:  
   ```xml  
   <modules>  
     <module>module-1</module>  
     <module>module-2</module>  
   </modules>  
   ```  

**Q8: How do you override a plugin’s default behavior?**  
**A8**: Add the plugin to `<build><plugins>` and specify `<configuration>`:  
```xml  
<plugin>  
  <groupId>org.apache.maven.plugins</groupId>  
  <artifactId>maven-surefire-plugin</artifactId>  
  <configuration>  
    <skipTests>true</skipTests>  
  </configuration>  
</plugin>  
```  

**Q9: What is the `<properties>` section used for?**  
**A9**: To define reusable variables (e.g., versions, paths) to avoid duplication.  

**Q10: How do you activate a profile?**  
**A10**: Use `-P<profile-id>`:  
```bash  
mvn install -Pprod  
```  


This breakdown provides a comprehensive understanding of the POM structure, its key components, and practical use cases. 
- - - 
