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

The TestRig tool can be used to check the parser. We first need to compile the Java classes previously generated:

```
%JAVA_HOME%\bin\javac -cp antlr-4.7.2-complete.jar Vtl*.java
```

We can then analyse a VTL expression, for example (line 2643 of the VTL [reference manual](https://sdmx.org/wp-content/uploads/VTL-2.0-Reference-Manual-20180712-final.pdf)):

```
DS_r := DS_1[calc Me_2 := upper(Me_1)]
```

Save this VTL statement in a `vtl-example.txt` file and run the following command:

```
java -cp .;antlr-4.7.2-complete.jar org.antlr.v4.gui.TestRig Vtl start -tree -ps vtl-example.ps vtl-example.txt
```

This outputs the structure of the statement:

```
(start (statement (varID DS_r) := (expr (exprAtom (ref (varID DS_1))) [ (datasetClause (calcClause calc (calcClauseItem (componentID Me_2) := (calcExpr (expr (exprAtom upper ( (expr exprAtom (ref (varID Me_1)))) ))))))) ])) <EOF>)
```

A `vtl-example.ps` is also produced, which contain an image of the parse tree corresponding to the VTL statement:

![Parse tree](/img/vtl-example.png "Parse tree")

