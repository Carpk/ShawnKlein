---
layout: post
cover: 'assets/images/crashing_waves.jpg'
title: Introduction to Android
date:   2016-07-16 10:18:00
tags: general coding java
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

This introduction serves as a reference guide for people new to developing Android applications. 

### Files

###### app/build.gradle

Gradle build scripts are a DSL, they come with a top-level build file and build files for each module.

###### app/src/main/AndroidManifest.xml

This file contains metadata that describes our application ot the OS. File is always named AndroidManifest.xml and lives in the root directory of our project.

When creating new activities, we need to add it to our AndroidManifest file so our OS can access it. The dot at the start of this attribute’s value tells the OS that this activity’s class is in the package specified in the package attribute in the manifest element at the top of the file.

````xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="net.shawnklein.carpk.testapp" >
    <activity android:name=".NewActivity"
              android:label="@string/app_name" />
</manifest>
````
This file also tells the OS which Activity is the launcher activity, that is the activity that first starts when the OS calls on the application. The manifest declaration specifies this with `intent-filter` element (pg109).

###### app/src/main/java/tld/company/user/project/*.java

These files hold the code that preforms the functionality we need our app to accomplish.

###### app/src/main/res/layout/*.xml

These files hold widgets that interprept what our activities will look like.

###### app/build/generated/source/r/debug/tld/company/user/project/R.java

The `R.java` file is our resources file.

````java
public static final class string {
    public static final int abc_action_bar_home_description=0x7f060000;
    public static final int abc_action_bar_home_description_format=0x7f060001;
    public static final int abc_action_bar_home_subtitle_description_format=0x7f060002;
    public static final int abc_action_bar_up_description=0x7f060003;
}
````    

### Activities
An activity is an instance of the `Activity` class and is responsible for managing user interactions with a screen of information.

Inside an activity are widgets: can show text, graphics, interact with user, or arrange other widgets on screen. buttons, text input, check boxes.

`@Override` annotation ensures the class actually has the method you are attempting to override. The compiler will notify you if it does not possess this class.

A `@TargetApi(11)` annotation is used to suppress build errors from Lint. It is used to say that the below method will only be referenced by an OS version that is passed in as an argument. In our case, version `11`, Honeycomb, instead of the version what is declared in our manifest.

We start an activity by using the `startActivity` method. `public void startActivity(Intent intent)` This sends a call to a part of the OS called the 'ActivityManager', which creates `Activity` instances and calls `onCreate()`. An intent is an object that a component can use to communicate with the OS, which are activities, services, broadcast recievers, and content providers. `public Intent(Context packageContext, Class class)` the Class specifies which activity the ActivityManager should start, Context tells which package the Class object can be found in. We can add additional information to the intent by calling `public Intent putExtra(String name, boolean value)`, this allows us to pass information between our current activity and the one will will be creating.

````java
// MainActivity.java
Intent i = new Intent(MainActivity.this, SideActivity.class);
i.putExtra(SideActivity.MY_EXTRA_VALUE, myValue);
startActivity(i);
// SideActivity.java
public static final String MY_EXTRA_VALUE = "net.ShawnKlein.testapp";
mMyValue = getIntent().getBooleanExtra(MY_EXTRA_VALUE, false);
````

The ActivityManager will then check the package's manifest for a declaration with the same name as the specified Class. If no declaration is found, it will respond with a ActivityNotFoundException. We call `Activity.getIntent()` to return the intent that started this activity and `public boolean getBooleanExtra(String name, boolean defaultValue)` to pull the information out of the intent. It is also important to note that we should define keys for extras on the activities that retrieve and use them, we do this with `public static final String MY_EXTRA_VALUE = "net.ShawnKlein.testapp";`.

The above example is a an _explicit intent_, when an activity wants to start an activity in another application, it would be considered and _implicit intent_.

For our child activity to return data back to our main activity, we will use `public void startActivityForResult(Intent intent, int requestCode)` instead of `startActivity(i)` and `protected void onActivityResult(int requestCode, int resultCode, Intent data)` to actually retrive the returning data. The Child activity will use `public final void setResult(int resultCode)` or `public final void setResult(int resultCode, Intent data)`, if no result code is set, and the parent expects a return, the OS will send RESULT_CANCELED as the `resultCode`. 

````java
// SideActivity.java
public static final String RETURNING_DATA =
    "net.shawnklein.testapp.data_result"
private void setReturningDataResult(boolen myValue) {
    Intent data = new Intent();
    data.putExtra(RETURNING_DATA, myValue);
    setResult(RESULT_OK, data);
}
// MainActivity.java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    myVariable = data.getBooleanExtra(Sideactivity.RETURNING_DATA, false);
}
````

To retrieve our returing data, we use `protected void onActivityResult()`

### Fragments

Is a controller object that an activity can deputize to perform tasks. Such as managing a user interface. To set our class to use a fragment, we replace `Activity` with `FragmentActivity`. We use `getSuportFragmentManager()` to maintain a bakc stack of fragment transactions. ....pg142...... We use a fragment transaction with `FragmentManager.beginTransaction()` returns an instance of FragmentTransaction, which we use to add, remove, attach, detach, or replace fragments in the fragment list. Our `add()` method takes 2 parameters, the container view ID and our TestFragment. The container view ID tells the FragmentManager where the fragment view should appear and is its unique identifier in the FragmentManager's list.

````java
public class TestActivity extends FragmentActivity {
  @Override
  protected void onCreate(Bundle savedInstanceState) {
    FragmentManager fm = getSupportFragmentManager();
    Fragment fragment = fm.findFragmentById(R.id.fragmentContainer);
    if (fragment == null) {
      fragment = new TestFragment();
      fragment.beginTransaction().add(R.id.fragmentContainer, fragment).commit();
    }
  }
}
````

A workflow is as follows, we ask FragmentManager for fragment, if in list, FragmentManager will return it. If `null`, a new CrimeFragment will be created.

PAUSE on PAGE 145



Our fragment class does not inflate using the `onCreate()` such as activity would, it inflates using the `onCreateView()` method. and we explicitly inflate the the fragments view by calling `LayoutInflater.inflate()`. The first parameter is the resource ID, second is the parent's view, and third tells the inflater whether to add the inflated view to the parent.

````java
public class TestFragment extends Fragment {
  // ....
  @Override
  public View onCreateView(LayoutInflater inflater, ViewGroup parent, Bundle savedInstanceState) {
    View v = inflater.inlfate(R.layout.fragment_test, parent, false);
    return v;
  }
}
````


Now that we have created our fragment, we need 2 things to host a UI fragment, a spot in the layout for the fragment's view, and manage the lifecycle of the fragment. Fragments lifecycle are called by the hosting activity, instead of the OS. You can add a fragment to either the hosting activity's _layout_ or _code_.

###### Layout

A layout fragment is simple, but inflexable. We hard code the fragment and its view to the activity's view and cannot swap out the fragment during the activity's lifetime.



###### Code

A code fragment gives us more control of how our fragment will interact with our activity. It will allow us to determine when to add the fragment to the host activity, remove it, replace it with another, and add the initial fragment back again.







### Classes

Classes are found in both the `.java` and `.xml` files. We are able to reference the `.xml` widgets using `findViewById(int id)` method in our `.java` file. Each class implements or inherits thier own attributes and methods

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

### Attributes

Attributes are found will go inside our class widgets, they describe how the widget should function.

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
  <tr>
    <td>android:name</td>
    <td></td> 
    <td></td>
  </tr>
  <tr>
    <td>android:label</td>
    <td></td> 
    <td></td>
  </tr>
  <tr>
    <td>android:padding</td>
    <td></td> 
    <td>padding describes how much bigger the widget's size is over the content</td>
  </tr>
  <tr>
    <td>android:layout_margin</td>
    <td></td> 
    <td>margin is used by parent widget to space the calling widget from other widgets</td>
  </tr>
  <tr>
    <td>android:layout_weight</td>
    <td></td> 
    <td>In regards to only 2 widgets, if both values are the same, it wil split the space 50/50. if 2 and 1, 2/3 and 1/3 respectively</td>
  </tr>
</table>

Attributes that begin with `layout_` are directed to be used by the widget's parent, these are known as _layout parameters_. When `layout_` is ommited, the widget calls a method to configure itself based on the attribute and its value.

<table style="width:100%">
  <tr>
    <th>Unit</th>
    <th>Size</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>dp or dip</td>
    <td>density independent pixel</td> 
    <td>find more of me on page 155</td>
  </tr>
  <tr>
    <td>sp</td>
    <td>scale independent pixel</td> 
    <td>density dependent pixels take into account user font preference, used for text size</td>
  </tr>
  <tr>
    <td>pt, mm, in</td>
    <td>points, millimeters, inches</td> 
    <td>Scaled units(points are 1/72 of an inch)</td>
  </tr>
</table>



### Resources

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

Other files 

Refreneces to a string will begin with `@string/` and refrences for a drawable will begin with `@drawable/`. Drawable resorces have differant files for different dpi screens.

* __mdpi__ medium density screens(~160dpi)
* __hdpi__ high density screens(~240dpi)
* __xhdpi__ extra high density screens(~320dpi)
* __xxhdpi__ extra extra high density screens(~480dpi)
* __xxxhdpi__ extra extra extra high density screens(~640dpi)

### Listening

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

### Toasting

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

It is idiomatic to have a prefix of `s` for static interfaces. Such as `sCommonVariable`.


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

### Saving State

To persist our data upon changing the device configuration, we will use `protected void onSaveInstanceState(Bundle outState)` method. This method is called by the system before `onPause()`, `onStop()`, and `onDestroy`. And requires the data passed in as as a Bundle object.

````java
private static final String KEY_INDEX = "index";
@Override
public void onSaveInstanceState(Bundle savedInstanceState) {
  super.onSaveInstanceState(savedInstanceState);
  savedInstanceState.putInt(KEY_INDEX, mCurrentIndex);
}
````









 
