---
layout:     post
summary:    High level overview of Gradle and how it fits into the entire Android build system.
title:      Beginner's Guide to Gradle for Android Developers
---
The goal of this article is to give you a high level overview of Gradle and how it fits into the entire build system when developing Android applications. I'll go through the basics of Gradle and the Android Plugin for Gradle. I'll then go through the default `build.gradle` scripts that come with a new Android Project.

This article will not teach you how to write custom Gradle scripts or anything of that sort. At the title says, it's really meant for beginners.

## Backstory
I'll start off with a confession: I'm mainly an iOS developer. Throughout my career, only a quarter of it has been spent working on Android applications. Because of this, I never really understood what Gradle was. I knew that it was "the thing" that went to work whenever I clicked on the green play button in Android Studio, but I had no idea what it actually did.

This lack of knowledge made me very impatient whenever I saw the words "Gradle Build Running" for more than 10 seconds. _"What's taking so long?"_ I'd ask Android Studio, hoping for some sort of a sign that it wasn't just stuck. At work, whenever I saw an Android dev blankly staring at the screen, I'd often jokingly ask them _"are you waiting for Gradle to build?"_

Eventually I got frustrated and decided to figure out if I could cut down the build time. I watched a talk from Google I/O '17 titled ["Speeding Up Your Android Gradle Builds"](https://www.youtube.com/watch?v=7ll-rkLCtyk) confident that I'd unlock the key to all the Gradle speed I ever wanted.

![no-idea-dog](/images/no-idea-dog.jpeg){: .centre-image}

What I got instead after 40 minutes was the realisation that I knew nothing about Gradle, hence I would have no chance at all at making it any better. I put my foot down and decided that it was time for me to understand Gradle.

---

## The Basics
To kick things off, let's clear up some things:
1. Android Studio has no idea how to compile your Java & Kotlin code into an APK file.
2. Gradle has no idea how to compile your Java & Kotlin code into an APK file either.

Yes, you read that correctly.
Gradle itself has no idea how to compile an APK file because Gradle is actually a **general purpose build tool**. It is not limited to building Android apps. On [Gradle's GitHub repository](https://github.com/gradle/gradle), it's described as:

> …a build tool with a focus on build automation and support for multi-language development. If you are building, testing, publishing, and deploying software on any platform, Gradle offers a flexible model that can support the entire development lifecycle from compiling and packaging code to publishing web sites.

On its own, Gradle itself isn't actually able to do much. All of its useful features come from its rich **plugin** ecosystem. Think of all those third party libraries that you add into your Android app as plugins. You use those plugins to extend the functionality of your application, just like how Gradle uses plugins to extend its own functionality.

There are many plugins that come [bundled with Gradle](https://docs.gradle.org/current/userguide/standard_plugins.html) as well as many more that [you can download](https://plugins.gradle.org). However if you go through the list of plugins that come with Gradle, you'll realise that "Android" is nowhere to be found on that page.

## Android Plugin for Gradle
The [Android Plugin for Gradle](https://developer.android.com/studio/releases/gradle-plugin) is the plugin that enables Gradle to be able to compile your code into an APK file, sign your APK with your keys and even install the APK onto your emulator or test devices. This plugin is what drives your entire build system.

Without it there's just no way for Gradle to know how to do anything with your code. This is what I meant by both Android Studio and Gradle having no clue how to build your Android project: this plugin is the magic chain between Android Studio and Gradle.

## Diving into the Scripts
Now that we've got some of the basics down, let's look into how this actually translates to the real world. I'll go through the files that you get when you start a brand new project in Android Studio:

![gradle-1](/images/gradle-1.png){: .centre-image}
*"Open up your Gradle file" they said..*

> What are all these files for?

All the files with the word "gradle" in them are used to configure Gradle for our Android projects. There are multiple files because they all serve different purposes.

### Gradle Wrapper
The `gradle-wrapper.properties` file has one simple purpose: to determine which version of Gradle to use to build the project. It will then automatically download and store that version of Gradle for you. If you're on a Mac, run the following command: `ls ~/.gradle/wrapper/dists/` You'll be able to see all the versions of Gradle that the Gradle Wrapper has ever downloaded for you.

![gradle-2](/images/gradle-2.png){: .centre-image}
*gradle-wrapper.properties*

Take note that your Gradle version is separate from your Android plugin version. At the time of writing, the current [latest version of Gradle is v4.3](https://gradle.org/releases/). Android Studio still defaults to v4.1, so you can safely bump this up to v4.3 if you like.

### settings.gradle
The `settings.gradle` file is where you inform Gradle about all of the sub-projects/modules that your project has. This is done through [the `include` command](https://docs.gradle.org/current/dsl/org.gradle.api.initialization.Settings.html#org.gradle.api.initialization.Settings:include%28java.lang.String[]%29). If you were to add another module into your project, Android Studio will automatically add it into this file.

### build.gradle
From Gradle's point of view, our project is considered a [multi-project build](https://docs.gradle.org/current/userguide/multi_project_builds.html) where you have a root project and one or more sub-projects. From an Android development point of view, these sub-projects are called [modules](https://developer.android.com/studio/projects/#ApplicationModules).

This is why there are two `build.gradle` files that you see. One is for your root project, while the other is for the `app` module that comes with your project. Let's start off with the one for your root project.

![gradle-3](/images/gradle-3.png){: .centre-image}
*Root project's build.gradle*

1. This [entire `buildscript{}` block](https://docs.gradle.org/current/dsl/org.gradle.api.Project.html#org.gradle.api.Project:buildscript%28groovy.lang.Closure%29) is used to tell Gradle script itself what it needs in order to compile this project.
2. We declare Android Plugin for Gradle as one of the dependencies for this buildscript. "3.0.0" refers to the version of the plugin to be used.
3. We tell Gradle to look for the things we need inside the [`google()` Maven repository](https://dl.google.com/dl/android/maven2/index.html) and [`jcenter()` repository](https://jcenter.bintray.com).
4. Add an [extra property](https://docs.gradle.org/current/javadoc/org/gradle/api/Project.html#extraproperties) to the Gradle project, allowing it to be accessible throughout the Gradle project. In other words, this is a Gradle style global variable. We can see this variable value being used to determine which version of [the `kotlin-gradle-plugin` to be imported](https://bintray.com/bintray/jcenter/org.jetbrains.kotlin%3Akotlin-gradle-plugin).
5. As the name implies, [the `allprojects{}` block](https://docs.gradle.org/current/dsl/org.gradle.api.Project.html#org.gradle.api.Project:allprojects%28groovy.lang.Closure%29) is being used to inform Gradle that for all the sub-projects being compiled, use this set of repositories to resolve any required dependencies.

![gradle-4](/images/gradle-4.png){: .centre-image}
*app module's build.gradle*

1. First we apply the actual Android plugin, then we apply the Kotlin Android plugin followed by its [extensions plugin](https://kotlinlang.org/docs/tutorials/android-plugin.html).
2. The only reason this entire `android{}` block works is because we asked Gradle to apply the Android plugin mentioned previously. I'm sure you're familiar with modifying values inside the block, but have you ever wondered what are all the possible values that you can put inside? Good thing it's [all documented here](http://google.github.io/android-gradle-dsl/current/com.android.build.gradle.AppExtension.html)!
3. This is where you will add all of your third party libraries as Gradle dependencies. Take note that there's no `repositories{}` block inside your app's `build.gradle` file. It's not necessary since we already declared it using the `allprojects{}` block in the root project.
4. Remember the global variable we declared in the root's build file? Well here it is in action again. It's probably a good idea to adopt the same technique for [managing the version of your support libraries used](https://developer.android.com/studio/build/gradle-tips#configure-project-wide-properties) to ensure that they all use the same version.

## Gradle Tasks
Now that we've dived through the scripts, there's one more thing about Gradle that you need to know about: tasks.

Tasks are basically the things that Gradle can do whenever a build is triggered. Remember earlier I said that Android Studio has no idea how to compile your code? That's because clicking on the big green play button in Android Studio will trigger a specific task for Gradle to perform.

![gradle-5](/images/gradle-5.png){: .centre-image}

On the bottom right corner, click on the "Gradle Console" button to open up the Gradle console. Then click on play button to run the app. A bunch of commands will appear, but we only care about the one at the top:

*Executing tasks: [:app:assembleDebug]*

We just told Gradle to perform the `assembleDebug` task. We can do the exact same thing via the command line. Click on the Terminal tab at the bottom left and run this command: `./gradlew assembleDebug --console plain`

![gradle-6](/images/gradle-6.png){: .centre-image}

Voila! You just made Gradle run the exact same command that the play button did. A few things to note:
1. `./gradlew` means to use the Gradle Wrapper instead of "vanilla" Gradle instead. It is highly recommended to always use the Wrapper version.
2. `assembleDebug` is the name of the task you just asked it to run.
3. `--console plain` tells Gradle to print out the build log exactly as how you see it in Android Studio. Completely optional.

Let's run one last command: `./gradlew tasks`

This command will list out all the tasks that Gradle currently knows about in this project and it provides a short description about what each task does. Cool right?

Now go and click on the Gradle tab at the top right of Android Studio.

![gradle-7](/images/gradle-7.png){: .centre-image}

Tada! It's the same stuff. This segment just lists out all the possible tasks the Gradle can run for this project. Double clicking on `assembleDebug` here does the same exact thing you did just now on the command line, and does the same thing as the play button.

If you run the Rebuild Project command in Android Studio while keeping the Gradle Console open, you'll realise that all it is doing is to run the `clean` task followed by the `assembleDebug` command. This is how I found out that running Clean Project before Rebuild Project was completely unnecessary as Rebuild Project would run the same `clean` task anyway. The more you know!

---

## Closing Thoughts
I hope that this article has given you a better picture of how Gradle fits into your development workflow. This took way longer to write that you would think, but it was well worth it for me. I've since rewatched the ["Speeding Up Your Android Gradle Builds"](https://www.youtube.com/watch?v=7ll-rkLCtyk) video and I'm proud to say I'm no longer completely lost by the end of it.

---

_This is a mirror of the article that I [published on Medium](https://journals.apptivitylab.com/beginners-guide-to-gradle-for-android-developers-7972bfdf0668)._
