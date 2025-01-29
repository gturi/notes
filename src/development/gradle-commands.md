---
tags:
  - development
  - gradle
  - java
---

# Useful gradle commands

## Update gradle wrapper

```bash
./gradlew wrapper --gradle-version=8.9
```

## Run spring-boot with active profile

```bash
./gradlew bootRun --args='--spring.profiles.active=dev'
```
