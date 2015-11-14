simple steps
============

We will be using Gradle, IntelliJ IDEA Community Edition, and Git to run through our build.
Since we're working on Mac, we will use Homebrew to ensure that we have the tools necessary for building.

Verify you have the following downloaded and installed before getting started:

* [Java 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [IntelliJ IDEA Community Edition](https://www.jetbrains.com/idea/download/)
* [Homebrew](http://brew.sh/)

Project Init
------------

Start off by making a new directory to house your code for the project.
All further instructions will be based on the assumption that you are working in this directory.

    $ mkdir life
    $ cd life

Gradle
------

Gradle is a build tool like Ant.
Its configuration file is a Domain Specific Language (DSL) based on Groovy.
This means that you are technically writing code when you are setting up your build script.

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

The first command creates the directories which will hold the code for the project.
The second command creates the initial main method for the application.
Let's see if everything worked:

    $ gradle build
    $ java -cp build/classes/main/ org.life.Life
    $ java -cp build/libs/life.jar org.life.Life

You'll notice that the gradle build created a `jar` file for you in addition to compiling the class we wrote earlier.
Also note that the jar is named `life.jar`.
That is because the jar file is named for the project.
If you want to change the name of the project (and therefore the jar file), you can create a `settings.gradle` file and add `rootProject.name = 'other'` to the file.

To make our building/running lives easier, I'd like to make a nice simple single command which builds and runs the program all at once.
Fortunately, Gradle has something for this, no problem.
Add the following to the end of the `build.gradle` file:

    task run( type: JavaExec, dependsOn: build ) {
        description 'Executes our Game of Life program'
        main = 'org.life.Life'
        classpath sourceSets.main.runtimeClasspath
        classpath configuration.runtime
    }

Now you should be able to run `gradle run` on the command line and see the compilation and execution of the program.

    $ gradle run

Additionally, you can run `gradle tasks` to see all of the tasks set up and you should find our `run` task at the end of the list in the `Other` section.

    $ gradle tasks


[comment]: # (Git and Github)
[comment]: # (--------------)

[comment]: # (*NOTE* This step can be done later or ignored entirely. It will be useful for other things.)

[comment]: # (Get a [Github](https://github.com) set up)


IntelliJ Setup
--------------

Now that we can easily run the program from the command line, let's get set up to use the fancy-schmancy IDE.
Start off by running `gradle idea`.
This will set up the base IntelliJ IDEA configuration files based on the work in the gradle file and the project layout.

    $ gradle idea

Now, start up IntelliJ.
If this is the first time you have used IntelliJ, you will probably need to configure both Gradle and Java environments.

From the start screen, select the `Configure` menu and select `Project Defaults` -> `Project Structure`.
In the dialog which pops up, find the `Platform Settings` -> `SDKs` section.
You shouldn't have anything in here yet, so let's add some.
Click the `+` at the top and select a JDK.
Most likely, you will have the latest JDK on your system automatically selected, so you can just press `OK`.
Next, go to the `Project Settings` -> `Project` section and select the JDK you just set up where it says `<No SDK>` currently.
This will default new projects to the correct JDK when you start up.
Also select the default language level of `8 - Lambdas, type annotations, etc.`
That should be everything for this screen.
Select `OK` to head back to the startup screen.

Now you will open the project.
You should be able to find an "Open..." option directly in front of you or from the "File" menu.
Navigate to the `life` directory you've been working in and open the project (select the directory and then "Open").
You will get a new coding window set up for your project.
In the top right corner, you will probably see a prompt saying that you should link up the Gradle project.
This is exacty what you want to do; IntelliJ keeps its own internal caches and indexes on everything in your project, so it asks to link itself to the files we already created.
Select the `Import Gradle project` option in the popup; this will open a dialog to handle the importing.

Most of the defaults here are fine but you will probably have to set the Gradle home for the project.
If you installed gradle with Homebrew, you should set the Gradle home to `/usr/local/Cellar/gradle/2.8/libexec/`.

The project is now ready to set up a run configuration inside of IntelliJ.
Go the the `Run` menu and select `Edit Configurations...`
Add a new configuration with the `+` option in the top left.
Select `Gradle` from the resulting menu; this will add a new `Unnamed` Gradle execution.
Start off by renaming the item to `Gradle Run`.
Select the `Single instance only` option in the top right.
Select the `life` project we've been working on by clicking on the folder-ish icon next to the `...` button.
Enter `run` for the tasks to run.
That is everything; click `OK` then go back to the `Run` menu and select `Run 'Gradle Run'` to try it out.
The first time I ran it, it took a long time to execute the first time, but subsequent runs were very fast.

The run configuration set up above just executes the same thing that we were running form the command line.
However, this won't allow debugging.
Let's set up another config to allow debugging.
Add another run configuration by going to the `Run` menu -> `Edit Configurations...` -> `+` and select `Application` this time.
Rename to something like `Life`.
For the `Main class`, type in `org.life.Life`; you may notice an auto-complete pop up while you are typing.
That's all you need; click `OK` and run it.


Actually Building Shit
----------------------

Now that we can build, run, and debug our project, we can get to actually building shit.

TODO


