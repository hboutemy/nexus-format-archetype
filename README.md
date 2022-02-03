# nexus-format-archetype

<!--
[Maven Central - Snapshot](https://repository.sonatype.org/content/repositories/snapshots/org/sonatype/nexus/archetypes/nexus-format-archetype/)
-->
[![Maven Central](https://img.shields.io/maven-central/v/org.sonatype.nexus.archetypes/nexus-format-archetype.svg?label=Maven%20Central)](https://search.maven.org/artifact/org.sonatype.nexus.archetypes/nexus-format-archetype)
[![CircleCI](https://circleci.com/gh/sonatype-nexus-community/nexus-format-archetype.svg?style=shield)](https://circleci.com/gh/sonatype-nexus-community/nexus-format-archetype) 
[![Join the chat at https://gitter.im/sonatype/nexus-developers](https://badges.gitter.im/sonatype/nexus-developers.svg)](https://gitter.im/sonatype/nexus-developers?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![DepShield Badge](https://depshield.sonatype.org/badges/sonatype-nexus-community/nexus-format-archetype/depshield.svg)](https://depshield.github.io)

A [Maven Archetype](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html) for creating a Nexus format plugin with a _lot_ of the boilerplate required to start development already created.

See the [nexus-development-guides](https://sonatype-nexus-community.github.io/nexus-development-guides/) for more information about developing a plugin for a new format.

## How to create a format
1. Change directory to a new folder where you wish to generate the format boilerplate code.

    **NOTE**: In all the commands below where you see `-DarchetypeVersion=1.0.REPLACE_WITH_LATEST_ARCHETYPE_VERSION` please be sure you replace
    `1.0.REPLACE_WITH_LATEST_ARCHETYPE_VERSION` with the latest version of the archetype, like the numeric part of the version shown on the "Maven Central" badge above.
 
2. Generating a format plugin is as easy as running the following:   
   
       mvn archetype:generate -DarchetypeArtifactId=nexus-format-archetype -DarchetypeGroupId=org.sonatype.nexus.archetypes -DarchetypeVersion=1.0.REPLACE_WITH_LATEST_ARCHETYPE_VERSION -DpluginFormat=foo -DpluginClass=Foo      
    
#### Required parameters:

Coordinates of the archetype:

**archetypeArtifactId** = _Must be:_ nexus-format-archetype

**archetypeGroupId** = _Must be:_ org.sonatype.nexus.archetypes

**archetypeVersion** = _The version of this archetype_ (e.g. 1.0.REPLACE_WITH_LATEST_ARCHETYPE_VERSION)

It is recommended to keep the naming of the following parameters consistent with the plugin you wish to develop:

**pluginFormat** = _A name with no whitespace that best describes the format_ (e.g. raw, yum, npm etc.)

**pluginClass** = _The class name that will be used to generate the plugin boilerplate code_ (e.g. Raw, Yum, Npm etc.)

#### Optional parameters:

**nexusPluginsVersion** = _The version of Nexus to use in the format to be developed_ 
(default: Declared as a property in the archetype [pom.xml ~line 65-> defaultNexusPluginsVersion](./pom.xml#L65]))

**artifactId** = _The artifactId of the format to be developed_ (default: nexus-repository-${pluginFormat})

**groupId** = _The groupId (and package) of the format to be developed_ (default: org.sonatype.nexus.plugins)

**version** = _The version of the format to be developed_ (default: 0.0.1-SNAPSHOT)     

## How to contribute to this archetype
Once the code has been checked out.

Build the archetype:

    mvn clean install
The standard [Maven Archetype Plugin](https://maven.apache.org/archetype/maven-archetype-plugin/index.html) 
docs are a good place to start.

We also use the [archetype integration-test goal](https://maven.apache.org/archetype/maven-archetype-plugin/integration-test-mojo.html)
to verify the archetype works as expected. You may need to update some of the [reference](src/test/resources/projects/it1/reference/)
files after changes are made that affect generated source code. 

Currently, the archetype integration test requires Java 8 (as does Nexus Repository Manager) to run.

You can manually run the archetype integration tests via:

    mvn clean package archetype:integration-test

#### Archetype IT Debugging notes

  After running the above `... package archetype:integration-test` command, you may see integration test errors like:
  
    [ERROR] Failed to execute goal org.apache.maven.plugins:maven-archetype-plugin:3.1.1:integration-test (default-cli) on project nexus-format-archetype: 
    [ERROR] Archetype IT 'it1' failed: Some content are not equals

  This occurs when [archetype-resources](src/main/resources) files have been changed, 
  but the IT [reference](src/test/resources/projects/it1/reference) source files no longer match the output 
  produced by the archetype. Any changes made to the files in the [archetype-resources](src/main/resources) folder will have to be made to the corresponding files in the [reference](src/test/resources/projects/it1/reference) folder. One quick way to find out which files differ is to compare the following two
  directories:
  
    $ diff -r target/test-classes/projects/it1/reference/ target/test-classes/projects/it1/project/nexus-repository-foo/
    diff -r target/test-classes/projects/it1/reference/pom.xml target/test-classes/projects/it1/project/nexus-repository-foo/pom.xml
    29c29
    <   <packaging>zzzbundle</packaging>
    ---
    >   <packaging>bundle</packaging>

  In the command above, the first directory holds the "expected" (reference) output. The second directory holds the "actual" output generated 
  by running the archetype integration test. 

  ![ITreferenceDiff](doc/images/diffITFolders.png)

#### Releasing
 
Releases occur automatically after a commit to the `master` branch. 

To avoid performing a release after a commit to `master`, be sure your commit message includes `[skip ci] `.

