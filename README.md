# cyclonedx-maven-demo
CycloneDX SBom (Software Bill of material) Maven Demo

Nowadays securing the software supply chain is a very important aspect of the software development and delivery ecosystem.

[CycloneDX](https://cyclonedx.org) is a software bill of material format supported by [OWASP](https://owasp.org).

CycloneDX is a very lightweight SBOM, which represents all direct and transitive dependencies added to Maven pom.xml file. 

CycloneDX provides various tool sets to generate SBOM from many different programing language projects. ie. Java, Python, Node, etc. Ref. [CycloneDX Tools ecosystem](https://cyclonedx.org/tool-center/)

This sample project is using Maven build system for generating artifacts. [cyclonedx-maven-plugin](https://github.com/CycloneDX/cyclonedx-maven-plugin) is used for generating CycloneDX SBom file.

CycloneDX SBOM file can be used for project vulnerability analysis using the OWASP [Dependency Track](https://dependencytrack.org/)](https://dependencytrack.org/) application

Sample cyclonedx-maven-plugin configuration.

```xml
<plugin>
    <groupId>org.cyclonedx</groupId>
    <artifactId>cyclonedx-maven-plugin</artifactId>
    <version>2.7.0</version>
    <configuration>
        <projectType>library</projectType>
        <schemaVersion>1.3</schemaVersion>
        <includeBomSerialNumber>true</includeBomSerialNumber>
        <includeCompileScope>true</includeCompileScope>
        <includeProvidedScope>true</includeProvidedScope>
        <includeRuntimeScope>true</includeRuntimeScope>
        <includeSystemScope>true</includeSystemScope>
        <includeTestScope>false</includeTestScope>
        <includeLicenseText>false</includeLicenseText>
        <outputFormat>all</outputFormat>
    </configuration>
    <executions>
        <execution>
            <phase>package</phase>
            <goals>
                <goal>makeAggregateBom</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

Execution of Maven build command would generate SBOM files in target folder with name bom.json and bom.xml
```bash
mvn clean install
```
Maven build output
```
[INFO] --- cyclonedx-maven-plugin:2.7.0:makeAggregateBom (default) @ cyclonedx-maven-demo ---
[INFO] CycloneDX: Parameters
[INFO] ------------------------------------------------------------------------
[INFO] schemaVersion          : 1.3
[INFO] includeBomSerialNumber : true
[INFO] includeCompileScope    : true
[INFO] includeProvidedScope   : true
[INFO] includeRuntimeScope    : true
[INFO] includeTestScope       : false
[INFO] includeSystemScope     : true
[INFO] includeLicenseText     : false
[INFO] outputReactorProjects  : true
[INFO] outputFormat           : all
[INFO] outputName             : bom
[INFO] ------------------------------------------------------------------------
[INFO] CycloneDX: Creating BOM
[INFO] CycloneDX: Writing BOM (XML): /home/ravi.soni/git/maven/cyclonedx-maven-demo/target/bom.xml
[INFO] CycloneDX: Validating BOM (XML): /home/ravi.soni/git/maven/cyclonedx-maven-demo/target/bom.xml
[INFO] CycloneDX: Writing BOM (JSON): /home/ravi.soni/git/maven/cyclonedx-maven-demo/target/bom.json
[INFO] CycloneDX: Validating BOM (JSON): /home/ravi.soni/git/maven/cyclonedx-maven-demo/target/bom.json
```
Check CycloneDX SBOM files in a target folder    
```
[ravi.soni@GB8TRF2 cyclonedx-maven-demo] $ ls -l target/
total 17380
-rw-rw-r-- 1 ravi.soni ravi.soni    81647 Jul 14 16:05 bom.json
-rw-rw-r-- 1 ravi.soni ravi.soni    69804 Jul 14 16:05 bom.xml
drwxrwxr-x 3 ravi.soni ravi.soni     4096 Jul 14 16:05 classes
-rw-rw-r-- 1 ravi.soni ravi.soni 17619933 Jul 14 16:05 cyclonedx-maven-demo-0.0.1-SNAPSHOT.jar
-rw-rw-r-- 1 ravi.soni ravi.soni     3247 Jul 14 16:05 cyclonedx-maven-demo-0.0.1-SNAPSHOT.jar.original
drwxrwxr-x 3 ravi.soni ravi.soni     4096 Jul 14 16:05 generated-sources
drwxrwxr-x 2 ravi.soni ravi.soni     4096 Jul 14 16:05 maven-archiver
drwxrwxr-x 3 ravi.soni ravi.soni     4096 Jul 14 16:05 maven-status
```

The best way to verify all dependencies (direct and transitive) of the project is to run a Maven command and print on the console.

```
mvn dependency:tree
```
Output a dependency tree of a project.
```
[INFO] --- maven-dependency-plugin:3.3.0:tree (default-cli) @ cyclonedx-maven-demo ---
[INFO] com.rvsoni.maven:cyclonedx-maven-demo:jar:0.0.1-SNAPSHOT
[INFO] +- org.springframework.boot:spring-boot-starter-web:jar:2.7.1:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter:jar:2.7.1:compile
[INFO] |  |  +- org.springframework.boot:spring-boot:jar:2.7.1:compile
[INFO] |  |  +- org.springframework.boot:spring-boot-autoconfigure:jar:2.7.1:compile
[INFO] |  |  +- org.springframework.boot:spring-boot-starter-logging:jar:2.7.1:compile
[INFO] |  |  |  +- ch.qos.logback:logback-classic:jar:1.2.11:compile
[INFO] |  |  |  |  \- ch.qos.logback:logback-core:jar:1.2.11:compile
[INFO] |  |  |  +- org.apache.logging.log4j:log4j-to-slf4j:jar:2.17.2:compile
[INFO] |  |  |  |  \- org.apache.logging.log4j:log4j-api:jar:2.17.2:compile
[INFO] |  |  |  \- org.slf4j:jul-to-slf4j:jar:1.7.36:compile
[INFO] |  |  +- jakarta.annotation:jakarta.annotation-api:jar:1.3.5:compile
[INFO] |  |  \- org.yaml:snakeyaml:jar:1.30:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter-json:jar:2.7.1:compile
[INFO] |  |  +- com.fasterxml.jackson.core:jackson-databind:jar:2.13.3:compile
[INFO] |  |  |  +- com.fasterxml.jackson.core:jackson-annotations:jar:2.13.3:compile
[INFO] |  |  |  \- com.fasterxml.jackson.core:jackson-core:jar:2.13.3:compile
[INFO] |  |  +- com.fasterxml.jackson.datatype:jackson-datatype-jdk8:jar:2.13.3:compile
[INFO] |  |  +- com.fasterxml.jackson.datatype:jackson-datatype-jsr310:jar:2.13.3:compile
[INFO] |  |  \- com.fasterxml.jackson.module:jackson-module-parameter-names:jar:2.13.3:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter-tomcat:jar:2.7.1:compile
[INFO] |  |  +- org.apache.tomcat.embed:tomcat-embed-core:jar:9.0.64:compile
[INFO] |  |  +- org.apache.tomcat.embed:tomcat-embed-el:jar:9.0.64:compile
[INFO] |  |  \- org.apache.tomcat.embed:tomcat-embed-websocket:jar:9.0.64:compile
[INFO] |  +- org.springframework:spring-web:jar:5.3.21:compile
[INFO] |  |  \- org.springframework:spring-beans:jar:5.3.21:compile
[INFO] |  \- org.springframework:spring-webmvc:jar:5.3.21:compile
[INFO] |     +- org.springframework:spring-aop:jar:5.3.21:compile
[INFO] |     +- org.springframework:spring-context:jar:5.3.21:compile
[INFO] |     \- org.springframework:spring-expression:jar:5.3.21:compile
[INFO] \- org.springframework.boot:spring-boot-starter-test:jar:2.7.1:test
[INFO]    +- org.springframework.boot:spring-boot-test:jar:2.7.1:test
[INFO]    +- org.springframework.boot:spring-boot-test-autoconfigure:jar:2.7.1:test
[INFO]    +- com.jayway.jsonpath:json-path:jar:2.7.0:test
[INFO]    |  +- net.minidev:json-smart:jar:2.4.8:test
[INFO]    |  |  \- net.minidev:accessors-smart:jar:2.4.8:test
[INFO]    |  |     \- org.ow2.asm:asm:jar:9.1:test
[INFO]    |  \- org.slf4j:slf4j-api:jar:1.7.36:compile
[INFO]    +- jakarta.xml.bind:jakarta.xml.bind-api:jar:2.3.3:test
[INFO]    |  \- jakarta.activation:jakarta.activation-api:jar:1.2.2:test
[INFO]    +- org.assertj:assertj-core:jar:3.22.0:test
[INFO]    +- org.hamcrest:hamcrest:jar:2.2:test
[INFO]    +- org.junit.jupiter:junit-jupiter:jar:5.8.2:test
[INFO]    |  +- org.junit.jupiter:junit-jupiter-api:jar:5.8.2:test
[INFO]    |  |  +- org.opentest4j:opentest4j:jar:1.2.0:test
[INFO]    |  |  +- org.junit.platform:junit-platform-commons:jar:1.8.2:test
[INFO]    |  |  \- org.apiguardian:apiguardian-api:jar:1.1.2:test
[INFO]    |  +- org.junit.jupiter:junit-jupiter-params:jar:5.8.2:test
[INFO]    |  \- org.junit.jupiter:junit-jupiter-engine:jar:5.8.2:test
[INFO]    |     \- org.junit.platform:junit-platform-engine:jar:1.8.2:test
[INFO]    +- org.mockito:mockito-core:jar:4.5.1:test
[INFO]    |  +- net.bytebuddy:byte-buddy:jar:1.12.11:test
[INFO]    |  +- net.bytebuddy:byte-buddy-agent:jar:1.12.11:test
[INFO]    |  \- org.objenesis:objenesis:jar:3.2:test
[INFO]    +- org.mockito:mockito-junit-jupiter:jar:4.5.1:test
[INFO]    +- org.skyscreamer:jsonassert:jar:1.5.0:test
[INFO]    |  \- com.vaadin.external.google:android-json:jar:0.0.20131108.vaadin1:test
[INFO]    +- org.springframework:spring-core:jar:5.3.21:compile
[INFO]    |  \- org.springframework:spring-jcl:jar:5.3.21:compile
[INFO]    +- org.springframework:spring-test:jar:5.3.21:test
[INFO]    \- org.xmlunit:xmlunit-core:jar:2.9.0:test
```

Once a project is built run a ```jq``` command to print the same depencency information from bom.json file.

```
jq '.components[]| "\(.group)/\(.name)@\(.version)" ' target/bom.json 
```
Output

```
[ravi.soni@GB8TRF2 cyclonedx-maven-demo] $ jq '.components[]| "\(.group)/\(.name)@\(.version)" ' target/bom.json 
"org.springframework.boot/spring-boot-starter-web@2.7.1"
"org.springframework.boot/spring-boot-starter@2.7.1"
"org.springframework.boot/spring-boot@2.7.1"
"org.springframework.boot/spring-boot-autoconfigure@2.7.1"
"org.springframework.boot/spring-boot-starter-logging@2.7.1"
"ch.qos.logback/logback-classic@1.2.11"
"ch.qos.logback/logback-core@1.2.11"
"org.apache.logging.log4j/log4j-to-slf4j@2.17.2"
"org.apache.logging.log4j/log4j-api@2.17.2"
"org.slf4j/jul-to-slf4j@1.7.36"
"jakarta.annotation/jakarta.annotation-api@1.3.5"
"org.yaml/snakeyaml@1.30"
"org.springframework.boot/spring-boot-starter-json@2.7.1"
"com.fasterxml.jackson.core/jackson-databind@2.13.3"
"com.fasterxml.jackson.core/jackson-annotations@2.13.3"
"com.fasterxml.jackson.core/jackson-core@2.13.3"
"com.fasterxml.jackson.datatype/jackson-datatype-jdk8@2.13.3"
"com.fasterxml.jackson.datatype/jackson-datatype-jsr310@2.13.3"
"com.fasterxml.jackson.module/jackson-module-parameter-names@2.13.3"
"org.springframework.boot/spring-boot-starter-tomcat@2.7.1"
"org.apache.tomcat.embed/tomcat-embed-core@9.0.64"
"org.apache.tomcat.embed/tomcat-embed-el@9.0.64"
"org.apache.tomcat.embed/tomcat-embed-websocket@9.0.64"
"org.springframework/spring-web@5.3.21"
"org.springframework/spring-beans@5.3.21"
"org.springframework/spring-webmvc@5.3.21"
"org.springframework/spring-aop@5.3.21"
"org.springframework/spring-context@5.3.21"
"org.springframework/spring-expression@5.3.21"
"org.slf4j/slf4j-api@1.7.36"
"org.springframework/spring-core@5.3.21"
"org.springframework/spring-jcl@5.3.21"
```