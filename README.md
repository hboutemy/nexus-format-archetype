# nexus-format-archetype

[![Build Status](https://travis-ci.org/sonatype-nexus-community/nexus-format-archetype.svg?branch=master)](https://travis-ci.org/sonatype-nexus-community/nexus-format-archetype) 
[![Join the chat at https://gitter.im/sonatype/nexus-developers](https://badges.gitter.im/sonatype/nexus-developers.svg)](https://gitter.im/sonatype/nexus-developers?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![DepShield Badge](https://depshield.sonatype.org/badges/sonatype-nexus-community/nexus-format-archetype/depshield.svg)](https://depshield.github.io)

Archetype for creating Nexus format plugin with a _lot_ of the boilerplate required to start development already created.

## How to create a format
Once the code has been checked out.

Build the archetype:

    mvn clean install

Change directory to a new folder where you wish to generate the format boilerplate code.
 
Generating a format plugin is as easy as running the following:
     
    mvn archetype:generate                              \ 
      -DarchetypeArtifactId=nexus-format-archetype      \
      -DarchetypeGroupId=org.sonatype.nexus.archetypes  \
      -DarchetypeVersion=1.0-SNAPSHOT                   \
      -DgroupId=org.sonatype.nexus.repository           \
      -DartifactId=nexus-repository-foo                 \
      -DpluginFormat=foo                                \
      -DpluginClass=Foo                                 \
      -Dversion=0.0.1

Repeated for the "newline" challenged:

    mvn archetype:generate -DarchetypeArtifactId=nexus-format-archetype -DarchetypeGroupId=org.sonatype.nexus.archetypes -DarchetypeVersion=1.0-SNAPSHOT -DgroupId=org.sonatype.nexus.repository -DartifactId=nexus-repository-foo -DpluginFormat=foo -DpluginClass=Foo -Dversion=0.0.1
    
It is recommended to keep the naming of the following parameters consistent with the plugin you wish to develop:

**pluginFormat** = _A name with no whitespace that best describes the format_ (e.g. raw, yum, npm etc.)

**pluginClass** = _The class name that will be used to generate the plugin boilerplate code_ (e.g. Raw, Yum, Npm etc.)

**version** = _The version of the format to be developed_      

## How to contribute to this archetype

The standard [Maven Archetype Plugin](https://maven.apache.org/archetype/maven-archetype-plugin/index.html) 
docs are a good place to start.

We also use the [archetype integration-test goal](https://maven.apache.org/archetype/maven-archetype-plugin/integration-test-mojo.html)
to verify the archetype works as expected. You may need to update some of the [reference](src/test/resources/projects/it1/reference/)
files after changes are made that affect generated source code. 

You can manually run the integration tests via:

    mvn clean package archetype:integration-test
