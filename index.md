## Setup

You'll need `sbt 1.4.5`.

To run the tests:

```shell
sbt test
```

To build a standalone compiler jar file:

```shell
sbt clean compile assembly
```

You can then interpret Kotlin code:

```shell
java -jar target/scala-2.13/lexi-assembly-0.1.0-SNAPSHOT.jar -lang kotlin "val x = 5"
```

You would interpret Scala by changing the `-lang` switch option:

```shell
java -jar target/scala-2.13/lexi-assembly-0.1.0-SNAPSHOT.jar -lang scala "val x = 5"
```

You can also compile a Kotlin file into a Java class file:

```shell
java -jar target/scala-2.13/lexi-assembly-0.1.0-SNAPSHOT.jar Main.kt
```

Same concept for Scala:

```shell
java -jar target/scala-2.13/lexi-assembly-0.1.0-SNAPSHOT.jar Main.scala
```

## Why?

Having tinkered with a few compilers, I've noticed that many compilers can be difficult to work with and integrate with (compiler plugins, easy access to internals).

The goal of the Lexi compiler is to provide a multi-lingual and multi-target compiler that provides easy integration with its internals via a first-class API.

## Supported Languages

To start, I'm implementing the current [Kotlin language spec](https://github.com/Kotlin/kotlin-spec). Next on the docket will be Scala. From there, I'll see what catches my fancy.

## Meta Features

### Compiler

Note: The compiler features are pretty useful in their own right, but be sure to read through the runtime features as those are really where Lexi shines!

### Multiple Languages

Lexi supports multiple languages, and is intended to provide fairly easy language support by creating a language semantics plugin.

At a high level, a language semantics plugin does the following:

1. Parses the source code into a language-specific AST.
1. Transforms the AST into the Lexi backend IR tree.

One example of how this could be implemented (and how I'm currently implementing JVM languages like Kotlin and Scala):

1. Define a language grammar via ANTLR v4.
1. Transform ANTLR v4 parse tree to your own custom abstract syntax tree.
1. Apply any language-specific semantic rules (type features, validity checks, etc...) to the AST.
1. Define a translation between the final AST and Lexi's built-in IR tree. From there Lexi will handle the rest of the backend compiler tasks, including backend specific code generation (JVM, native, etc...).

#### First-Class API

The compiler is structured into phases, and all phases are exposed via a first-class API. This means that all tree structures can be accessed and modified at compile-time via custom plugins.

#### Multiple Targets

The compiler supports building native targets via LLVM as well as JVM bytecode.

## Contributing

This project is open-source and contributions are more than welcome! Feel free to open a pull request! You can also reach out to me at matt@mattmoore.io if you have any questions or want to know how to get started.

Check out the [compiler docs](docs/README.md)&mdash;a good place to get started if you want to contribute to the compiler itself.
