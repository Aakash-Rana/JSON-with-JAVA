#### Pom.XML
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>packt.javaee</groupId>
    <artifactId>json</artifactId>
    <version>1.0-SNAPSHOT</version>
    <name>JSON with Java EE: Hands on Training</name>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <jsonp.version>1.1.2</jsonp.version>
    </properties>

    <dependencies>
        <!-- JSON-P API -->
        <dependency>
            <groupId>javax.json</groupId>
            <artifactId>javax.json-api</artifactId>
            <version>${jsonp.version}</version>
        </dependency>

        <!-- JSON-P RI -->
        <dependency>
            <groupId>org.glassfish</groupId>
            <artifactId>javax.json</artifactId>
            <version>${jsonp.version}</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.7.0</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>

            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>1.6.0</version>
                <configuration>
                    <executable>java</executable>
                    <arguments>
                        <argument>-classpath</argument>
                        <!-- automatically creates the classpath using all project dependencies,
                             also adding the project build directory -->
                        <classpath/>
                        <argument>packt.javaee.json.Main</argument>
                    </arguments>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```