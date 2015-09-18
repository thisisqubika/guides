# Android Style Guide

## Table of Contents

* [Naming](#naming)

## Naming

### Java

* Non-public, non-static field names start with m.
* Static field names start with s.
* Other fields start with a lower case letter.
* Public static final fields (constants) are ALL_CAPS_WITH_UNDERSCORES.
* Long, descriptive method and variable names are good.

**For example:**

```
public class MyClass {
    public static final int SOME_CONSTANT = 42;
    public int publicField;
    private static MyClass sSingleton;
    int mPackagePrivate;
    private int mPrivate;
    protected int mProtected;
}
```

### Activities and Fragments

* `Activity Java class:` The name of the class needs to be a description of the activity followed by 'Activity'.
* `Activity layout file:` The word *activity* followed by underscore and the description of the activity.
* The same criteria is used for Fragments.
* `General layouts:` The word *layout* followed by underscore and the description of the layout.
* `Drawables:` Always define with the prefix *dw*.

**For example:**

```
LoginActivity.java

activity_login.xml

layout_list_item_asset.xml

dw_rounded_button_blue.xml
```
