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


Gradle build scripts are a DSL, they come with a top-level build file and build files for each module.

#### activity
An activity is an instance of `Activity` and is responsible for managing user interactions with a screen of information.

widgets: can show text, graphics, interact with user, or arrange other widgets on screen. buttons, text input, check boxes.

linearLayout < ViewGroup < View, 
LinearLayout inherits from ViewGroup, which is a widget that contains other widgets.
ViewGroups are FrameLayout, TableLayout, RelativeLayout

android:layout_width and layout_height
match_parent view will be as big as parent
wrap_content view with be as big as contents require

android:orientation
On LinearLayout, determines if children will appear vertically or horizontally.

android:text
string resources are places in a separate strings file,
"@string/referenced_text"

### resources

resources are images, audio, and XML files that live in the res/ diretory. we use the resource ID 'R.layout.activity_main' to access these files. This ID returns an int set from `class layout` in R.java, this looks something like  `public static final int activity_main=0x7f04001a;`. 

setContentView(R.layout.activity_main) is how our MainActivity.java file knows which layout to inflate. android:id="@+id/false_button" + is used during the creation of the id.

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















