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

Maven automatically downloads this dependency and its required **transitive dependencies**.

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

**Important:** When you run a later phase, Maven executes the earlier phases too.

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
- Performs additional configured checks/verification
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

### One-line definition

> **Maven is a Java build and dependency management tool that uses `pom.xml` to manage a project's dependencies, build lifecycle, plugins, and packaging.**
