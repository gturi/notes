---
tags:
  - development
  - maven
  - java
  - config
---

# Maven compiler plugin configuration

Nice configuration of maven compiler plugin:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.12.1</version>
            <configuration>
                <source>${java.version}</source>
                <target>${java.version}</target>
                <release>${java.version}</release>
                <parameters>true</parameters>
                <annotationProcessorPaths>
                    <path>
                        <groupId>org.projectlombok</groupId>
                        <artifactId>lombok</artifactId>
                        <version>${lombok.version}</version>
                    </path>
                    <path>
                        <groupId>org.projectlombok</groupId>
                        <artifactId>lombok-mapstruct-binding</artifactId>
                        <version>0.2.0</version>
                    </path>
                    <path>
                        <groupId>org.mapstruct</groupId>
                        <artifactId>mapstruct-processor</artifactId>
                        <version>${mapstruct.version}</version>
                    </path>
                </annotationProcessorPaths>
            </configuration>
        </plugin>
    </plugins>
</build>
```

## Notes

### `<parameters>true</parameters>`

Stores formal parameter names of constructors and methods in the generated class file so that the method java.lang.reflect.Executable.getParameters from the Reflection API can retrieve them (https://docs.oracle.com/en/java/javase/17/docs/specs/man/javac.html#option-parameters).

This is pretty useful to reduce boilerplate code when designing APIs, if expected request names are the same as java method argument names.

#### Without `<parameters>true</parameters>`:

```java
@GetMapping(path = "/store/{storeId}/pet")
public ResponseEntity<List<Pet>> getStorePets(
    @PathVariable(name = "storeId") String storeId,
    @RequestParam(name = "petName") String petName) {
    // implementation
}
```

#### With `<parameters>true</parameters>`:

```java
@GetMapping(path = "/store/{storeId}/pet")
public ResponseEntity<List<Pet>> getStorePets(
    @PathVariable String storeId,
    @RequestParam String petName) {
    // implementation
}
```
