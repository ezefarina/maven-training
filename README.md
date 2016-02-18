# Maven Interactive Tutorial - Git based

### Steps reference
* Step 1: Parent creation
* Step 2: Module aggregation
* Step 3: Second module aggregation

### Parent creation
Parent POM file is created with the following archetype
```
mvn archetype:generate -DgroupId=com.ezefarina.maven -DartifactId=maven-training-base -DarchetypeGroupId=org.codehaus.mojo.archetypes -DarchetypeArtifactId=pom-root -DinteractiveMode=false
```
Defined the following elements:
* modelVersion: Defines the XSD version used for the XML validation of the current POM file
* groupId: Package formatted name of the project or group of projects. eg. org.apache.maven
* version: Version identifier for the pair groupId:artifactId
    - During packaging phase, it's used for naming the output file/s
    - During install phase, it's used for creating the path and putting the artifact in the local repository
    - During deploy phase, it's used for creating the path and putting the artifact in the remote repository
* packaging: Defines what is the build output artifact type. Core possible ones: pom, jar, maven-plugin, ejb, war, ear, rar, par
* artifactId: Unique identifier for this artifact, under the defined groupId
* name: Used as a short name or alias, for listing in IDE's and the developer's alternative name for this artifact

For full POM reference you can check the [official specification](http://maven.apache.org/ref/3.3.9/maven-model/maven.html)

And [here](https://gist.github.com/zbigniewTomczak/4235871) is a good list of available archetypes

### Module aggregation
A child module is added, based on the following archetype
```
mvn archetype:generate -DgroupId=com.ezefarina.maven -DartifactId=maven-training-base-module -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```
By running an artifact creation like this, standing over an existing pom location it will automatically add the new artifact as a child of it. In this case, the module added is a simple Java Main class with its default project structure.
This default structure is defined in the archetype itself and it contains the following components:
* Module folder
* Java main class for standalone execution
* Test class for the java class created
* Own POM file
* JUnit added as a default dependency
* (conditional) Module added as a child if there's a POM to do so in the current location

### Second module aggregation
Another child module is added, using a web application archetype
```
mvn archetype:generate -DgroupId=com.ezefarina.maven -DartifactId=maven-training-base-webapp -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false
```
This new module is added as well to the parent POM, but in this case the output structure is a bit different, as the archetype is different. We have an index.jsp, a web.xml, a resources folder, and some other elements that are common (or needed) for a really basic web application.

