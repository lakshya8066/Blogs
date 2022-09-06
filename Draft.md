This is a final summary of my Google Summer of Code Project with in-toto. This was a summer-long project. List of the things I want to talk about in this blog:

1. A little about in-toto 
2. Introduction to the Jenkins plugin of in-toto
3. What is SLSA Provenance and my project to change the predicate of the in-toto attestation
4. The first was to get familiar with the Jenkins Plugin and how to test it and set the development environment locally
5. Then I found that the in-toto-java library supports java 11 so I had to update the Jenkins plugin java level to 11. This was hectic since there was a lot of friction in understanding what is going wrong.
6. After that, I found out that there was v0.2 of the SLSA predicate and the in-toto-java library only contained v0.1 code. So I created a new directory for java v0.2 which enabled me to use the v0.2 library in the plugin
7. I then proceeded to the main work of writing the code that will create a metadata file that will be in compliance with the SLSA Provenance.
8. This took a lot of decision-making about what info should each field contain and what is the actual meaning of the fields and how these fields can be viewed in a different context.
9. Mapped the fields with relevant objects that have the information required.
10. Serialize the Provenance metadata object
11. Then download the metadata file to the local directory.


# About in-toto

In-toto is a framework designed to protect software supply chain integrity. It provides security against attackers who can get control of a step in the supply chain and alter the product for malicious intents like introducing backdoor in the source code and including vulnerable libraries in the final product. To address these issues, in-toto, cryptographically ensures the integrity of the software supply chain and makes sure that all the steps within the supply chain are clearly laid out. It achieves that by providing integrity, authentication and auditability to the supply chain as a whole. in-toto creates a file called layout for the project which is essentially a file signed by the project owner. This file dictates the series of steps that need to be carried out in the SSC(Software supply chain) in order to create a final product. Through the course of the build a file called a link metadata is generated which gathers adn stores information about the commands and files related to each step of the build.   


In-toto can significantly reduce an attacker’s opportunity when used in conjunction with other techniques described in the CNCF whitepaper and SLSA levels.

# Goal of the project

The goal of the project was to add support for a new predicate called SLSA Provenance along with the old Link predicate in the in-toto attestation. This new predicate not only gathers information that the OG link metadata did but also extra information that will describe when, where and how the software artifacts were produced. Due to these extra information in the metadata, it achieves a higher SLSA (Supply-chain Levels for Software Artifacts) level guaranteeing more resilient integrity.  

(Put image from SLSA Provenance)

# Implementation of project

The project is divided in two parts,
1. Refactoring the code in in-toto-java library to support the Data Structue of the metadata using Object Oriented Properties. Updating to Jenkins repository to Java 11 from Java 8.
2. Gathering the correct information from the Jenkins internal objects to store in the metadata file.

First challenge was to upadate the `pom.xml` of the maven project that built the plugin. This [Pull Request](https://github.com/in-toto/in-toto-jenkins-plugin/pull/4/files) contains the changes made to the pom.xml that upgrades the dependencies to the lastes version as well remove some redundant dependencies that are no longer of use. 

The major portion of this update was to making every dependency compatible with Java 11. Once the PR was merged, we also had to update the Java version in the CI system of Jenkins tests. This [pull request](https://github.com/jenkinsci/in-toto-plugin/pull/30/files) is for the same.

Once this was over with and we got the plugin up and running with Java 11, it was time to work on the setting up a class and sub-class structure that would depict the metadata format as required by the SLSA Provenance. 

The code for version 0.1 of SLSA Provenance was already present in the in-toto-java library but not for verison 0.2 . This came up in one of our meetings that we should have support for both the versions to expand the usage of the library. So, the next task was to add a class based heirarchy and create a sort of a parent data structure that had a place for every feild of the Provenance metadata. 
This [pull request](https://github.com/in-toto/in-toto-java/pull/64) contains the code to shift the existing code for version 0.1 to a new deidcated directory. 

The code for the version 0.2 of SLSA Provenance resides in a new directory and this [pull request](https://github.com/in-toto/in-toto-java/pull/40) achieves that. There are minor changes from v0.2 to v0.1 of SLSA Provenance and this Pr updates the the data structure with the changes.
The first half the projects end here where we complete the basic setup and preparation to structure the data structure.


# in-toto wrapper
The Provenance metadata for now is only enables for the in-toto wrapper. 

# Figuring out the objects that have the information that we require in the metadata
Almost all the information that we need can be found in setup function of the wrapper. This function contains environment variables related to all the internal of the Jenkins core. The 

# 