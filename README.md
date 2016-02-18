# Maven Interactive Tutorial - Git based

### Steps reference
* Step 1: Parent creation
* Step 2: Module aggregation
* Step 3: Second module aggregation
* Step 4: Centralized dependencies management in parent POM
* Step 5: Multi-module inherited properties
* Step 6: Profiles to alter a lifecycle
* Step 7: Plugin basic configuration

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

### Centralized dependencies management in parent POM
By doing so, we ensure that the utilization of the project dependencies are common to all its children, making it easier to see, manage and update them. This can be achieved by defining *<dependencyManagement>* section in the parent pom.
This tag means that all that it contains are not dependencies itself that are strictly going to be used (besides they are potentially going to be), but they are there to be parametrized for its children to be used. We define here, that the JUnit version is 3.8.1. We will not specify the version in the *<dependencies>* version of the children as we want it to be handled by the parent only.
On the other side, we are not defining the scope on the parent, as the usage of the artifact may be different for each module. It can be done, but it depends on the case.

For further info about dependencies management, check the [official doc here](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html)

### Multi-module inherited properties
By defining properties in the parent POM, they will be available for its childs. On the other side, if we define a property in a specific module it won't be available for its parent, as it only works downstream.
We can do this by defining the *<properties>* section. Not necessarily have to be made in the parent, it just depends on the visibility you need for each of them.

### Profiles to alter a lifecycle
Configuring different profiles allow us to define different behaviours based on which profile is being requested. We could export a coverage report only for development, configure a different set of URLs or usernames for production, just as an example.
In this case, the *development* profile is enabled by default (it gets executed if you don't specify a profile), and a *production* role is off by default. The difference between them is that they will set the same property with a different value. The idea is to spit out the logs to the console on development, and to a file on production environment. So depending where are we going to send our build is which profile we will activate like this:
```
mvn install -Pproduction
```
For further reference about the Lifecycle, Phases and goals check [this article](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)

Fur further info about check [this other one](http://maven.apache.org/guides/introduction/introduction-to-profiles.html)

### Plugin basic configuration
You would need to configure a plugin when you need to attach a specific functionality to a specific phase. In this example, we are speficying which should be the name of the output WAR file on the webapp module. In this case the *maven-war-plugin* is the default plugin used for war packaging during *package* phase. In this case we are explicitly adding it to parametrize the final name this war will have (of course you are able to do a lot more than this, it's just a simple example).
Each plugin has a default phase defined, that it will be attached to if you don't define a different one.
