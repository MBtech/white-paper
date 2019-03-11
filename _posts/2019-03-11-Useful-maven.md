---
layout: post
title: "Maven tidbits"
categories:
  - cheatsheet
tags:
  - cheatsheet
---
# Useful maven tidbits
Updated on: 11-03-2019
## Using a local version of the dependency
Sometimes you just need to use a local version of the dependency rather than fetching it from the
maven central repository. You can add a local repository by providing the path to the local repository folder by adding the following to `pom.xml`a:
```xml
<repositories>
        <repository>
            <id>LocalRepoName</id>
            <url>file://pathtorepo</url>
        </repository>
    </repositories>
```
### Building a specific module
If you want to build a single module 'B' and the module required by 'B' then you can use the following command:

```bash
mvn install -pl B -am
```

## Working with SNAPSHOT artifacts of an Apache project
SNAPSHOT artifacts from Apache have dedicated Maven repository. You would need to add the information about the following repository to get them through maven. Add the following to `pom.xml`:
```xml
<repositories>
    <repository>
      <id>apache.snapshots</id>
      <name>Apache Development Snapshot Repository</name>
      <url>https://repository.apache.org/content/repositories/snapshots/</url>
      <releases><enabled>false</enabled></releases>
      <snapshots><enabled>true</enabled></snapshots>
   </repository>
</repositories>
```

## Adding target bytecode version info
In specific situation IntelliJ IDEA causes the following error with Maven, `Error:java: javacTask: source release 8 requires target release 1.8`. You can resolve that error by adding the compiler plugin to pom.xml under the top-level project node:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
        </plugin>
    </plugins>
</build>
```

## Disable checkstyle
While not recommended, in some cases you might be look to do some quick modifications and test them for a maven project.
You can disable the checkstyle plugin Using
```mvn [target] -Dcheckstyle.skip```

**References:**

1.  http://jojovedder.blogspot.com/2009/04/running-maven-offline-using-local.html
2. https://stackoverflow.com/questions/1114026/maven-modules-building-a-single-specific-module
3. https://stackoverflow.com/questions/39702621/how-to-import-apache-flink-snapshot-artifacts
4. https://stackoverflow.com/questions/29888592/errorjava-javactask-source-release-8-requires-target-release-1-8
5. https://stackoverflow.com/questions/32308188/disable-maven-checkstyle
