# Creation and use of VTL parsers with Antlr

This paper describes how to use the [Antlr](https://www.antlr.org) parser generator to create parsers for the VTL language, and how to use these parsers.

Antlr is a powerful tool that can generate parsers for languages described by formal grammars. These parsers can automatically build parse trees, which are data structures explaining how the language represents a given expression.

Le language used here is [VTL 2.0](https://sdmx.org/?page_id=5096), the Validation and Transformation Language published by the SDMX initiative. VTL has a grammar which is expressed in EBNF notation (Extended Backus-Naur Form).

## Creation of the VTL parsers

In the following code examples, we use a Windows system and suppose that the JAVA_HOME environment variable is set.

Create a working directory and download the latest version of Antl: we refer here to [version 4.7.2](https://www.antlr.org/download/antlr-4.7.2-complete.jar). Place also in the directory the VTL grammar, described in the `Vtl.g4` and `VtlTokens.g4` files of the [VTL 2.0 package](https://sdmx.org/wp-content/uploads/VTL-2.0-package-2018.07.12.zip).

### Java parser generation

The Java parser and associated artefacts are created with the following command (the `-visitor` option is explained below):

```
java -jar antlr-4.7.2-complete.jar -visitor Vtl.g4
```

We can notice that the VTL grammar produces a warning message:

```
warning(154): Vtl.g4:321:0: rule joinExpr contains an optional block with at least one alternative that can match an empty string
```

### Parser verification