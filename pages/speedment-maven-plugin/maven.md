---
permalink: maven.html
sidebar: mydoc_sidebar
title: Speedment Maven Plugin
keywords: Maven, Plugin, Tool, Generate, Reload, Clear
toc: false
Tags: Installation
---

## Maven Targets

The Speedment Mavan Plugin has four Maven targets that can be used to simplify and/or automate our build process. These Maven targets 
can be used to read the meta data (e.g. tables and columns) from the database. It is also used to generate code that we can use in 
our applications. Therefore, before we can use Speedment, we must run at least one of the Maven targes.

## Installation

To install the Speedment Maven Plugin, we just add it as a plugin in our pom.xml file as described hereunder:

``` xml
    <build>
        <plugins>
            <plugin>
                <groupId>com.speedment</groupId>
                <artifactId>speedment-maven-plugin</artifactId>
                <version>${speedment.version}</version>
            </plugin>
        </plugins>
    </build>
```

Once the file has been saves, the new Maven targets are immediately available to our project.

N.B. Set the property ${speedment.version} to the latest Speedment version released, currently {{ site.data.speedment.version }}. A list
 of allversions of the Speedment Maven Plugin can be found 
[here](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.speedment%22%20AND%20a%3A%22speedment-maven-plugin%22).

The Speedment Maven Plugin automatically depends on relevant version of open-source JDBC database drivers. These dependencies can be overridden 
should we want to use another version. In the example below, we override the MySql JDBC version with an older one:

``` xml
    <build>
        <plugins>
            <plugin>
                <groupId>com.speedment</groupId>
                <artifactId>speedment-maven-plugin</artifactId>
                <version>${speedment.version}</version>
                <dependencies>
                    <dependency>
                        <groupId>mysql</groupId>
                        <artifactId>mysql-connector-java</artifactId>
                        <version>5.1.38</version>
                    </dependency>
                </dependencies> 
            </plugin>
        </plugins>
    </build>
```


## Installation Example

Here is an example of a complete POM file that is setup for a Speedment application that runs against a MySQL database:

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.comapny</groupId>
    <artifactId>test_speedment</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>jar</packaging>
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <speedment.version>{{ site.data.speedment.version }}</speedment.version>
    </properties>
    <build>
        <plugins>
            <plugin>
                <groupId>com.speedment</groupId>
                <artifactId>speedment-maven-plugin</artifactId>
                <version>${speedment.version}</version>
            </plugin>
        </plugins>
    </build>
    <dependencies>
        <dependency>
            <groupId>com.speedment</groupId>
            <artifactId>runtime</artifactId>
            <version>${speedment.version}</version>
            <type>pom</type>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.40</version>
        </dependency>
    </dependencies>
</project>
```


## Maven Targets
There are four Maven targets in the Speedment Maven Plugin:

| Target   | Purpose                                                         | Tool |
| :------- | :-------------------------------------------------------------- | :--- |
| tool     | Starts the graphical tool that connects to an existing database | Yes  |
| generate | Generates code                                                  | No   |
| reload   | Reloads meta data and updates the config file                   | No   |
| clear    | Removes all generated code                                      | No   |


### Tool (speedment:tool)
Starts the Speedment Graphical Tool that can be used to connect to the database and generate code. All settings are saved
in a JSON configuration file. Click [here] () to read more about the graphical tool.

### Generate
By using the `speedment:generate` target we can generated code directly from the JSON configuration file without connecting to the database and without starting the Tool. 

### Reload (speedment:reload)
Reloads the metadata from the database and merges any changes with the existing JSON configuration file without starting the Tool.

### Clear (speedment:clear)
Removes all the generated files from our project without starting the Tool. Files that are manually changed are protected by a cryptographic
hash code and will not be removed.


## Configuration

The Speedment Maven Plugin can be configured in many ways. New functionality can be added dynamically by adding `TypeMapper`s, `Component`s
and `Bundle`s.

### Adding a Type Mapper
`TypeMapper`s are used to map a database type to a Java type. For example, a `Timestamp` field can be mapped to the Java type `long` to
save memory and reduce the number of objects that are created during execution. `TypeMapper`s can be added to the Maven Targets dynamically 
and will then be available like any built-in `TypeMapper`.

``` xml
    <build>
        <plugin>
            <groupId>com.speedment</groupId>
            <artifactId>speedment-maven-plugin</artifactId>
            <version>${speedment.version}</version>
            <dependencies>
                <dependency>
                    <groupId>de.entwicklung</groupId>
                    <artifactId>typemapers</artifactId>
                    <version>1.0.0</version>
                </dependency>
            </dependencies> 
            <configuration>
                <components>
                    <component>de.entwicklung.typemapper.JaNeinStringToBooleanTypeMapper</component>
                </components>
            </configuration>
        </plugin>
    <build>
    <dependencies>
        <dependency>
            <groupId>com.speedment</groupId>
            <artifactId>runtime</artifactId>
            <version>${speedment.version}</version>
            <type>pom</type>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.40</version>
        </dependency>
        <dependency>
            <groupId>de.entwicklung</groupId>
            <artifactId>typemapers</artifactId>
            <version>1.0.0</version>
        </dependency>
    </dependencies>
```

This example show a fictive German `TypeMapper` that converts a `String` that is either "Ja" (Yes) or "Nein" (No) and maps that to
a `boolean` that is either `true` ("Ja") or `false` ("Nein"). 

N.B. `TypeMappers` that are added to the plugins must also be on the class path once our application run. Remember to depend on the artifact 
that contains the `TypeMapper` in your POM file as shown in the last part of the example above.

### Adding a Component
TBW

### Adding a Bundles
TBW


### Enable Debug Mode
If we want to follow more closely what is going on in the Speedment Maven Plugin, we can enable *Debug Mode*. In this mode, information about 
what classes are being initiated are shown in the standard logger together with other data. This makes it easier to trouble shoot 
problems and can provide valuable information in many cases.

Enable debug mode by adding a `<configuration><debug>true</debug></configuration>` element in the POM as described here:

``` xml
    <build>
        <plugins>
            <plugin>
                <groupId>com.speedment</groupId>
                <artifactId>speedment-maven-plugin</artifactId>
                <version>${speedment.version}</version>
            </plugin>
            <configuration>
                <debug>true</debug>
            <configuration>
        </plugins>
    </build>
```

Once Debug Mode is enabled, much more information will be printed out on the console when the plugin runs.

### The Configuration File
Speedment stores the configuration of the database metadata in a special JSON file that, by default, is 
located in the file src/main/json/speedment.json

### Specifying a Configuration File
TBW

### Resetting the Configuration File
If the configuration file is removed, the Tool will be reset and we can start all over with a clean sheet.
