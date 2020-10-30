# HocFireBase1

A new Flutter project.

## Getting Started

### Create project

```bash
flutter create HocFireBase1
```

### Firebase create android app with "package name"

- Get "package name in HocFireBase1 Project path: *Android\app\src\main\AndroidManifest.xml*

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
package="com.example.HocFireBase1">
```

- Download Android config file *google-services.json* and put into *android/app/*

### Edit android/build.gradle

```config
buildscript {
  repositories {
    // Check that you have the following line (if not, add it):
    google()  // Google's Maven repository
  }
  dependencies {
    ...
    // Add this line
    classpath 'com.google.gms:google-services:4.3.3'
  }
}

allprojects {
  ...
  repositories {
    // Check that you have the following line (if not, add it):
    google()  // Google's Maven repository
    ...
  }
}
```

### Edit android\app\build.gradle

```config
apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply plugin: 'com.google.gms.google-services' //this line
```

- add package in pubspec.yaml
-- firebase_core
-- firebase_database

```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(MyApp());
}
```

```dart
class _MyHomePageState extends State<MyHomePage> {
  final fb = FirebaseDatabase.instance;
  final myController = TextEditingController();
  final name = "Name";

  @override
  Widget build(BuildContext context) {
    final ref = fb.reference();
    return Scaffold(
        appBar: AppBar(
          title: Text(widget.title),
        ),
        body: Container(
            child: Column(
          children: <Widget>[
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: <Widget>[
                Text(name),
                Flexible(child: TextField(controller: myController)),
              ],
            ),
            RaisedButton(
              onPressed: () {
                ref.child(name).set(myController.text);
              },
              child: Text("Submit"),
            )
          ],
        )));
  }

  @override
  void dispose() {
    // Clean up the controller when the widget is disposed.
    myController.dispose();
    super.dispose();
  }
}
```

```dart
var retrievedName;

            RaisedButton(
              onPressed: () {
                ref.child("Name").once().then((DataSnapshot data){
                  print(data.value);
                  print(data.key);
                  setState(() {
                    retrievedName = data.value;
                  });
                });
              },
              child: Text("Get"),
            ),
            Text(retrievedName ?? "name"),

```
