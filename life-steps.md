simple steps
============

We will be using Gradle, IntelliJ IDEA Community Edition, and Git to run through our build. Since we're working on Mac, we will use Homebrew to ensure that we have the tools necessary for building.

Verify you have the following downloaded and installed before getting started:

* [Java 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [IntelliJ IDEA Community Edition](https://www.jetbrains.com/idea/download/)
* [Homebrew](http://brew.sh/)

Project Init
------------

Start off by making a new directory to house your code for the project. All further instructions will be based on the assumption that you are working in this directory.

    $ mkdir life
    $ cd life

Gradle
------

Gradle is a build tool like Ant. Its configuration file is a Domain Specific Language (DSL) based on Groovy. This means that you are technically writing code when you are setting up your build script.

Add the following to a `build.gradle` file:

    apply plugin: 'java'
    apply plugin: 'idea'

    repositories {
        mavenLocal()
        mavenCentral()
    }

This is a super barebones build file which will allow you to compile and run your Java program.

Now lets make a class for you to actually run. Run the following commands, verifying that you know what they do.

    $ mkdir -p src/main/java/org/life
    $ cat >> src/main/java/org/life/Life.java <<JAVA
    package org.life;
    
    public class Life {
        public static void main( String[] args ) {
            System.out.println("Hello, world!");
        }
    }
    JAVA

The first command creates the directories which will hold the code for the project. The second command creates the initial main method for the application.
Let's see if everything worked:

    $ gradle build
    $ java -cp build/classes/main/ org.life.Life
    $ java -cp build/libs/life.jar org.life.Life

You'll notie that the gradle build created a `jar` file for you in addition to compiling the class we wrote earlier. Also note that the jar is named `life.jar`. That is because the jar file is named for the project. If you want to change the name of the project (and therefore the jar file), you can create a `settings.gradle` file and add `rootProject.name = 'other'` to the file.

To make our building/running lives easier, I'd like to make a nice simple single command which builds and runs the program all at once. Fortunately, Gradle has something for this, no problem. Add the following to the end of the `build.gradle` file:

    task run( type: JavaExec, dependsOn: build ) {
        description 'Executes our Game of Life program'
        main = 'org.life.Life'
        classpath sourceSets.main.runtimeClasspath
        classpath configuration.runtime
    }

Now you should be able to run `gradle run` on the command line and see the compilation and execution of the program.

    $ gradle run


[comment]: # (Git and Github)
[comment]: # (--------------)

[comment]: # (*NOTE* This step can be done later or ignored entirely. It will be useful for other things.)

[comment]: # (Get a [Github](https://github.com) set up)


IntelliJ Setup
--------------

Now that we can easily run the program from the command line and are tracking changes we make, let's get set up to use the fancy-shamncy IDE. Start off by running `gradle idea`. This will set up the base IntelliJ IDEA configuration files based on the work in the gradle file and the project layout.

    $ gradle idea

Now, start up IntelliJ.
