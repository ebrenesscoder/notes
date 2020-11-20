
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
    defaultConfig {
      ...
      // ASS THIS
      multiDexEnabled true
      ...
    }
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

``` graphql
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
Next, generate the classes for these models. In Android Studio, click the Gradle Task dropdown in the toolbar and select `modelgen`. This generates java classes, like `Todo.java`, `Priority.java (enum)`

## Configure DataStore
Create a new class next to `MainActivity` and name it `MyAmplifyApplication`
Paste the following code to initialize amplify
``` java
 public class MyAmplifyApplication extends Application {
   @Override
     public void onCreate() {
         super.onCreate();
         try {
             // Amplify.addPlugin(new AWSApiPlugin());
             Amplify.addPlugin(new AWSDataStorePlugin());
             Amplify.configure(getApplicationContext());

             Log.i("Tutorial", "Initialized Amplify");
         } catch (AmplifyException e) {
             Log.e("Tutorial", "Could not initialize Amplify", e);
         }
     }
 }
```
Open `AndroidManifest.xml` to configure your application.
Add `xmlns:tools="http://schemas.android.com/tools"` to the `manifest` node.
Add the `android:name` and `tools:replace` attributes to the `application` node:

```xml
<application
   android:name=".MyAmplifyApplication"
   tools:replace="android:name"
   ...
```

In the Gradle Task dropdown menu in the toolbar, select app, and run the application. In logcat, you’ll see a log line indicating success:
```
 com.example.todo I/Tutorial: Initialized Amplify
 ```
 

