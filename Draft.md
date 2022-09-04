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

In-toto is a framework designed to protect software supply chain integrity. It provides security against attackers who can get control of a step in the supply chain and alter the product for malicious intents like introducing backdoors in the source code and including vulnerable libraries in the final product. To address these issues, in-toto, cryptographically ensures the integrity of the software supply chain and makes sure that all the steps within the supply chain are clearly laid out. It achieves that by providing integrity, authentication and auditability to the supply chain as a whole. in-toto creates a file called layout for the project which is essentianlly a file signed by the project owner. This file dictates the series of steps that need to be carried out in the SSC(Software supply chain) in order to create a final product. Thirugh the course of the build a file called a link metadata is generated which gathers adn stores information about the commands and files related to each step of the build.   


In-toto can significantly reduce an attacker’s opportunity when used in conjunction with other techniques described in the CNCF whitepaper and SLSA levels.

# Goal of the proejct

The goal of the project was to add support for a new predicate along with the old Link predicate. This new predicate i.e. Provenance not only achieves a higher SLSA (Supply-chain Levels for Software Artifacts) level guaranteeing more resilient integrity, but also describes where, when, and how software artifacts were produced by defining the moving parts in a complex supply chain. 