# release-version-finder


## Table of content
- [Introduction](#introduction)
- [Technologies Used](#technologies-used)
- [Explanation](#explanation)
  - [Properties](#properties)
  - [Build resources](#build-resources)
  - [maven-resources-plugin](#maven-resources-plugin)
  - [build-helper-maven-plugin](#build-helper-maven-plugin)
- [How to execute](#how-to-execute)
- [Github Actions](#github-actions)
- [Contribution](#contribution)
- [Contact Information](#contact-information)

## Introduction

This project uses maven plugins to write important release information such as major version, minor version etc in a build-info.properties file.

## Technologies Used

- Java 11
- [SpringBoot](https://start.spring.io/): Used to create easy stand-alone, production-grade Spring based Applications.
- [Maven](https://maven.apache.org/): Apache Maven is a software project management and comprehension tool. Based on the concept of a project object model (POM), Maven can manage a project's build, reporting and documentation from a central piece of information.
- [Git Actions](https://docs.github.com/en/actions): GitHub Actions helps automate tasks within the software development life cycle. GitHub Actions are event-driven, meaning that you can run a series of commands after a specified event has occurred. They happen directly on the Github repo itself.


## Explanation
The properties that should be created on runtime are placed in /src/main/resources/**build-info.properties** file. For this project, 6 properties are defined. They are: 

|Property|Variable|
| --- | --- |
|version|${pom.version}|
|version.major|${parsedVersion.majorVersion}|
|version.minor|${parsedVersion.minorVersion}|
|build.number|${parsedVersion.buildNumber}|
|build.date|${timestamp}|
|build.branch.env|${build.branch}|

Some of these properties are overriden in pom.xml under properties section whereas others proiperties are used with their default variables. The main version extraction process happens inside `validate` phase inside ***build-helper-maven-plugin*** under goal: 
> parse-version.

All relevant parts of pom.xml are described below with comments.
### Properties
```xml

<properties>
		<java.version>11</java.version>
		<!-- Properties defined here override the ones defined in src/main/resources/build-info.properties.-->
		<!-- If the property is not defined here, default property is used in build-info.properties -->
		
		<timestamp>${maven.build.timestamp}</timestamp>
		<!-- Ideally speaking, this should be left blank and later filed out by ci. For now, it is hard-coded -->
		<build.branch>master</build.branch>	
		<maven.build.timestamp.format>yyyy-MM-dd HH:mm:ss</maven.build.timestamp.format>
	</properties>

```


### Build resources
```xml

   <resources>
		<!-- This ensures that build-info properties file is used for processing. -->
			<resource>
				<directory>src/main/resources</directory>
				<filtering>true</filtering>
			</resource>
		</resources>

```

### maven-resources-plugin
```xml

   <plugin>
			<!--  This is required for version to be extracted. Without it, build-helper-maven-plugin will not work.-->
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-resources-plugin</artifactId>
		  <configuration>
				<!-- This means that $(default delimitier) is used -->
				<useDefaultDelimiters>true</useDefaultDelimiters>
			</configuration>
		</plugin>

```

### build-helper-maven-plugin
```xml

   <plugin>
			<!-- This plugin fills the build-info file with values. -->
			<groupId>org.codehaus.mojo</groupId>
			<artifactId>build-helper-maven-plugin</artifactId>
			<executions>
				<execution>
					<phase>validate</phase>
          <id>parse-version</id>
					<goals>
						<goal>parse-version</goal>
					</goals>
					<configuration>
						<!-- The property prefix which is used for checking variables in build-info properties file. -->
						<propertyPrefix>parsedVersion</propertyPrefix>
					</configuration>
				</execution>
			</executions>
		</plugin>

```

## How to execute
Run the following command
> mvn clean install

After executin is complete, go to target folder and extract the generated  jar file.

Once extracted, please go to target/{generated file}/BOOT-INF/classes and open **build-info.properties**. This contains the values extracted from the pom.xml. One possible application is in Continuous Integration where these infos can be extracted by the pipeline for subequent report generation.

## Github Actions
To ensure the correctness of the code committed, github action for 'building the java project using Maven' is used. This ensures that all tests are green and the projects runs correctly on Windows, MacOS and Linux systems.

## Contribution

Feature requests, issues, pull requests and questions are welcome. -

## Contact Information

How to reach me? At [github specific gmail account](mailto:syedumerahmedcode@gmail.com?subject=[GitHub]%20Hello%20from%20Github).
