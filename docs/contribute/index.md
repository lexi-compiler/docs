# Contributing to Lexi

This project is open-source and contributions are more than welcome! Feel free to open a pull request! You can also reach out to me at matt@mattmoore.io if you have any questions or want to know how to get started.

Check out the [architecture docs](/docs/architecture) to learn the internals of the compiler.

# Developer Setup

You'll need `sbt 1.4.5`.

### _Note:_

While Lexi is itself written in Scala (v3), the JDK you use must be greater than 8.

While you can compile with JDK 8 and above, I build against [GraalVM Community Edition 11](https://www.graalvm.org). The reason for this is to easily generate [native images of the compiler](https://www.graalvm.org/reference-manual/native-image/), so compiler end users do not have to worry about JDK versions.

To run the tests:

```shell
sbt test
```

To build a standalone compiler jar file:

```shell
sbt graalvm-native-image:packageBin
```

This will compile a native executable stored in `target/graalvm-native-image/`

You can compile a Kotlin file into a Java class file:

```shell
lexi Main.kt
```

Same concept for Scala:

```shell
lexi Main.scala
```
