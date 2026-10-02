[ChatGPT](https://chatgpt.com/share/6ab74837-0f4c-83ee-a23a-a82bfb04814a)

Here’s a compact note you can keep for your Maven/Spring Boot learning.

# Maven `package` vs `verify`

## 1. Maven Lifecycle

Maven follows a sequence of lifecycle phases:

```text
validate
   ↓
compile
   ↓
test
   ↓
package
   ↓
verify
   ↓
install
   ↓
deploy
```

When you run a later phase, Maven automatically executes the earlier phases first.

For example:

```bash
mvn verify
```

will execute:

```text
validate → compile → test → package → verify
```

---

## 2. `mvn package`

The `package` phase creates the project's distributable artifact.

For a typical Java/Spring Boot application, this is a JAR:

```bash
mvn package
```

After execution, you may see:

```text
target/
├── classes/
├── test-classes/
└── myapp-1.0.0.jar
```

### Key point

> **`package` = create the JAR/WAR artifact.**

---

## 3. `mvn verify`

`verify` is a lifecycle phase that comes **after `package`**.

When you run:

```bash
mvn verify
```

Maven does:

```text
compile
   ↓
test
   ↓
package
   ↓
JAR created
   ↓
verify
   ↓
additional checks
```

Therefore, `mvn verify` also results in a JAR being created because Maven passes through the `package` phase first.

However:

> **The `verify` phase itself normally does not create another JAR.**

It performs additional verification/checking configured in the `pom.xml`.

---

## 4. How `pom.xml` controls `verify`

Plugins can be configured to execute during the `verify` phase.

Example:

```xml
<execution>
    <id>check-coverage</id>

    <phase>verify</phase>

    <goals>
        <goal>check</goal>
    </goals>
</execution>
```

This tells Maven:

> When you reach the `verify` phase, execute this plugin's `check` goal.

Conceptually:

```text
mvn verify
    │
    ├── package
    │     └── JAR created
    │
    └── verify
          └── plugin checks
```

---

## 5. What can `verify` check?

It depends on the plugins configured in `pom.xml`.

Examples:

```text
JaCoCo      → code coverage checks
Checkstyle  → coding-style checks
Failsafe    → integration tests
Custom      → project-specific validation
```

For example, if JaCoCo requires 80% code coverage:

```text
Actual coverage = 65%
Required        = 80%

65% < 80%
     ↓
BUILD FAILURE
```

So:

```bash
mvn verify
```

can fail even though compilation and unit tests succeeded.

---

## 6. Phase vs Goal

This distinction is important.

### Phase

A phase is a stage in the Maven lifecycle:

```text
compile
test
package
verify
install
deploy
```

### Goal

A goal is a specific operation provided by a Maven plugin:

```text
compiler:compile
surefire:test
jar:jar
jacoco:check
```

A configuration such as:

```xml
<phase>verify</phase>

<goals>
    <goal>check</goal>
</goals>
```

means:

```text
Maven reaches verify phase
        ↓
Execute the plugin's check goal
```

---

# 7. `package` vs `verify`

| Command | What happens |
|---|---|
| `mvn package` | Builds the project and creates the JAR/WAR |
| `mvn verify` | Does everything up to `package`, then performs additional verification |
| `mvn install` | Does everything up to `verify`, then installs the artifact into the local Maven repository |
| `mvn deploy` | Does everything up to `install`, then publishes the artifact to a remote repository |

### Simple mental model

```text
mvn package
      ↓
BUILD THE JAR


mvn verify
      ↓
BUILD THE JAR
      +
CHECK/VERIFY THE BUILD


mvn install
      ↓
BUILD THE JAR
      +
VERIFY
      +
PUT JAR IN LOCAL MAVEN REPOSITORY
```

---

# 8. Most Important Understanding

There is **no separate "package JAR" and "verify JAR."**

If you run:

```bash
mvn verify
```

the JAR is created when Maven reaches:

```text
package
```

Then Maven continues to:

```text
verify
```

and performs whatever additional checks are configured.

So remember:

> **`package` → creates the artifact.**

> **`verify` → performs additional validation after packaging.**

> **`mvn verify` automatically runs `package` first, so you don't need to run `mvn package` separately.**
