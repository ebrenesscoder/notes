
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

Create a folder named `graphql` under `/app/src/main` next to the `java` folder

Create a file named `aws-directives.graphql` in the folder created in the last step.

Paste this content into the file

``` graphql
directive @model(
    queries: ModelQueryMap
    mutations: ModelMutationMap
    subscriptions: ModelSubscriptionMap
    timestamps: TimestampConfiguration
) on OBJECT
input ModelMutationMap {
    create: String
    update: String
    delete: String
}
input ModelQueryMap {
    get: String
    list: String
}
input ModelSubscriptionMap {
    onCreate: [String]
    onUpdate: [String]
    onDelete: [String]
    level: ModelSubscriptionLevel
}
enum ModelSubscriptionLevel {
    off
    public
    on
}
input TimestampConfiguration {
    createdAt: String
    updatedAt: String
}

directive @connection(keyName: String, fields: [String!]) on FIELD_DEFINITION
directive @key(fields: [String!]!, name: String, queryField: String) on OBJECT
directive @function(name: String!, region: String) on FIELD_DEFINITION
# Streams data from DynamoDB to Elasticsearch and exposes search capabilities.
directive @searchable(queries: SearchableQueryMap) on OBJECT
input SearchableQueryMap { search: String }
directive @versioned(versionField: String = "version", versionInput: String = "expectedVersion") on OBJECT

directive @http(method: HttpMethod, url: String!, headers: [HttpHeader]) on FIELD_DEFINITION
enum HttpMethod { PUT POST GET DELETE PATCH }
input HttpHeader {
    key: String
    value: String
}

directive @predictions(actions: [PredictionsActions!]!) on FIELD_DEFINITION
enum PredictionsActions {
    identifyText # uses Amazon Rekognition to detect text
    identifyLabels # uses Amazon Rekognition to detect labels
    convertTextToSpeech # uses Amazon Polly in a lambda to output a presigned url to synthesized speech
    translateText # uses Amazon Translate to translate text from source to target language
}

# When applied to a type, augments the application with
# owner and group-based authorization rules.
directive @auth(rules: [AuthRule!]!) on OBJECT | FIELD_DEFINITION
input AuthRule {
    allow: AuthStrategy!
    provider: AuthProvider
    ownerField: String # defaults to "owner" when using owner auth
    identityClaim: String # defaults to "username" when using owner auth
    groupClaim: String # defaults to "cognito:groups" when using Group auth
    groups: [String]  # Required when using Static Group auth
    groupsField: String # defaults to "groups" when using Dynamic Group auth
    operations: [ModelOperation] # Required for finer control

    # The following arguments are deprecated. It is encouraged to use the 'operations' argument.
    queries: [ModelQuery]
    mutations: [ModelMutation]
}
enum AuthStrategy { owner groups private public }
enum AuthProvider { apiKey iam oidc userPools }
enum ModelOperation { create update delete read }

# The following objects are deprecated. It is encouraged to use ModelOperations.
enum ModelQuery { get list }
enum ModelMutation { create update delete }

```

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
 ## Create a Todo
 Next, you’ll create a Todo and save it to DataStore.
 Open `MainActivity` and add the following code to the bottom of the `onCreate()` method:
 ```java
 Todo item = Todo.builder()
       .name("Build Android application")
       .description("Build an Android application using Amplify")
       .build();
```

Below that, add the code to save the item to DataStore:

 ``` java
 Amplify.DataStore.save(
         item,
         success -> Log.i("Tutorial", "Saved item: " + success.item().getName()),
         error -> Log.e("Tutorial", "Could not save item to DataStore", error)
 ); 
 ```

 ## Query Todos
 Now that you have some data in DataStore, you can run queries to retrieve those records.
 
 ```java
 Amplify.DataStore.query(
       Todo.class,
       todos -> {
           while (todos.hasNext()) {
               Todo todo = todos.next();

               Log.i("Tutorial", "==== Todo ====");
               Log.i("Tutorial", "Name: " + todo.getName());

               if (todo.getPriority() != null) {
                   Log.i("Tutorial", "Priority: " + todo.getPriority().toString());
               }

               if (todo.getDescription() != null) {
                   Log.i("Tutorial", "Description: " + todo.getDescription());
               }
           }
       },
       failure -> Log.e("Tutorial", "Could not query DataStore", failure)
);
```

Queries can also contain predicate filters. These will query for specific objects matching a certain condition.

The following predicates are supported:

Strings

eq ne le lt ge gt contains notContains beginsWith between

Numbers

`eq` `ne` `le` `lt` `ge` `gt` `between`

Lists

`contains` `notContains`

To use a predicate, pass an additional argument into your query. For example, the following code queries for all high priority items:

``` java
Amplify.DataStore.query(
       Todo.class,
       Where.matches(
           Todo.PRIORITY.eq(Priority.HIGH)
       ),
       todos -> {
           while (todos.hasNext()) {
               Todo todo = todos.next();

               Log.i("Tutorial", "==== Todo ====");
               Log.i("Tutorial", "Name: " + todo.getName());

               if (todo.getPriority() != null) {
                   Log.i("Tutorial", "Priority: " + todo.getPriority().toString());
               }

               if (todo.getDescription() != null) {
                   Log.i("Tutorial", "Description: " + todo.getDescription());
               }
           }
       },
       failure -> Log.e("Tutorial", "Could not query DataStore", failure)
);
```

 ## Connect to the cloud
 Now that your have DataStore persisting data locally, in the next step you’ll connect it to the cloud. With a couple of commands, you’ll create an AWS AppSync API and configure DataStore to synchronize its data to it.

Make sure you have configure an aws profile.

Edit the file `amplify-gradle-config.json` and change the profile value with yours and the `envName` to `prod`

Uncomment the line `Amplify.addPlugin(new AWSApiPlugin());` in the class `MainActivity.java`
In the Gradle Task dropdown menu in the toolbar, select app and run the application. This will synchronize the existing local Todo items to the cloud. DataStore.observe will log a message when new items are synchronized locally.
When doing this sync a new API will appear in the App Sync console.

Next, push your new API to AWS. In Android Studio, click the Gradle Task dropdown in the toolbar and select amplifyPush.

Modify your initialization code to initialize API in order to connect to the backend. Open `MainActivity` and remove all of the previous code you entered that saved and queried for Todo items. Now, add the following code to the bottom of the `onCreate()` method:

``` java
Amplify.DataStore.observe(Todo.class,
       started -> Log.i("Tutorial", "Observation began."),
       change -> Log.i("Tutorial", change.item().toString()),
       failure -> Log.e("Tutorial", "Observation failed.", failure),
       () -> Log.i("Tutorial", "Observation complete.")
);
```


``` graphql
 query GetTodos {
     listTodos {
         items {
             id
             name
             priority
             description
         }
     }
 }
 ```
 
 
 ``` graphql
  mutation CreateTodo {
     createTodo(
         input: {
             name: "Tidy up the office"
             description: "Organize books, vacuum, take out the trash"
             priority: NORMAL
         }
     ) {
         id
         name
         description
         priority
         _version
         _lastChangedAt
     }
 }
 ```
