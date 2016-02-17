# Maven Training Base

### History Reference

### Parent creation
Parent POM file was created with the following archetype
```
mvn archetype:generate -DgroupId=com.openenglish.maven -DartifactId=maven-training-base -DarchetypeGroupId=org.codehaus.mojo.archetypes -DarchetypeArtifactId=pom-root -DinteractiveMode=false
```
Defined the following elements:
* modelVersion: Defines the XSD version used for the XML validation of the current POM file
* groupId: Package formatted name of the project or group of projects. eg. org.apache.maven, com.openenglish.lp2.persistence
* version: Version identifier for the pair groupId:artifactId
    - During packaging phase, it's used for naming the output file/s
    - During install phase, it's used for creating the path and putting the artifact in the local repository
    - During deploy phase, it's used for creating the path and putting the artifact in the remote repository
* packaging: Defines what is the build output artifact type. Core possible ones: pom, jar, maven-plugin, ejb, war, ear, rar, par
* artifactId: Unique identifier for this artifact, under the defined groupId
* name: Used as a short name or alias, for listing in IDE's and the developer's alternative name for this artifact

