+++ 
date = 2019-03-19T15:10:00+00:00
title = "Xamarin Android and Android Studio"
aliases = ['/2019/03/19/xamarin-android-and-android-studio', '/2019/03/19/xamarin-android-and-android-studio.html']
+++

# The Problem

If you ever worked with Android Studio, you know that Visual Studio or Visual Studio for Mac are not good IDE's to edit Android XML resource files.


![Visual Studio for Mac - Sad Autocomplete](/img/2019-03-19-xamarin-android-and-android-studio/vsbad.png)

![Android Studio - A superior Autocomplete](/img/2019-03-19-xamarin-android-and-android-studio/asgood.png)

# The Solution

What we want is to use **Visual Studio for coding** and **Android Studio to edit the resources**.

To do this, let's jump into Android Studio and create a new empty android app. This project is going to be our **resources host**.

**But we don't want to keep moving resources from IDE to IDE**. There is option on gradle to change our resources folder path. What we want is to point to our resources folder on our Xamarin Android project.

Add the sourceSet on `build.gradle (Module: app)`
 
```groovy
android {
   [...]
    sourceSets {
        main {
            res.srcDirs = ['/path_to_you_xamarin_android_project/Resources']
        }
    }
}
```

and enjoy changing the xml files inside Android Studio. That was easy :)

# The Catch

If you add new xml files on Android Studio they will **not appear in Visual Studio**, but they are created on the correct folders. You need to **manually add the new files in Visual Studio**. I usually create the files in Visual Studio and then go to Android Studio to have all the goodness of autocomplete, preview, etc.