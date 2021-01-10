# Contributing to Lexi

This project is open-source and contributions are more than welcome! Feel free to open a pull request! You can also reach out to me at matt@mattmoore.io if you have any questions or want to know how to get started.

Check out the [architecture docs](docs/README.md) to learn the internals of the compiler.

# Developer Setup

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
