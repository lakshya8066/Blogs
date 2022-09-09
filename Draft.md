This is a final summary of my Google Summer of Code Project with in-toto. This was a summer-long project. List of the things I want to talk about in this blog:

# About in-toto

In-toto is a framework designed to protect software supply chain integrity. It provides security against attackers who can get control of a step in the supply chain and alter the product for malicious intents like introducing backdoor in the source code and including vulnerable libraries in the final product. To address these issues, in-toto, cryptographically ensures the integrity of the software supply chain and makes sure that all the steps within the supply chain are clearly laid out. It achieves that by providing integrity, authentication and auditability to the supply chain as a whole. in-toto creates a file called layout for the project which is essentially a file signed by the project owner. This file dictates the series of steps that need to be carried out in the SSC(Software supply chain) in order to create a final product. Through the course of the build a file called a link metadata is generated which gathers adn stores information about the commands and files related to each step of the build.   
In-toto can significantly reduce an attacker’s opportunity when used in conjunction with other techniques described in the CNCF whitepaper and SLSA levels.

# Goal of the project

The goal of the project was to add support for a new predicate called SLSA Provenance along with the old Link predicate in the in-toto attestation. This new predicate not only gathers information that the OG link metadata did but also extra information that will describe when, where and how the software artifacts were produced. Due to these extra information in the metadata, it achieves a higher SLSA (Supply-chain Levels for Software Artifacts) level guaranteeing more resilient integrity.  

![SLSA Provenance](/images/provenance.svg)

# Implementation of project

## 1. Refactoring the code

The in-toto-java library which acts as in-toto api for Jenkins is build on Java 11 whereas the Jenkins was build on Java 8. So the first challenge was to upadate the `pom.xml` of the maven project that built the plugin. This [Pull Request](https://github.com/in-toto/in-toto-jenkins-plugin/pull/4/files) contains the changes made to the pom.xml that upgrades the dependencies to the lastes version as well remove some redundant dependencies that are no longer of use. 

The major portion of this update was to making every dependency compatible with Java 11. Once the PR was merged, we also had to update the Java version in the CI system of Jenkins tests. This [pull request](https://github.com/jenkinsci/in-toto-plugin/pull/30/files) is for the same.

Once this was over with and we got the plugin up and running with Java 11, it was time to work on the setting up a class and sub-class structure that would depict the metadata format as required by the SLSA Provenance. 

![Provenance Metadata](/images/example.png)

The code for version 0.1 of SLSA Provenance was already present in the in-toto-java library but not for verison 0.2 . This came up in one of our meetings that we should have support for both the versions to expand the usage of the library. So, the next task was to add a class based heirarchy and create a sort of a parent data structure that had a place for every feild of the Provenance metadata. 
This [pull request](https://github.com/in-toto/in-toto-java/pull/64) contains the code to shift the existing code for version 0.1 to a new deidcated directory. 

The code for the version 0.2 of SLSA Provenance resides in a new directory and this [pull request](https://github.com/in-toto/in-toto-java/pull/40) achieves that. There are minor changes from v0.2 to v0.1 of SLSA Provenance and this Pr updates the the data structure with the changes.
The first half the projects end here where we complete the basic setup and preparation to structure the data structure.

## 2. Gathering the correct information from the Jenkins internal objects to store in the metadata file.
# in-toto wrapper
The Provenance metadata for now is only enables for the in-toto wrapper. 

# Gathering information to fill in the metadata
Now that the structure for the metadata is ready in in-toto-java library, we can use it to store the metadata. Since the defination of the fields in the metadata are flexible, there was a lot of discussion around how we want to use them. This [pull reuqest](https://github.com/in-toto/in-toto-jenkins-plugin/pull/5#pullrequestreview-1098226215) adds support for a new predicate i.e SLSA Provenance v0.2 that gathers information from each step of the build, store it in a txt file and dumps it in the local directory. 

This was the summary of my work as a Google Summer of Code mentee at CNCF. Thank you for your time reading the report.

If you have anything you wanna talk about or just say Hi, do reach out to me on Linkedin or Twitter. I will be more than happy to talk to you.