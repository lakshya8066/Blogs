This is a final summary of my Google Summer of Code Project with in-toto. This was a summer-long project. List of the things I want to talk about in this blog:

1. A little about in-toto 
2. Introduction to the Jenkins plugin of in-toto
3. What is SLSA Provenance and my project to change the predicate of the in-toto attestation
4. The first was to get familiar with the Jenkins Plugin and how to test it and set the development environment locally
5. Then I found that the in-toto-java library supports java 11 so I had to update the Jenkins plugin java level to 11. This was hectic since there was a lot of friction in understanding what is going wrong.
6. After that, I found out that there was v0.2 of the SLSA predicate and the in-toto-java library only contained v0.1 code. So I created a new directory for java v0.2 which enabled me to use the v0.2 library in the plugin
7. I then proceeded to the main work of writing the code that will create a metadata file that will be in compliance with the SLSA Provenance.
8. This took a lot of decision-making about what info should each field contain and what is the actual meaning of the fields and how these fields can be viewed in a different context.
9. Finally completed the project by generating the Provenance metadata.

