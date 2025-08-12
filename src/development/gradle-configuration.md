---
tags:
  - development
  - gradle
  - java
  - config
---

# Gradle configuration

## JavaCompile configuration

```groovy
tasks.withType(JavaCompile).configureEach {
    options.compilerArgs += ['-parameters']
}
```
