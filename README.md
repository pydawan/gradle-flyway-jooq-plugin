gradle-jooq-flyway-plugin
==================

# Overview
[Gradle](http://www.gradle.org) plugin that improves integration of [jOOQ](http://www.jooq.org) and [Flyway](http://www.flywaydb.org)
plugins by improving Gradle uptodate check on jOOQ task adding check of currently applied Flyway scripts.
For each jOOQ task, the plugin adds dependency to Flyway by checking information wheather database schema changed or not since last generation. 
If you rely on Flyway to manage database changes and using jOOQ same time - jOOQ plugin will regenerate your classes every time 
you changed the database with flywayMigrate task.  

# Plugin

## General
The flywayJooq plugin relies on two great plugins: [flyway-gradle-plugin](https://github.com/flyway/flyway) by Axel Fontaine 
and [gradle-jooq-plugin](https://github.com/etiennestuder/gradle-jooq-plugin) by Etienne Studer. 
When both of them are applied flywayJooq adds an extra dependency to the jOOQ task that checks if there are any differences in applied scripts.

The plugin is hosted at [Bintray's JCenter](https://bintray.com/wstrzalka/gradle-plugins/gradle-flyway-jooq-plugin).

## Gradle 1.x and 2.x
To use the jOOQ plugin with versions of Gradle 1.x and 2.x, apply the plugin in your `build.gradle` script:

```groovy
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'pl.codelabs:gradle-flyway-jooq-plugin:1.0.0'
    }
}
apply plugin: 'pl.codelabs.flywayJooq'
```

# Tasks
For each jOOQ configuration declared in the build, the plugin adds dependency to :flywayJooqInfoFile task that dumps out list of applied scripts into a file.
the format of the file is identical to flywayInfo (as it's generated by Flyway utils) with the difference only scripts applied to database are there.

```console
>gradle generateSampleJooqSchemaSource
:flywayInfoFile                                                                  
:generateSampleJooqSchemaSource UP-TO-DATE      
               
BUILD SUCCESSFUL

>gradle flywayMigrate 
:flywayInfoFile                                                                  
:generateSampleJooqSchemaSource UP-TO-DATE      
:compileJava UP-TO-DATE                                          
:processResources UP-TO-DATE      
:classes UP-TO-DATE      
:compileTestJava UP-TO-DATE                                              
:processTestResources UP-TO-DATE      
:testClasses UP-TO-DATE      
:flywayMigrate                                                                    

>gradle generateSampleJooqSchemaSource
:flywayInfoFile                                                                  
:generateSampleJooqSchemaSource
               
BUILD SUCCESSFUL
```

# Configuration

The plugin doesn't need any configuration but if you really need you can reconfigure the temporary file generated by the :flywayJooqInfoFile task

By default, the generated file is written to `build/tmp/flywayJooqInfoFile/flywayJooq.info`.

flywayJooq {
   infoFile 'flywayJooq.info'
}

# License

This plugin is available under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0.html).

(c) by Etienne Studer
