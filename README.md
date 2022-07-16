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
Check SBOM files in a target folder
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