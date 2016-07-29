---
layout: post
cover: 'assets/images/crashing_waves.jpg'
title: Introduction to Android
date:   2016-07-16 10:18:00
tags: general programming
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

### Files of Interest

app/build.gradle

Gradle build scripts are a DSL, they come with a top-level build file and build files for each module.

app/src/main/AndroidManifest.xml

This file contains metadata that describes our application ot the OS. File is always named AndroidManifest.xml and lives in the root directory of our project.

When creating new activities, we need to add it to our AndroidManifest file so our OS can access it. The dot at the start of this attribute’s value tells the OS that this activity’s class is in the package specified in the package attribute in the manifest element at the top of the file.

````xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.shawnklein.carpk.cloudorder" >

    <activity android:name=".NewActivity"
              android:label="@string/app_name" />

</manifest>
````


### Activity
An activity is an instance of `Activity` and is responsible for managing user interactions with a screen of information.

widgets: can show text, graphics, interact with user, or arrange other widgets on screen. buttons, text input, check boxes.

`@Override` annotation ensures the class actually has the method you are attempting to override. The compiler will notify you if it does not possess this class.

We start an activity by using the `Activity` method. `public void startActivity(Intent intent)` This sends a call to a part of the OS called the 'ActivityManager', which creates `Activity` instances and calls `onCreate()`. An intent is an object that a component can use to communicate with the OS, which are activities, services, broadcast recievers, and content providers. `public Intent(Context packageContext, Class<?> cls)`

#### Classes

Each class implements or inherits thier own attributes and methods

<table style="width:100%">
  <tr>
    <th>Class</th>
    <th>Inherits from</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>LinearLayout</td>
    <td>< ViewGroup < View < Object</td> 
    <td>"match_parent" view will be as big as parent, "wrap_content" view with be as big as contents</td>
  </tr>
  <tr>
    <td>FrameLayout</td>
    <td>< ViewGroup < View < Object</td> 
    <td>"match_parent" view will be as big as parent, "wrap_content" view with be as big as contents</td>
  </tr>
  <tr>
    <td>TextView</td>
    <td>< View < Object</td> 
    <td></td>
  </tr>
  <tr>
    <td>Button</td>
    <td>< TextView < View < Object</td> 
    <td>determines if children will appear vertically or horizontally</td>
  </tr>
  <tr>
    <td>ImageButton</td>
    <td>< ImageView < View < Object</td> 
    <td>Displays a button with image instead of text.</td>
  </tr>
</table>

#### Attributes

<table style="width:100%">
  <tr>
    <th>Attribute</th>
    <th>Inherits from</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>android:id</td>
    <td></td> 
    <td></td>
  </tr>
  <tr>
    <td>android:orientation</td>
    <td>LinearLayout</td> 
    <td>determines if children will appear vertically or horizontally</td>
  </tr>
  <tr>
    <td>android:layout_width</td>
    <td>TextView</td> 
    <td>"match_parent" view will be as big as parent, "wrap_content" view with be as big as contents</td>
  </tr>
  <tr>
    <td>android:layout_height</td>
    <td>TextView</td> 
    <td>"match_parent" view will be as big as parent, "wrap_content" view with be as big as contents</td>
  </tr>
  <tr>
    <td>android:onClick</td>
    <td>View</td> 
    <td>Invokes context when the view is clicked</td>
  </tr>
  <tr>
    <td>android:text</td>
    <td>TextView</td> 
    <td>string resources are places in a separate strings file, "@string/referenced_text"</td>
  </tr>
  <tr>
    <td>android:drawableRight</td>
    <td>TextView</td> 
    <td>drawable to be drawn to the right of the text</td>
  </tr>
  <tr>
    <td>android:src</td>
    <td>ImageView</td> 
    <td>sets a drawable as the content "@[+][package:]type:name", "#rgb"</td>
  </tr>
</table>

android:name
android:label


### resources

resources are images, audio, and XML files that live in the res/ diretory. we use the resource ID `R.layout.activity_main` to access these files. This ID returns an int set from `class layout` in R.java, this looks something like  `public static final int activity_main=0x7f04001a;`. 

`setContentView(R.layout.activity_main)` is how our `MainActivity.java` file knows which layout to inflate. In `android:id="@+id/false_button"` the `+` is used during the creation of the id.

Android keeps all the strings all in one place, the strings.xml file.

````java
// strings.xml
<resources>
  <string name="hello_toast">Hello!</string>
</resources>
````

public View findViewById(int id) takes ID of widget and returns a View object. We will prefix with "Button" to return a button instead of a view `mTrueButton = (Button)findViewById(R.id.true_button)`

### listening

Android applications are typically event-driven, they start and then wait for an event, such as the user pressing a button. We say it waits by listening for an event, such as `View.OnClickListener` interface.

````java
mTrueButton.setOnClickListener(new View.OnClickListener() {
  @override
  public void onClick(View v) {
    // I do something!
  }
})
````

This listener is implemented as an anonymous inner class. With OnClickListener's sole interface method being onClick(View), we must implement this method.

(If your knowledge of anonymous inner classes, listeners, or interfaces is rusty, you may want to review some Java before continuing or at least keep a reference nearby.)

### toasting

To create a toast, we will call `public static Toast makeText(Context context, int resId, int duration)` from the Toast class. Then use `Toast.show()` to display it.

````java
// MainActivity.java
public void onClick(View v) {
  Toast.makeText(MainActivity.this,
                 R.string.hello_toast,
                 Toast.LENGTH_SHORT).show();
}
````

### generating getters and setters

If we prefix our variable names with a letter `private int mCount;`, we can tell Android Studio to generate getters and setters for us.

File>Other Settings>Default Settings>Editor>Code Style>Java>Code Generation[tab]

For Field, we will set `m` in the Name prefix column;
And for Static field, we set `s` in the same column.

This is going to allow us to generate our methods on the fly. Right click the variable we want to generate `private int mCount;`, select the prefixed variables to generate and press `okay`. We should have populated out class with getters and setters for the prefixed methods we selected.


### resources

Refreneces to a string will begin with `@string/` and refrences for a drawable will begin with `@drawable/`. Drawable resorces have differant files for different dpi screens.

⋅⋅* __mdpi__ medium density screens(~160dpi)
⋅⋅* __hdpi__ high density screens(~240dpi)
⋅⋅* __xhdpi__ extra high density screens(~320dpi)
⋅⋅* __xxhdpi__ extra extra high density screens(~480dpi)
⋅⋅* __xxxhdpi__ extra extra extra high density screens(~640dpi)


### Logs

Android sends log messages to a shared system level log when using `public static int d(String tag, String msg)`. The `d` is for "debug". 

````java
protected void onCreate(Bundle savedInstanceState){
  Log.d("MainActivity", "onCreate(Bundle) has been called");
  ....
````

<table style="width:100%">
  <tr>
    <th>Level</th>
    <th>Medthod</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>Verbose</td>
    <td>v</td> 
    <td></td>
  </tr>
  <tr>
    <td>Debug</td>
    <td>d</td> 
    <td>determines if children will appear vertically or horizontally</td>
  </tr>
  <tr>
    <td>Info</td>
    <td>i</td> 
    <td>"match_parent" view will be as big as parent, "wrap_content" view with be as big as contents</td>
  </tr>
  <tr>
    <td>Warning</td>
    <td>w</td> 
    <td>"match_parent" view will be as big as parent, "wrap_content" view with be as big as contents</td>
  </tr>
  <tr>
    <td>Error</td>
    <td>e</td> 
    <td>Invokes context when the view is clicked</td>
  </tr>
</table>

Some may even take a third argument for an instance of `Throwable`, which logs information about a thrown exception.

````java
try {
  question = mQuestionBank[mCurrentIndex];
} catch (ArrayIndexOutOfBoundsException ex) {
  Log.e(TAG, "Index was oout of bounds", ex);
}
````

### Lifecycle

Upon initially starting the application, our first activity executes the `onCreate()` hook, followed by `onStart()`, and then `onResume()` to gives us our running activity. If we have another activity created, our application executes the `onPause()` method for our current activity, but will resume with `onResume()` once this activey returns to the foreground. The `onStop()` hook occurs when we hit home during the use of our application, if we return, our applicaiton will start up where we initially left. However, the OS can determine that it needs additional resources and can kill these processes on a need be basis. And these activities will execute `onDestroy()` when we are finished with the current activity. Home button will cause the activity to execute `onPause()` and `onStop()`. Using the back button will also call `onDestroy()` which will also destroy any stashed state.

![DNS query diagram](/assets/images/activity_lifecycle.jpg)

Rotating the screen to change its orientation will cause it to change the device configuration, which calls `onDestroy()` and `onCreate()` in order to inflate the new activity with the device configuration.

To persist our data upon changing the device configuration, we will use `protected void onSaveInstanceState(Bundle outState)` method. This method is called by the system before `onPause()`, `onStop()`, and `onDestroy`. And requires the data passed in as as a Bundle object.

````java
private static final String KEY_INDEX = "index";
@Override
public void onSaveInstanceState(Bundle savedInstanceState) {
  super.onSaveInstanceState(savedInstanceState);
  savedInstanceState.putInt(KEY_INDEX, mCurrentIndex);
}
````










