
# Amplify - Android

## Base project
Create new android project

## Add amplify to your project
Expand Gradle Scripts in the project file viewer and open `build.gradle (Project: Todo)`.

``` gradle
buildscript {
   repositories {
       google()
       jcenter()
   }

   dependencies {
       classpath 'com.android.tools.build:gradle:4.0.1'

       // ADD THIS
       classpath 'com.amplifyframework:amplify-tools-gradle-plugin:1.0.1'
   }
}

allprojects {
   repositories {
       google()
       jcenter()
   }
}

// ADD THIS
apply plugin: 'com.amplifyframework.amplifytools'
```
