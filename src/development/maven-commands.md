---
tags:
  - development
  - maven
  - java
---

# Useful maven commands

## Specify jdk to use for running maven command

Useful if you do not want to have to setup java/maven globally.

```bash
function mvn() {
   MVN_HOME=$HOME/Programs/apache-maven-3.6.3/bin/mvn
   JAVA_HOME=$HOME/.jdks/corretto-1.8.0_342 "$MVN_HOME" "$1"
}
```

## Override java properties

```bash
mvn spring-boot:run \
  -Dspring.profiles.active=debug \
  -Denvironment.endpoint=https://my.company.dev.endpoint.com \
  -Dserver.port=3000
```

## Test utilities

```bash
# -DforkCount=0 allows to attach debugger in IntelliJ
# -Dsurefire.useFile=false
# -DtrimStackTrace=false this avoids stacktrace trimming

# run every test
mvn test -DforkCount=0 -Dsurefire.useFile=false -DtrimStackTrace=false
# run test in a class
mvn test -Dtest=ClassToTest -DforkCount=0 -Dsurefire.useFile=false -DtrimStackTrace=false
# run specific test in a class
mvn test -Dtest=ClassToTest#methodToTest -DforkCount=0 -Dsurefire.useFile=false -DtrimStackTrace=false
```

## See what the final pom will be when inheriting from parent

```bash
mvn help:effective-pom
```

## Show dependency tree

```bash
# useful to discover possible classpath clash errors
mvn dependency:tree -Dverbose
```

## Upgrade maven wrapper

```bash
mvn wrapper:wrapper -Dmaven=3.9.5
```
