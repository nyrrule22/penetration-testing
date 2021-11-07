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

### Vulnerabilities

* Weak or improper cryptography use
* Exported Preference Activities
* Apps which enable backups
* Apps which are debuggable
* App Permissions.
* Firebase Instance(s)
* Sensitive data in the code

#### Weak or improper cryptography use

Incorrect uses of encryption algorithm may result in sensitive data exposure, key leakage, broken authentication, insecure session and spoofing attack.

Example: For Java implementation, the following API is related to encryption. Review the parameters of the encryption implementation.

* `IvParameterSpec iv = new IvParameterSpec(initVector.getBytes("UTF-8"));`
* `SecretKeySpec skeySpec = new SecretKeySpec(key.getBytes("UTF-8"), "AES");`

How to search this when I have the source code of the application?

* `grep -r "SecretKeySpec" *`
* `grep -rli "aes" *`
* `grep -rli "iv"`

Solution: Use a cryptography asymmetric.

#### Exported Preference Activities

As we know, Android's activity component is application screen(s) and the action(s) that applied on that screen(s) when we use the application. When as activity is found to be shared with other apps on the device therefore leaving it accessible to any other application on the device.

Okay, exploit this in dynamic analysis... How identify the activity is exported?

With your favorite editor of text. Gedit/Vim/subl, etc… open the AndroidManifest.xml or use cat and grep.

`cat AndroidManifest.xml | grep "activity" --color`

#### Apps which enable backups

This is considered a security issue because people could backup your app via ADB and then get private data of your app into their PC.

1. Shared preference.
2. directory returned by getFilesDir().
3. getDataBase(path) also includes files created by SQLiteOpenHelper.
4. files in directories created with getDir(Sring, int).
5. files on external storage returned by getExternalFilesDir (String type).

How to identify this?&#x20;

Open the AndroidManifest.xml or use cat & grep: android:allowBackup="true"

`cat AndroidManifest.xml | grep "android:allowBackup" --color`

Solution: android:allowBackup="false"

#### Apps which are debuggable

Debugging was enabled on the app which makes it easier for reverse engineers to hook a debugger to it. This allows dumping a stack trace and accessing debugging helper classes.

How to identify this? Open the AndroidManifest.xml or use cat & grep: android:debuggable="true"

`cat AndroidManifest.xml | grep "android:debuggable" --color`

...

#### Firebase Instance(s)

Last year, security researchers have discovered unprotected Firebase databases of thousands of iOS and Android mobile applications that are exposing over 100 million data records, including plain text passwords, user IDs, location, and in some cases, financial financial records such as banking and cryptocurrency transactions.

Google's Firebase service is one of the most popular back-end development platforms for mobile and web applications that offers developers a cloud-based database, which stores data in JSON format and synced it in the real-time with all connected clients.

How identify this?

[FireBase Scanner](https://github.com/shivsahni/FireBaseScanner), The script helps security analysts to identify misconfigured firebase instances.

`git clone https://github.com/shivsahni/FireBaseScanner`

`python FireBaseScanner.py -p /path/apk`

#### Sensitive data in the code

Users, passwords, internal IP and more...

With your favorite editor of text, Gedit/Vim/subl, etc…, grep or GUI decompiler back to reversing and experiment with your favorite tool.

#### App Permissions

System permissions are divided into two groups: “normal” and “dangerous.” Normal permission groups are allowed by default, because they don’t pose a risk to your privacy. (e.g., Android allows apps to access the Internet without your permission.) Dangerous permission groups, however, can give apps access to things like your calling history, private messages, location, camera, microphone, and more. Therefore, Android will always ask you to approve dangerous permissions.

### Frameworks

#### MARA Framework: [https://github.com/xtiankisutsa/MARA\_Framework](https://github.com/xtiankisutsa/MARA\_Framework)

Is a Mobile Application Reverse engineering and Analysis Framework. It is a tool that puts together commonly used mobile application reverse engineering and analysis tools, to assist in testing mobile applications against the OWASP mobile security threats.

#### QARK: [https://github.com/linkedin/qark](https://github.com/linkedin/qark)

Is a static code analysis tool, designed to recognize potential security vulnerabilities and points of concern for Java-based Android applications.

#### MobSF: [https://mobsf.github.io/docs/#/](https://mobsf.github.io/docs/#/)

Is an automated, all-in-one mobile application (Android/iOS/Windows) pen-testing, malware analysis and security assessment framework capable of performing static and dynamic analysis.

### Complications

#### Obfuscate Code

Is the process of modifying an executable so that it is no longer useful to a hacker but remains fully functional. While the process may modify actual method instructions or metadata, it does not alter the output of the program. To be clear, with enough time and effort, almost all code can be reverse engineered. However, on some platforms (such as Java, Android, iOS and .NET) free decompilers can easily reverse-engineer source code from an executable or library in virtually no time and with no effort. Automated code obfuscation makes reverse-engineering a program difficult and economically unfeasible.

#### Proguard

To obfuscate the code, use the Proguard utility, which makes these actions:

* Removes unused variables, classes, methods, and attributes
* Eliminates unnecessary instructions
* Removes Tutorial information: obfuscate Androiddepuración code;
* Renames classes, fields, and methods with illegible names

#### DEXGUARD

The enhanced commercial version of Proguard. This tool is capable of implementing the text encryption technique and renaming classes and methods with non-ASCII symbols.

#### Deguard

It is based on powerful probabilistic graphical models learned from thousands of open source programs. Using these models, Deguard retrieves important information in Android APK, including method and class names, as well as third-party libraries. Deguard can reveal string decoders and classes that handle sensitive data in Android malware.

## Dynamic Analysis

## Report

