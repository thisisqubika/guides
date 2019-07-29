# Android Style Guide

## Table of Contents

* [Kotlin](#kotlin)
* [Java](#java)

### Kotlin

## Naming

* All source files must be encoded as UTF-8.
* If a source file contains only a single top-level class, the file name should reflect the case-sensitive name plus
* If a source file contains multiple top-level declarations, choose a name that describes the contents of the file, apply PascalCase
* Special prefixes or suffixes, like those seen in the examples name_, mName, s_name, and kName, are not used except in the case of backing properties

### Package names

* Package names are all lowercase, with consecutive words simply concatenated together (no underscores).
```
// Okay
package com.example.deepspace
// WRONG!
package com.example.deepSpace
// WRONG!
package com.example.deep_space
```

### Type names

* Class names are written in PascalCase and are typically nouns or noun phrases. For example, `Character` or `ImmutableList`
* Interface names may also be nouns or noun phrases (for example, `List`), but may sometimes be adjectives or adjective phrases instead (for example `Readable`).
* Test classes are named starting with the name of the class they are testing, and ending with Test. For example, HashTest or HashIntegrationTest.

### Function names

* Function names are written in camelCase and are typically verbs or verb phrases. For example, sendMessage or stop.
* Underscores are permitted to appear in test function names to separate logical components of the name.

### Constants names

* Constant names use UPPER_SNAKE_CASE: all uppercase letters, with words separated by underscores.
* Constant values can only be defined inside of an object or as a top-level declaration. Values otherwise meeting the requirement of a constant but defined inside of a class must use a non-constant name.
* Constants which are scalar values must use the `const` modifier.
**For example:**
```
const val NUMBER = 5
val NAMES = listOf("Alice", "Bob")
val AGES = mapOf("Alice" to 35, "Bob" to 32)
val COMMA_JOINER = Joiner.on(',') // Joiner is immutable
val EMPTY_ARRAY = arrayOf<SomeMutableType>()
```

## Non-constants names

* Non-constant names are written in camelCase. These apply to instance properties, local properties, and parameter names. Typically nouns or noun phrases.
```
val variable = "var"
val nonConstScalar = "non-const"
val mutableCollection: MutableSet<String> = HashSet()
val mutableElements = listOf(mutableInstance)
val mutableValues = mapOf("Alice" to mutableInstance, "Bob" to mutableInstance2)
val logger = Logger.getLogger(MyClass::class.java.name)
val nonEmptyArray = arrayOf("these", "can", "change")
```

## Fomratting

### Braces

* Braces are not required for `when` branches and `if` statement bodies which have no else `if/else` branches and which fit on a single line.
**For example:**
```
if (string.isEmpty()) return

when (value) {
    0 -> return
    // …
}
```

* Braces are otherwise required for any `if`, `for`, `when` branch, `do`, and `while` statements, even when the body is empty or contains only a single statement.
**For example:**
```
if (string.isEmpty())
    return  // WRONG!

if (string.isEmpty()) {
    return  // Okay
}
```

* No line break before the opening brace
* Line break after the opening brace.
* Line break before the closing brace.
* Line break after the closing brace, only if that brace terminates a statement or terminates the body of a function, constructor, or named class.
* Avoid empty blocks, but if you need a empty block or block-like construct, it must be in K&R style.
* An if/else conditional that is used as an expression may omit braces only if the entire expression fits on one line.


### Identation

* Each time a new block or block-like construct is opened, the indent increases by four spaces. 
* When the block ends, the indent returns to the previous indent level. 
* The indent level applies to both code and comments throughout the block.

### Line wrapping

* Code has a column limit of 100 characters. Any line that would exceed this limit must be line-wrapped
* Can break line when a line is broken at a non-assignment operator the break comes before the symbol.
* Can break line when a line is broken at an assignment operator the break comes after the symbol.
* Method or constructor name stays attached to the open parenthesis (() that follows it.
* Comma (,) stays attached to the token that precedes it.
* Lambda arrow (->) stays attached to the argument list that precedes it.
* When a function signature does not fit on a single line, break each parameter declaration onto its own line. 
* Parameters defined in this format should use a single indent (+4). 
* The closing parenthesis ()) and return type are placed on their own line with no additional indent.
**For example:**
```
fun <T> Iterable<T>.joinToString(
    separator: CharSequence = ", ",
    prefix: CharSequence = "",
    postfix: CharSequence = ""
): String {
    // …
}
```

### Whitespaces

* Between consecutive members of a class: properties, constructors, functions, nested classes, etc
* Between statements, as needed to organize the code into logical subsections.
* Separating any reserved word, such as if, for, or catch from an open parenthesis (() that follows it on that line.
* Separating any reserved word, such as else or catch, from a closing curly brace (}) that precedes it on that line.
* Before any open curly brace ({).
* On both sides of any binary operator.
* Before a colon (:) only if used in a class declaration for specifying a base class / interfaces or when used in a where clause for generic constraints.
* After a comma (,) or colon (:).

### Enum Classes

* An enum with no functions and no documentation on its constants may optionally be formatted as a single line.
```
enum class Answer { YES, NO, MAYBE }
```
* When the constants in an enum are placed on separate lines, a blank line is not required between them except in the case where they define a body.
```
enum class Answer {
    YES,
    NO,

    MAYBE {
        override fun toString() = """¯\_(ツ)_/¯"""
    }
}
```

### Annotations

* Member or type annotations are placed on separate lines immediately prior to the annotated construct.
```
@Retention(SOURCE)
@Target(FUNCTION, PROPERTY_SETTER, FIELD)
annotation class Global
```

* Annotations without arguments can be placed on a single line.
* When only a single annotation without arguments is present it may be placed on the same line as the declaration.
```
@Volatile var disposable: Disposable? = null

@Test fun selectAll() {
    // …
}
```

### Implicit return/property types

* If an expression function body or a property initializer is a scalar value or the return type can be clearly inferred from the body then it can be omitted.
```
override fun toString(): String = "Hey"
// becomes
override fun toString() = "Hey"
```


### Java

## Naming

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
