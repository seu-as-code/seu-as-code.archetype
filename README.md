[![Build Status](https://travis-ci.org/seu-as-code/seu-as-code.archetype.svg)](https://travis-ci.org/seu-as-code/seu-as-code.archetype)
[![Download](https://api.bintray.com/packages/seu-as-code/generic/seuac-archetype/images/download.svg) ](https://bintray.com/seu-as-code/generic/seuac-archetype/_latestVersion)
[![Stories in Ready](https://badge.waffle.io/seu-as-code/seu-as-code.archetype.png?label=ready&title=Ready)](https://waffle.io/seu-as-code/seu-as-code.archetype)
[![Stories in Progress](https://badge.waffle.io/seu-as-code/seu-as-code.archetype.png?label=in%20progress&title=In%20Progress)](https://waffle.io/seu-as-code/seu-as-code.archetype)
[![Apache License 2](http://img.shields.io/badge/license-ASF2-blue.svg)](https://github.com/seu-as-code/seu-as-code.archetype/blob/master/LICENSE)
[![Join on Chat](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/seu-as-code/seu-as-code?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

# SEU-as-Code Archetype

  * SEU: German for software development environment (Software Entwicklungs-Umgebung)
  * as Code: to be able to configure, build and program the SEU as source code
  
The SEU-as-Code archetype is a quickstart template for new projects. The archetype is continuously evolved to
reflect the latest changes and enhancements.

## Quickstart

So you decided to build your next SEU for a new project as code. **Excellent choice!**
 
1. Download the latest SEU-as-Code Archetype distribution from either [Bintray](https://bintray.com/seu-as-code/generic/seuac-archetype/_latestVersion)
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

For a more detailed description on how to further customize the build file please see the [documentation](https://seu-as-code.github.io/).

## Developing with SEU-as-Code

### Project Layout
The default SEU-as-Code project comes with a predefined project layout. In essence, it is nothing more than a Gradle project.
The project contains a `build.gradle` file and brings it's own Gradle wrapper. Additionally, there are the following directories:

Directory name | Description
--- | ---
*gradle/* | This directory initially contains the Gradle wrapper bootstrap files for the SEU. All downloaded software packages and the `GRADLE_USER_HOME` directory will also be located in this directory.
*native/* | Contains the native DLLs for Scriptom. Mainly required by the hook scripts for basic Windows interaction.
*repo/* | Flat repository used for custom SEU specific software and home dependencies. Mainly used for additional customization.
*scripts*/ | Contains addidional build script fragments that can be included in the main `build.script`. See the section on `Script customization` for further details.


## Dependency Customization
All software artifacts and packages you can install in your SEU are managed as ordinary dependencies. Most of these are
located in the [Bintray Maven repository](https://bintray.com/seu-as-code/maven).

To determine where a software package will be installed we use Gradle configurations. Currently, the `seauc-base` plugin ships
with three inbuilt configurations:

Configuration | Description
--- | ---
*seuac* | This is a special configuration used for dependencies that need to be put in the Groovy root classloader, such as Scriptom or JDBC drivers.
*software* | The main configuration used for all packages that should be installed into the `software` directory specified by the SEU layout.
*home* | The configuration used to install packages into the user `home` directory of the SEU.

You use these configurations when you define your software dependencies on your SEU `build.gradle` file:
```groovy
dependencies {
    // dependencies for the Groovy root classloader
    seuac 'org.codehaus.groovy.modules.scriptom:scriptom:1.6.0'
    seuac 'com.h2database:h2:1.3.176'

    // mandatory dependencies for basic SEU setup
    home 'de.qaware.seu.as.code:seuac-home:2.0.0'
    software 'de.qaware.seu.as.code:seuac-environment:2.0.0:jdk8'

    // additional dependencies for a Groovy development environment
    software 'org.gradle:gradle:2.5'
    software 'org.groovy-lang:groovy:2.4.4'
}
```

You also have to option to create your own customized software packages and distribute these along with your SEU. Usually
you want your own custom project banner when you start a Console windows, and you might also want to change the default
JDK version. You can take the `de.qaware.seu.as.code:seuac-environment:2.0.0` JAR as a starting point for your own package.
Once you have customized this package, give it a different name, create a ZIP file and put it into the `repo` directory of
your SEU. Have a look at some of the [SEU-as-Code examples](https://github.com/seu-as-code/seu-as-code.examples).

You should refer to the official Gradle documentation for more information on handling dependencies:
[Dependency Management Basics](http://www.gradle.org/docs/current/userguide/artifact_dependencies_tutorial.html)


### Build Tasks
Per default, the following plugins are applied to the SEU template build file: `base`, `seuac-base`, `seuac-svn`, `seuac-git`.
Please refer to the individual plugin documentations for a complete list of tasks these plugin provide. To get a list of
the available tasks you can also issue the following command:
```groovy
gradlew tasks
```
You can add the `--all` command line parameter to see the complete list of all tasks, including non-public utility tasks.


### Graphical User Interface
In addition to interacting with your SEU-as-Code project using the traditional command line interface, Gradle also offers
a graphical user interface you can use:
```groovy
gradlew --gui
```
Of course, you can also import the SEU project into your favourite IDE as a Gradle project.


### Script Customization
You will probably extend the basic build script with additional functionality required by your project. You may need
to add a Git repository, interact with an application server or database. It is good practice to separate your build
script into small and manageable pieces. For example, with the template project you will get a `scripts/svn.gradle`
files with a possible SVN repository configuration. You only need to include and apply the extra scripts on your main
`build.gradle` file:
```groovy
apply from: 'scripts/svn.gradle'
```


### Task Customization
To customize your SEU even further you can write your own Gradle task definitions to copy files or do whatever you
need your SEU to do. The good thing is that you have the full Groovy power at your hands as well as loads of already
inbuilt Gradle tasks and plugins to help you with this. A simple hello task might look like this:
```groovy
task helloSeu << {
    println 'Hello SEU-as-Code!'
}
```

You should refer to the official Gradle documentation for more information on this. Here is a list of interesting and
helpful links on the topic:
- [Build Script Basics](http://www.gradle.org/docs/current/userguide/tutorial_using_tasks.html)
- [Writing Custom Task Classes](http://www.gradle.org/docs/current/userguide/custom_tasks.html)
- [Gradle Build Language Reference](http://www.gradle.org/docs/current/dsl/)
- [Gradle User Guide](http://www.gradle.org/docs/current/userguide/userguide.html)


## Building

To build the archetype archive all you have to do is to issue the following command:
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
