# Motivations

## Compilers Are Hard

Having tinkered with a few compilers, I've noticed that many compilers can be difficult to work with and integrate with, especially when it comes to creating compiler plugins and easily accessing and modifying compiler internals. Lexi is intended to provide a multi-lingual and multi-target compiler that provides for easy modification via a formally supported first-class API.

## Compilers Are Too Specialized

Compilers are typically built for a single language. Languages these days are converging more and more in their design, but compilers remain divergent.

Compiler authors end up reimplementing algorithms over and over again for every language. All languages share many algorithms in common, yet none of the compiler components are reusable. Even when a new language version is released, the compiler has to be changed because they're often not designed using sound software engineering principles.

Another concern is languages can't interop very well with each other. Scala, Kotlin and Java, for example, all target the JVM, and have very similar linguistic characteristics and algorithms under the hood, yet they do not interop 100% with each other. Of course, Kotlin has Java compatibility, but cannot interact fully with Scala. Additionally, each language has its own build tool.

What if I wanted to access Python libraries but using a strongly typed language like Scala or Kotlin? Realistically, your options are:

1. Rewrite the Python library in Scala or Kotlin.
1. Build some sort of REST API or equivalent around the library and consume it as a service.

Frankly, this is dumb. So much code gets written in different languages that cannot be shared outside that language or even often a different version of that same language.

Once Lexi is functioning, all that's needed is to define language-specific syntax and semantics, then translate that to Lexi's IR. From there, any language can access another language's library.

## Inspirations

### MS CLR / .NET

Honestly, Microsoft had a good idea in mind when they built the Common Language Runtime. They built a really solid platform to run multiple languages. Of course, CLR/.NET is an MS technology and only supports the languages they provide. The goal with Lexi is to truly make plug-and-play support easy to do with any language.

### Dotty / Scala 3

Dotty (Scala 3) has introduced a new compiler that is quite magnificent. In particular, it makes use of the new TASTy (Typed Abstract Syntax Trees). TLDR: TASTy is a formalized tree structure that encodes sufficient information about different versions of code to allow for compatibility between Scala 2 and 3.

Lexi is adopting the TASTy concept to provide language-agnostic representation of program logic. With Lexi, you can code and Python, Kotlin, Scala, etc...and yet each language will get compiled to an intermediate TASTy format that can then be transformed to any target - JVM, native, etc....

# Officially Supported API

The compiler is structured into phases, and all phases are exposed via a first-class API. This means that all tree structures can be accessed and modified at compile-time via custom plugins.

# Language-Independent IR (Intermediate Representation)

Lexi provides 

# Multi-Language Support

The goal with Lexi is to provide support for multiple languages through a language semantics plugin. The syntax and semantics for a given language is defined as a language plugin, which is then included in the compiler frontend.

To start, I'm implementing the current [Kotlin language spec](https://github.com/Kotlin/kotlin-spec). Next on the docket are Scala and Python.

At a high level, a language semantics plugin does the following:

1. Parses the source code into a language-specific TAST.
1. Transforms the AST into the Lexi intermediate representation.

One example of how a language plugin can be implemented (and how I'm currently implementing JVM languages like Kotlin and Scala):

1. Define a language grammar via ANTLR v4.
1. Transform ANTLR v4 parse tree to your own custom abstract syntax tree.
1. Apply any language-specific semantic rules (type features, validity checks, etc...) to the AST.
1. Define a translation between the final AST and Lexi's built-in IR tree. From there Lexi will handle the rest of the backend compiler tasks, including backend specific code generation (JVM, native, etc...).

# Multi-Target Support

The compiler supports building native targets as well as for use on the JVM.

# Contributing to Lexi

If you're interested in getting involved, we're always looking for help!

To get started, check out the [Contributor's Guide](docs/contribute)

# Architecture

Check out the [architecture docs](/docs/architecture) to learn the internals of the compiler.
