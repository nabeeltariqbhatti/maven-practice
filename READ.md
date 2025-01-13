# Maven Property Filtering with System and Command Line Properties

This project demonstrates how to reference system properties and pass properties through the command line using Maven's filtering process. Specifically, we will add properties to the `application.properties` file and pass properties dynamically via the command line.

## Table of Contents

- [Project Setup](#project-setup)
- [Step 1: Add Properties in `application.properties`](#step-1-add-properties-in-applicationproperties)
- [Step 2: Update `pom.xml` to Process Properties](#step-2-update-pomxml-to-process-properties)
- [Step 3: Pass Properties via Command Line](#step-3-pass-properties-via-command-line)
- [Step 4: Verify the Processed Properties](#step-4-verify-the-processed-properties)
- [Conclusion](#conclusion)

## Project Setup

To get started, clone this repository or set up a Maven project with the structure described below.


### Step 1: Add Properties in `application.properties`

First, create an `application.properties` file and define properties inside it. These properties will use Maven’s filtering feature to be replaced by system properties or command line properties during the build process.

Create the following file at `src/main/resources/META-INF/application.properties`:

```properties
# application.properties
java.version=${java.version}
command.line.prop=${command.line.prop}

```

### Step 2: Update `pom.xml` to Process Properties

Next, we need to configure Maven to filter the properties during the build. Update your `pom.xml` to ensure that the `application.properties` file is processed using the resources plugin.

Add the following configuration in your `pom.xml`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.mycompany.app</groupId>
    <artifactId>my-app</artifactId>
    <version>1.0-SNAPSHOT</version>

    <name>my-app</name>
    <!-- FIXME change it to the project's website -->
    <url>http://www.example.com</url>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.release>17</maven.compiler.release>
        <my.filter.value>test</my.filter.value>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.junit</groupId>
                <artifactId>junit-bom</artifactId>
                <version>5.11.0</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependencies>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-api</artifactId>
            <scope>test</scope>
        </dependency>
        <!-- Optionally: parameterized tests support -->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-params</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <filters>
            <filter>src/main/filters/filter.properties</filter>
        </filters>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <filtering>true</filtering>
            </resource>
        </resources>

        <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
            <plugins>
                <!-- clean lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#clean_Lifecycle -->
                <plugin>
                    <artifactId>maven-clean-plugin</artifactId>
                    <version>3.4.0</version>
                </plugin>
                <!-- default lifecycle, jar packaging: see https://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_jar_packaging -->
                <plugin>
                    <artifactId>maven-resources-plugin</artifactId>
                    <version>3.3.1</version>
                </plugin>
                <plugin>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.13.0</version>
                </plugin>
                <plugin>
                    <artifactId>maven-surefire-plugin</artifactId>
                    <version>3.3.0</version>
                </plugin>
                <plugin>
                    <artifactId>maven-jar-plugin</artifactId>
                    <version>3.4.2</version>
                </plugin>
                <plugin>
                    <artifactId>maven-install-plugin</artifactId>
                    <version>3.1.2</version>
                </plugin>
                <plugin>
                    <artifactId>maven-deploy-plugin</artifactId>
                    <version>3.1.2</version>
                </plugin>
                <!-- site lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#site_Lifecycle -->
                <plugin>
                    <artifactId>maven-site-plugin</artifactId>
                    <version>3.12.1</version>
                </plugin>
                <plugin>
                    <artifactId>maven-project-info-reports-plugin</artifactId>
                    <version>3.6.1</version>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
</project>

```
### Step 3: Pass Properties via Command Line

To pass properties dynamically at build time, use the `-D` flag in your Maven command. In this case, we will pass the property `command.line.prop` with the value `"hello again"`.

Run the following Maven command:

```bash
mvn process-resources "-Dcommand.line.prop=hello again"
