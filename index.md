# Motivations

## Compiler Integration/Modification Is Poor

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

### Lack of APIs / Poor Abstractions

I started tinkering with the Kotlin compiler to build some plugins. Now please note that this is not a bashing session on the Kotlin compiler. However, while attempting to work with its internals I noted that the Kotlin compiler is constantly changing and still does not have proper API support. In fact, at this moment there is no formal API. Furthermore, the Kotlin compiler is structured in a way that requires heavy dependence on PSI elements. These can be painful to work with and encode for other use cases. With Lexi, the goal is to remain independent from PSI&mdash;yet provide easy integration with PSI, especially for integration with IntelliJ.

Additionally, while I was initially excited about Kotlin Native, I quickly became discouraged with it. Kotlin Native has enough language differences that you're really not using the same code. Multiplatform is, in my humble opinion, a travesty. This was an opportunity for a compiler to solve cross-platform problems in a truly magical way, and yet it feels like a failure that requires more effort than it's really worth.

Kotlin C-Interop is much nicer than many other languages, but frankly it still requires too much heavy lifting. If you're using Native, you get access to the cinterop tool, but you still have to deal with Native code by concerning yourself too much with setup to get it to work.
he user
If you want to interop from JVM, you still have to manually deal with JNI. JNI is very tedious to deal with and takes a lot of error-prone effort to get right.

Lexi will provide true first-class abstractions around Native/JVM. The JNI layer will be handled automatically, and the goal is to provide compatibility with Native or JVM without being visible to the end user of the language. This means you can pull in shared native libraries and use them the same way whether you're compiling your program through Lexi for JVM or a native target.

### MS CLR / .NET

Honestly, Microsoft had a good idea in mind when they built the Common Language Runtime. They built a really solid platform to run multiple languages. Of course, CLR/.NET is an MS technology and only supports the languages they provide and is predominantly intended for Windows. The goal with Lexi is to truly make plug-and-play support easy to do with any language on any platform.

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
