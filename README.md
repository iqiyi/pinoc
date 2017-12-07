# Pinoc

Pinoc is a novel library for dynamic classloader-free modification of an Android app,

Specifically, Pinoc supports the code injection at the entrance to a Java method, the code replacement of a whole Java method,
and the addition of a new Java method.

## Features

1. Provides a novel technique for hotfix deployment without the need of a Java classloader.

2. Provides a novel technique for dynamic event tracking.

3. High compatibility. Pinoc can run at all JVM-based platforms.

4. Real-time effect. Pinoc will replace or modify the methods immediately
once it has read the specified configuration.

## Principle

<img src="docs/pics/pinoc_structure.png" width="1200" height="1000"/>

When an Android app is built, the Pinoc plugin replaces each Java method in your app with
its variant. Specifically, after the replacement each method (the original method) is replaced with its variant.

At runtime, when a method is invoked, it is the variant of the original method that is actually invoked.
The variant passes the information about the invocation to Pinoc,
which decides whether to replace or modify the original method,
according to a configuration file, which may be downloaded from a server.

To avoid the trouble caused by the Java classloader, Pinoc does not adopt the Java classloader.
Thus the replacement or the modification of the original method is not written in Java.

Instead it is written in Zlang, a flexible dynamically-typed programming language running on the
JVM and supporting access to Java objects and interaction with Java at runtime.
It is easy to convert a Java method or statement into a Zlang function or statement.

If Pinoc decides to replace or modify the original method, it compiles the instructions of
the replacement or the modification by the Zlang compiler, after which the output of the compilation
is passed to the Zlang executor for execution.

See [Principle of Pinoc](docs/pinoc_principle.md) for more information.

## Comparision with other techniques

Since Pinoc can replace and modify Java methods, we may adopt it for hotfix deployment and dynamic event tracking.

### Hotfix deployment

Compared with the other techniques for hotfix deployment, Pinoc has the following advantages:

1. Classloader-free. Pinoc does not adopt a Java classloader to load the hotfix.

2. High compatibility. Pinoc can run at all JVM-based platforms.

3. Real-time effects. Pinoc will replace or modify the methods immediately
once it has read the specified configuration.

### Dynamic event tracking

Compared with all of the bare current techniques, Pinoc has the following advantages:

1. Any method of any class can be tracked.

2. The tracking instructions can be specified dynamically and may perform almost
all the operations allowed by Android.

See [Principle of Pinoc](docs/pinoc_principle.md) for more information.

## Demo

A demo is provided, which you may refer to. See [Demo](docs/pinoc_demo.md) for the details.

## Deployment

To use the Pinoc library, add the following in your `build.gradle`:

```
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.iqiyi:pinoc-plugin:0.2.1' 
    }
}
apply plugin: "pinoc"
```

Also, you may disable Pinoc temporarily by adding the following
in `gradle.properties` of your project or modules:

```
pinoc-plugin.enabled=false // default is true
```

To learn Zlang, please refer to [Zlang](docs/zlang/zlang.md).

To learn Pinoc, please refer to [Usage of Pinoc](docs/pinoc_usage.md).

To learn a demo, please refer to [Demo of Pinoc](docs/pinoc_demo.md).

## License

Copyright (C) 2017 iQIYI.com

The binaries and source code of the Pinoc library and the Pinoc plugin can be used according to the
[Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0.html).