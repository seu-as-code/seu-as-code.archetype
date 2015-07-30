[![Build Status](https://travis-ci.org/seu-as-code/seu-as-code.archetype.svg)](https://travis-ci.org/seu-as-code/seu-as-code.archetype)
[![Stories in Ready](https://badge.waffle.io/seu-as-code/seu-as-code.archetype.png?label=ready&title=Ready)](https://waffle.io/seu-as-code/seu-as-code.archetype)

# SEU-as-Code Archetype

  * SEU: German for software development environment (Software Entwicklungs-Umgebung)
  * as Code: to be able to configure, build and program the SEU as source code
  
The SEU-as-Code archetype is a quickstart template for new projects. The archetype is continuously evolved to
reflect the latest changes and enhancements.

## Quickstart

So you decided to build your next SEU for a new project as code. **Excellent choice!**
 
1. Download the latest SEU-as-Code Archetype distribution from either [Bintray](https://dl.bintray.com/seu-as-code/generic)
or [GitHub](https://github.com/seu-as-code/seu-as-code.archetype/releases).

2. Extract the archive to a custom location and rename the template project directory, e.g. `seubase`.

3. Customize the `build.gradle` file. First you need to adjust the `ext` configuration section and set the
`seuRoot` directory path and the `seuName` accordingly. The adjust the `dependencies` configuration section and
add the `software` or `home` package dependencies you require. For a complete list of packages have a look at the
[SEU-as-Code Package](https://github.com/seu-as-code/seu-as-code.packages) repository.

4. In case you want to use one of the SCM plugins you will have to customize the SVN or Git URLs in the `gradle.properties` file,
as well as the included scripts.

5. At this point you should add your SEU project to a version control system of your choice.

6. Finally, open a console and issue the following command to create the SEU: `gradlew bootstrapSeu`.

For a more detailed description on how to further customize the build file please the [documentation](https://seu-as-code.github.io/).

## Building

To build the plugins all you have to do is to issue the following command:
```groovy
	gradlew clean distZip
```

## Contributing

Is there anything missing? Do you have ideas for new features or improvements? You are highly welcome to contribute
your improvements to the SEU-as-Code projects. All you have to do is to fork this repository, improve the code and 
issue a pull request.

## Maintainer

M.-Leander Reimer (@lreimer)

## License

This software is provided under the Apache License, Version 2.0 license.

See the `LICENSE` file for details.
