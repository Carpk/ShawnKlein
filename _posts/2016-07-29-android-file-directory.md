---
layout: post
cover: 'assets/images/railroad_fog.jpg'
title: Android File Directory
date:   2016-03-29 10:18:00
tags: coding java
subclass: 'post tag-test tag-content'
categories: 'casper'
navigation: True
logo: 'assets/images/logo.png'
---

This post will take a look at the Android file structure.

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

We access a resource in two ways, in code using `R.string.hello` and in XML using `@string/hello`




### Resources

Resources are images, audio, and XML files that live in the res/ diretory. We use the resource ID `R.layout.activity_main` to access these files. This ID returns an int set from `class layout` in R.java, this looks something like  `public static final int activity_main=0x7f04001a;`. 

The method `setContentView(R.layout.activity_main)` is how our `MainActivity.java` file knows which layout to inflate. In `android:id="@+id/false_button"` the `+` is used during the creation of the id.

Android keeps all the strings all in one place, the strings.xml file.

````java
// strings.xml
<resources>
  <string name="hello_toast">Hello!</string>
</resources>
````

The `public View findViewById(int id)` takes ID of widget and returns a View object. We will prefix with "Button" to return a button instead of a view `mTrueButton = (Button)findViewById(R.id.true_button)`

##### Drawables

References to a string will begin with `@string/` and refrences for a drawable will begin with `@drawable/`. Drawable resorces have differant files for different dpi screens.

* __mdpi__ medium density screens(~160dpi)
* __hdpi__ high density screens(~240dpi)
* __xhdpi__ extra high density screens(~320dpi)
* __xxhdpi__ extra extra high density screens(~480dpi)
* __xxxhdpi__ extra extra extra high density screens(~640dpi)












