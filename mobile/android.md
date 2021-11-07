# Android

## Introduction

### Native vs Hybrid Applications

Native: They are those developed applications only and exclusively for mobile operating systems, either Android or iOS. In Android you use the Java or kotlin programming language, while in iOS you make use of Swift or Objective-C. These programming languages are the official ones for the respective operating systems.

Hybrid: These applications use technologies such as HTML, CSS and JavaScript, all of these linked and processed through frameworks such as Apache Córdova "PhoneGap", Ionic, among others.

### Android SMALI Code

When you create an application code, the apk file contains a .dex file, which contains binary Dalvik bytecode. Smali is an assembly language that runs on Dalvik VM, which is Android's JVM.

{% tabs %}
{% tab title="Java" %}
```java
public static int simpleMethod(boolean param) {
    int x = 5;
    if (param) {
        return x;
    }
    return 0;
}
```
{% endtab %}

{% tab title="Smali" %}
```smali
.method public static simpleMethod(X)I
    .locals 1
    const/16 v0, 0x5 #@0
    if-eqz p0, :cond_0 #@2
    :goto_0
    reutrn v0 #@4
    :cond_0
    const/4 v0, 0x0 #@5
    goto :goto_0 #@6
.end method
```
{% endtab %}
{% endtabs %}

