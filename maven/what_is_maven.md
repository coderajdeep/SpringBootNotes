[ChatGPT](https://chatgpt.com/share/6abf90da-befc-83e8-a02c-4a49a0c2b77b)

# Maven — Compact Note

### What is Maven?
**Maven** is a **build automation and dependency management tool for Java projects**.

It helps with:
- Managing dependencies
- Compiling Java code
- Running tests
- Packaging applications
- Verifying builds
- Installing and deploying artifacts

### `pom.xml`
Maven's main configuration file is **`pom.xml`**.

**POM = Project Object Model**

It contains:
- Project information
- Java/compiler configuration
- Dependencies
- Plugins
- Build configuration

Example:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

Maven downloads this dependency and its required **transitive dependencies**.

### Dependency Version

The `<version>` of a dependency can often be omitted when its version is already managed by a parent POM or `<dependencyManagement>`.

For Spring Boot:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

Spring Boot manages the compatible version.

If Maven doesn't have a managed version, you generally need:

```xml
<version>1.2.3</version>
```

You can check the effective configuration with:

```bash
mvn help:effective-pom
```

### Classpath

**Classpath = the list of locations where Java looks for classes and resources needed by an application.**

For example:

```text
target/classes/
spring-core.jar
spring-web.jar
mysql-driver.jar
```

When your code uses:

```java
import org.springframework.web.bind.annotation.RestController;
```

Java looks in the classpath to find `RestController`.

There are two important classpaths:

- **Compile-time classpath** → classes needed by `javac` while compiling.
- **Runtime classpath** → classes needed by the JVM while running.

Maven manages dependencies and constructs the appropriate classpath when compiling/running the project.

Example:

```bash
java -cp "target/classes;lib/*" com.example.Main
```

Here, `-cp` means **classpath**.

### Maven Lifecycle

Important phases:

```text
validate → compile → test → package → verify → install → deploy
```

Examples:

```bash
mvn compile
mvn test
mvn package
mvn verify
mvn clean package
```

When you run a later phase, Maven executes the earlier phases too.

For example:

```bash
mvn verify
```

runs:

```text
validate
→ compile
→ test
→ package
→ verify
```

### `package` vs `verify`

**`package`**
- Builds the application
- Creates the JAR/WAR

**`verify`**
- Runs the lifecycle through `verify`
- Uses the artifact produced during `package`
- Performs configured verification/checks
- Normally does **not create another JAR**

### Spring Boot + Maven

Typical project:

```text
demo/
├── pom.xml
├── src/
│   ├── main/
│   └── test/
└── target/
```

Build:

```bash
mvn clean package
```

Result:

```text
target/demo-1.0.0.jar
```

Run:

```bash
java -jar target/demo-1.0.0.jar
```

### Key Concepts

```text
Maven
  ↓
Reads pom.xml
  ↓
Downloads dependencies
  ↓
Builds the classpath
  ↓
Compiles code
  ↓
Runs tests
  ↓
Packages application
  ↓
Verifies / installs / deploys
```

### One-line definitions

> **Maven:** Java build automation and dependency management tool.

> **POM:** Maven's project configuration file (`pom.xml`).

> **Dependency:** A library that your project requires.

> **Classpath:** Locations/JARs from which Java finds required classes and resources.
