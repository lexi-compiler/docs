# Why?

Having tinkered with a few compilers, I've noticed that many compilers can be difficult to work with and integrate with, especially when it comes to creating compiler plugins and easily accessing and modifying compiler internals. Lexi is intended to provide a multi-lingual and multi-target compiler that provides for easy modification via a formally supported first-class API.

# Officially Supported API

The compiler is structured into phases, and all phases are exposed via a first-class API. This means that all tree structures can be accessed and modified at compile-time via custom plugins.

# Multi-Language Support

The goal with Lexi supports multiple languages, and is intended to provide fairly easy language support by creating a language semantics plugin.

To start, I'm implementing the current [Kotlin language spec](https://github.com/Kotlin/kotlin-spec). Next on the docket will be Scala. From there, I'll see what catches my fancy.

At a high level, a language semantics plugin does the following:

1. Parses the source code into a language-specific AST.
1. Transforms the AST into the Lexi backend IR tree.

One example of how this could be implemented (and how I'm currently implementing JVM languages like Kotlin and Scala):

1. Define a language grammar via ANTLR v4.
1. Transform ANTLR v4 parse tree to your own custom abstract syntax tree.
1. Apply any language-specific semantic rules (type features, validity checks, etc...) to the AST.
1. Define a translation between the final AST and Lexi's built-in IR tree. From there Lexi will handle the rest of the backend compiler tasks, including backend specific code generation (JVM, native, etc...).

# Multi-Target Support

The compiler supports building native targets as well as for use on the JVM.
