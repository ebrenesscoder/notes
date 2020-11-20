
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

Under Gradle Scripts, open `build.gradle (Module: app)`.
``` gradle
android {
    compileOptions {
        // ADD THIS
        // to allow your application to make use of Java 8 features like Lambda expressions
        coreLibraryDesugaringEnabled true
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}

dependencies {
    // ADD THESE
    implementation 'com.amplifyframework:aws-api:1.4.1'
    implementation 'com.amplifyframework:aws-datastore:1.4.1'

    // ADD THIS
    coreLibraryDesugaring 'com.android.tools:desugar_jdk_libs:1.0.10'
}
```
Run Gradle Sync

## Generate model files
Switch to project view instead of Android view
Expand the ToDo folder and open the schema file located at `amplify > backend > api > amplifyDatasource > schema.graphql`.

``` graphqk
enum Priority {
  LOW
  NORMAL
  HIGH
}

type Todo @model {
  id: ID!
  name: String!
  priority: Priority
  description: String
}
```

