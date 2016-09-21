---
layout: post
cover: 'assets/images/crashing_waves.jpg'
title: Introduction to Android
date:   2016-07-16 10:18:00
tags: coding java
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

This introduction covers some fundamental topics for developing on Android devices. Its goal is to help you piece enough together to create some basic application. 

### Files

###### app/build.gradle

Gradle build scripts are a DSL, they come with a top-level build file and build files for each module.

###### app/src/main/AndroidManifest.xml

This file contains metadata that describes our application ot the OS. File is always named AndroidManifest.xml and lives in the root directory of our project.

When creating new activities, we need to add it to our AndroidManifest file so our OS can access it. The dot at the start of this attribute’s value tells the OS that this activity’s class is in the package specified in the package attribute in the manifest element at the top of the file.

````xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="net.shawnklein.carpk.testapp" >
    <application>
        <activity android:name=".MainActivity"
                  android:label="@string/app_name" />
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
    </application>
</manifest>
````
This file also tells the OS which Activity is the launcher activity, that is the activity that first starts when the OS calls on the application. The manifest declaration specifies this with `intent-filter` element.

###### app/src/main/java/tld/company/user/project/*.java

These files hold the code that preforms the functionality we need our app to accomplish.

###### app/src/main/res/layout/*.xml

These files hold widgets that interprept what our activities will look like.

###### app/build/generated/source/r/debug/tld/company/user/project/R.java

The `R.java` file is our resources file. It contains all the resource IDs for all the resources in our res/ directory.

````java
public static final class string {
    public static final int abc_action_bar_home_description=0x7f060000;
    public static final int abc_action_bar_home_description_format=0x7f060001;
    public static final int abc_action_bar_home_subtitle_description_format=0x7f060002;
    public static final int abc_action_bar_up_description=0x7f060003;
}
````

We access a resource in two ways, in code using `R.string.hello` and in XML using `@string/hello`.

### Activities

An activity is an instance of the `Activity` class and is responsible for managing user interactions with a screen of information. Inside an activity are widgets: which can show text, graphics, interact with user, or arrange other widgets on screen. buttons, text input, check boxes.

###### Activity Lifecycle

Upon initially starting the application, our first activity executes the `onCreate()` hook, followed by `onStart()`, and then `onResume()` to gives us our running activity. If we have another activity created, our application executes the `onPause()` method for our current activity, but will resume with `onResume()` once this activey returns to the foreground. The `onStop()` hook occurs when we hit home during the use of our application, if we return, our applicaiton will start up where we initially left. However, the OS can determine that it needs additional resources and can kill these processes on a need be basis. And these activities will execute `onDestroy()` when we are finished with the current activity. Home button will cause the activity to execute `onPause()` and `onStop()`. Using the back button will also call `onDestroy()` which will also destroy any stashed state.

![DNS query diagram](/assets/images/activity_lifecycle.jpg)

Rotating the screen to change its orientation will cause it to change the device configuration, which calls `onDestroy()` and `onCreate()` in order to inflate the new activity with the device configuration.

###### Saving State

To persist our data upon changing the device configuration, we will use `protected void onSaveInstanceState(Bundle outState)` method. This method is called by the system before `onPause()`, `onStop()`, and `onDestroy`. And requires the data passed in as as a Bundle object.

````java
private static final String KEY_INDEX = "index";
@Override
public void onSaveInstanceState(Bundle savedInstanceState) {
  super.onSaveInstanceState(savedInstanceState);
  savedInstanceState.putInt(KEY_INDEX, mCurrentIndex);
}
````
###### Starting an activity

We can start an activity by using the `startActivity(intent)` method. This sends a call to a part of the OS called the `ActivityManager`, which creates `Activity` instances and calls `onCreate()`. An intent is an object that a component can use to communicate with the OS. Components are activities, services, broadcast recievers, and content providers. 

<table style="width:100%">
  <tr>
    <th>Constructor</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Intent()</td>
    <td>Create an empty intent.</td>
  </tr>
  <tr>
    <td>Intent(Intent o)</td>
    <td>Copy constructor.</td>
  </tr>
  <tr>
    <td>Intent(String action)</td>
    <td>Create an intent with a given action.</td>
  </tr>
  <tr>
    <td>Intent(String action, Uri uri)</td>
    <td>Create an intent with a given action and for a given data url.</td>
  </tr>
  <tr>
    <td>Intent(Context packageContext, Class<?> cls)</td>
    <td>Create an intent for a specific component.</td>
  </tr>
  <tr>
    <td>Intent(String action, Uri uri, Context packageContext, Class<?> cls)</td>
    <td>Create an intent for a specific component with a specified action and data.</td>
  </tr>
</table>

The `Intent` class specifies which activity the `ActivityManager` class should start. Our below example uses the `Intent(Context packageContext, Class cls)` contructor. `Context` tells which package the class object can be found in, and `Class` is what we want to start. We can add additional information to the intent by calling `public Intent putExtra(String name, boolean value)`, this allows us to pass information between our current activity and the one will will be creating.

````java
// MainActivity.java
Intent i = new Intent(MainActivity.this, SideActivity.class);
i.putExtra(SideActivity.MY_EXTRA_VALUE, myValue);
startActivity(i);
// SideActivity.java
public static final String MY_EXTRA_VALUE = "net.ShawnKlein.testapp";
mMyValue = getIntent().getBooleanExtra(MY_EXTRA_VALUE, false);
````

With the above code, the `ActivityManager` will check the package's manifest for a declaration with the same name as the specified class. If no declaration is found, it will respond with a `ActivityNotFoundException`. We call `Activity.getIntent()` to return the intent that started this activity and `getBooleanExtra(String name, boolean defaultValue)` to pull the information out of the intent. It is also important to note that we should define keys for extras on the activities that retrieve and use them, we do this with `public static final String MY_EXTRA_VALUE = "net.ShawnKlein.testapp";`.

For our child activity to return data back to our main activity, we will use `startActivityForResult(Intent intent, int requestCode)` instead of `startActivity(i)` and `onActivityResult(int requestCode, int resultCode, Intent data)` to actually retrive the returning data. The Child activity will use `setResult(int resultCode)` or `setResult(int resultCode, Intent data)`, if no result code is set, and the parent expects a return, the OS will send `RESULT_CANCELED` as the `resultCode`. 

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

To retrieve our returing data, we use `protected void onActivityResult()`. This is part of the `Activity` class, and more infomation can be found in the classes section of this guide.

### Fragments

A fragment is a controller object that an activity can deputize to perform tasks, such as managing a user interface. A UI fragment as in either the entire screen or only part of it. Using UI fragments has the benefit of a more modular interface. One where you can remove a fragment and easily replace it with another for a different function or view. Fragments are not able to get on the view by themselves, their host must define a spot in it's layout for the fragment, and manage the lifecycle of the fragment instance.

To use a fragment in an activity, we must define a space for our fragment to be used in our 'xml' file. While our example is for a single fragment, and activity cn define multiple container views, as well as widgets of it's own. Our fragment would be defined as normal.

````xml
// activity_main.xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:id="@+id/fragmentContainer"
android:layout_width="match_parent"
android:layout_height="match_parent"
/>
````

To set our controller to use a fragment, we replace `Activity` with `FragmentActivity`. Then we make a call to set up the `FragmentManager` using `getSuportFragmentManager()`. The `FragmentManager` handles a list of fragments and a back stack of fragment transactions. We ask FragmentManager for fragment, if in list, FragmentManager will return it. If `null`, a new CrimeFragment will be created.

We use a fragment transaction with `FragmentManager.beginTransaction()` returns an instance of FragmentTransaction, which we use to add, remove, attach, detach, or replace fragments in the fragment list. Our `add()` method takes 2 parameters, the container view ID and our TestFragment. The container view ID tells the FragmentManager where the fragment view should appear and is its unique identifier in the FragmentManager's list.

````java
// TestFragment.java
public class TestFragment extends Fragment {
  @Override
  public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
  }
  @Override
  public View onCreateView(LayoutInflater inflater, ViewGroup parent, Bundle savedInstanceState) {
    View v inflater.inflate(R.layout.fragment_test, parent, false);
    return v;
  }
}
// TestActivity.java
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

Our fragment class does not inflate using the `onCreate()` such as activity would, it inflates using the `onCreateView()` method. and we explicitly inflate the the fragments view by calling `LayoutInflater.inflate()`. The first parameter is the resource ID, second is the parent's view, and third tells the inflater whether to add the inflated view to the parent.


PAUSE on PAGE 145



TextWatcher
FragmentManager



Now that we have created our fragment, we need 2 things to host a UI fragment, a spot in the layout for the fragment's view, and manage the lifecycle of the fragment. Fragments lifecycle are called by the hosting activity, instead of the OS. You can add a fragment to either the hosting activity's _layout_ or _code_.

###### Layout

A layout fragment is simple, but inflexable. We hard code the fragment and its view to the activity's view and cannot swap out the fragment during the activity's lifetime.



###### Code

A code fragment gives us more control of how our fragment will interact with our activity. It will allow us to determine when to add the fragment to the host activity, remove it, replace it with another, and add the initial fragment back again.

Fragments also have some convenience methods such as `getActivity()` which returns the hosting activity


### Intent

An intent is an abstract description of an operation to be performed. It can be used with startActivity to launch an Activity, broadcastIntent to send it to any interested BroadcastReceiver components, and startService(Intent) or bindService(Intent, ServiceConnection, int) to communicate with a background Service.

An Intent provides a facility for performing late runtime binding between the code in different applications. Its most significant use is in the launching of activities, where it can be thought of as the glue between activities. It is basically a passive data structure holding an abstract description of an action to be performed.

<table style="width:100%">
  <tr>
    <th>Returns</th>
    <th>Method</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>Object</td>
    <td>clone()</td> 
    <td>Creates and returns a copy of this object.</td>
  </tr>
  <tr>
    <td>Intent</td>
    <td>putExtra(String name, Serializable value)</td> 
    <td>Add extended data to the intent. Takes other 2nd arguments, UUID is a Serializable object</td>
  </tr>
</table>

https://developer.android.com/reference/android/content/Intent.html

### UUID

A class that represents an immutable universally unique identifier (UUID). A UUID represents a 128-bit value.

There exist different variants of these global identifiers. The methods of this class are for manipulating the Leach-Salz variant, although the constructors allow the creation of any variant of UUID (described below).

The layout of a variant 2 (Leach-Salz) UUID is as follows: The most significant long consists of the following unsigned fields:

https://developer.android.com/reference/java/util/UUID.html




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
  <tr>
    <td>ListView</td>
    <td></td> 
    <td>Displays stuff.</td>
  </tr>
</table>

###### Activity

Table of some of the more popular methods from the `Activity` class.

<table style="width:100%">
  <tr>
    <th>Returns</th>
    <th>Method</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>Intent</td>
    <td>getIntent()</td> 
    <td>Return the intent that started this activity.</td>
  </tr>
  <tr>
    <td>void</td>
    <td>setTitle(int titleId)</td> 
    <td>Change the title associated with this activity.</td>
  </tr>
  <tr>
    <td>View</td>
    <td>findViewById(int id)</td> 
    <td>Finds a view that was identifiesd by the id attribute from the XML that was processed in onCreate(Bundle).</td>
  </tr>
  <tr>
    <td>Void</td>
    <td>setContentview(View/int view.layoutResId)</td> 
    <td>Set the activity content to an explicit view or from a layout resource.</td>
  </tr>
  <tr>
    <td>void</td>
    <td>onActivityResult(int requestCode, int resultCode, Intent data)</td> 
    <td>Called when an activity you launched exits, giving you the requestCode you started it with, the resultCode it returned, and any additional data from it.</td>
  </tr>
</table>

Methods that are coming: setResult(), startActivityForResult(), startActivity(), onActivityResult(), 

Also a section regarding RESULT_CANCELED

###### Intent

Table of some of the more popular methods from the `Fragment` class.

<table style="width:100%">
  <tr>
    <th>Returns</th>
    <th>Method</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>boolean</td>
    <td>getBooleanExtra(String name, boolean defaultValue)</td> 
    <td>Retrieve extended data from the intent.</td>
  </tr>
  <tr>
    <td>Intent</td>
    <td>putExtra(String name, boolean value)</td> 
    <td>Add extended data to the intent. Can also pass in all primitives, Serializable, Bundle, arrays, and String.</td>
  </tr>
</table>

###### Fragment

Table of some of the more popular methods from the `Fragment` class.

<table style="width:100%">
  <tr>
    <th>Returns</th>
    <th>Method</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>final Activity</td>
    <td>getActivity()</td> 
    <td>Return the Activity this fragment is currently associated with.</td>
  </tr>
  <tr>
    <td>View</td>
    <td>onCreateView(LayoutInflater, ViewGrou, Bundle)</td> 
    <td>Called to have the fragment instantiate its user interface view.</td>
  </tr>
</table>

###### FragmentManager

Table of some of the more popular methods from the abstract `FragmentManager` class.

<table style="width:100%">
  <tr>
    <th>Returns</th>
    <th>Method</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>abstract Fragment</td>
    <td>findFragmentById(int id)</td> 
    <td>Finds a fragment that was identified by the given id.</td>
  </tr>
  <tr>
    <td>abstract FragmentTransactiopn</td>
    <td>beginTransaction()</td> 
    <td>Start a series of edit operations on the Fragments associated with this FragmentManager.</td>
  </tr>
</table>

###### ListFragment

Table of some of the more popular methods from the abstract `FragmentManager` class.

<table style="width:100%">
  <tr>
    <th>Returns</th>
    <th>Method</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>void</td>
    <td>setListAdapter(ListAdapter adapter)</td> 
    <td>Provide the cursor for the list view.</td>
  </tr>
  <tr>
    <td>ListView</td>
    <td>getListView</td> 
    <td>Get the activity's list view widget.</td>
  </tr>
  <tr>
    <td></td>
    <td>onListItemClick()</td> 
    <td></td>
  </tr>
  <tr>
    <td>ListAdapter</td>
    <td>getListAdapter()</td> 
    <td>Get the ListAdapter associated with this fragment's ListView.</td>
  </tr>
</table>

###### Adapter

Table of some of the more popular methods from the public `Adapter` interface.

<table style="width:100%">
  <tr>
    <th>Returns</th>
    <th>Method</th> 
    <th>Description</th>
  </tr>
  <tr>
    <td>abstract object</td>
    <td>getItem(int position)</td> 
    <td>Get the data item associated with the specified position in the data set.</td>
  </tr>
  <tr>
    <td>abstract object</td>
    <td>getItem(int position)</td> 
    <td>Get the data item associated with the specified position in the data set.</td>
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

### Adaptor

An Adapter object acts as a bridge between an AdapterView and the underlying data for that view. The Adapter provides access to the data items. The Adapter is also responsible for making a View for each item in the data set.

Known subclasses are:

ArrayAdapter<T>, BaseAdapter, CursorAdapter, HeaderViewListAdapter, ListAdapter, ResourceCursorAdapter, SimpleAdapter, SimpleCursorAdapter, SpinnerAdapter, ThemedSpinnerAdapter, WrapperListAdapter

Adaptor is responisble for:

* creating the necessary view object
* populating it with the data from the model layer
* returning the view object to the ListView


### R.layout

Are built in XML layout documents, a small sample set is as follows:

activity_list_item, browser_link_context_header, list_content, select_dialog_item, simple_dropdown_item1line, simple_expandable_list_item_1, simple_list_item_1, simple_list_activated_2, simple_list_multiple_choice, simple_spinner_item, two_line_list_item


extras


`@Override` annotation ensures the class actually has the method you are attempting to override. The compiler will notify you if it does not possess this class.

A `@TargetApi(11)` annotation is used to suppress build errors from Lint. It is used to say that the below method will only be referenced by an OS version that is passed in as an argument. In our case, version `11`, Honeycomb, instead of the version what is declared in our manifest.

193




 
