# Methodology

## Method

1. Information Gathering
2. Reversing
3. Static Analysis
4. Dynamic Analysis
5. Report

## Information Gathering

How do I find the application of the organization?

Easy, play store: is a digital distribution platform for mobile apps for devices with Android operating system.

`https://play.google.com/store/apps/details?id=com.shortstack.hackertracker`

> Where `id=` specifies the name of the package for a select app

## Reversing

####

### View Devices

#### ADB

Android Debug Bridge (ADB) is a development tool that facilitates communication between an Android device and a personal computer.

* `adb devices`

### Extract APK

* You will need to have installed the app on your device and know package name
* `adb shell pm path package_name`
* `adb shell pm path com.shortstack.hackertracker`
* This will print the path to the APK of the given
* `package /data/app/com/shortstack.hackertracker-1/base.apk`
* The below command pulls the file remote to local. If local isn't specified, it will pull to the current folder.
* `adb pull <remote> [<localDestination>]`
* `adb pull /data/app/com.shortstack.hackertracker-1/base.apk`

### Get the Source Code

#### jadx

The jadx suite allows you to simply load an APK and look at its Java source code. What’s happening under the hood is that jADX is decompiling the APK to smali and then converting the smali back to Java.

* `jadx -d [path-output-folder] [path-apk-or-dex-file]`

#### Dex2Jar

Use dex2jar to convert an APK to a JAR file.

* `d2j-dex2jar.sh or .bat /path/application.apk`
* Once you have the JAR file, simply open it with JD-GUI and you’ll see its Java code.

#### apktool

This tool get a source code in smali.

* `apktool d file.apk`

#### jadx-gui

UI version of jadx

* `jadx\bin\jadx-gui`

## Static Analysis

## Dynamic Analysis

## Report

