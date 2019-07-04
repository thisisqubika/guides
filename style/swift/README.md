# Swift Style Guide

A guide to our Swift style and conventions.

This is an attempt to encourage patterns that accomplish the following goals (in
rough priority order):

 1. Increased rigor, and decreased likelihood of programmer error
 1. Increased clarity of intent
 1. Reduced verbosity
 1. Fewer debates about aesthetics

If you have suggestions, please see our [contribution guidelines](/#contributing).

----

## Overall design decisions

Here, we'll specificy several design decisions that are not necess decided by a linter or a code style, but good practices to follow in the project.

#### Whitespace

 * Four spaces, not tabs.
 * End files with a newline.
 * Make liberal use of vertical whitespace to divide code into logical chunks.
 * Don’t leave trailing whitespace.
   * Not even leading indentation on blank lines.

#### Make a folder associated with each group

Whenever we create a new group we should create an associated folder for that group, all files that are in the same group must be on the same folder on the file system. This is enabled by default by the newest versions of Xcode.

*Note:* Supporting Files group will have a folder called SupportingFiles

#### Prefer `let`-bindings over `var`-bindings wherever possible

Use `let foo = …` over `var foo = …` wherever possible (and when in doubt). Only use `var` if you absolutely have to (i.e. you *know* that the value might change, e.g. when using the `weak` storage modifier).

_Rationale:_ The intent and meaning of both keywords is clear, but *let-by-default* results in safer and clearer code.

A `let`-binding guarantees and *clearly signals to the programmer* that its value is supposed to and will never change. Subsequent code can thus make stronger assumptions about its usage.

It becomes easier to reason about code. Had you used `var` while still making the assumption that the value never changed, you would have to manually check that.

Accordingly, whenever you see a `var` identifier being used, assume that it will change and ask yourself why.

#### Avoid Using Force-Unwrapping of Optionals

If you have an identifier `foo` of type `FooType?` or `FooType!`, don't force-unwrap it to get to the underlying value (`foo!`) if possible.

Instead, prefer this:

```swift
if let foo = foo {
    // Use unwrapped `foo` value in here
} else {
    // If appropriate, handle the case where the optional is nil
}
```

Alternatively, you might want to use Swift's Optional Chaining in some of these cases, such as:

```swift
// Call the function if `foo` is not nil. If `foo` is nil, ignore we ever tried to make the call
foo?.callSomethingIfFooIsNotNil()
```

_Rationale:_ Explicit `if let`-binding of optionals results in safer code. Force unwrapping is more prone to lead to runtime crashes.

#### Always prefer guard over if

Always prefer to use `guard`s instead of nested `if let` with an else clause.

Prefer:

```swift
guard a else {
    // else a code
    return
}

guard b else {
    // else b code
    return
}

// main code
```

to:

```swift
if a {
    if b {
        //  main code
    } else {
        // else b code
    }
} else {
    // else a code
}
```

_Rationale:_ Guards avoid nesting conditions, which makes the code clearer. This swift feature lets us define conditions that must be meet so the main code of the function is executed.

#### Avoid Using Implicitly Unwrapped Optionals

Where possible, use `let foo: FooType?` instead of `let foo: FooType!` if `foo` may be nil (Note that in general, `?` can be used instead of `!`).

_Rationale:_ Explicit optionals result in safer code. Implicitly unwrapped optionals have the potential of crashing at runtime.

#### Prefer implicit getters on read-only properties and subscripts

When possible, omit the `get` keyword on read-only computed properties and
read-only subscripts.

So, write these:

```swift
var myGreatProperty: Int {
	return 4
}

subscript(index: Int) -> T {
    return objects[index]
}
```

… not these:

```swift
var myGreatProperty: Int {
	get {
		return 4
	}
}

subscript(index: Int) -> T {
    get {
        return objects[index]
    }
}
```

_Rationale:_ The intent and meaning of the first version is clear, and results in less code.

#### Always specify access control explicitly for top-level definitions

Top-level functions, types, and variables should always have explicit access control specifiers:

```swift
public var whoopsGlobalState: Int
internal struct TheFez {}
private func doTheThings(things: [Thing]) {}
```

However, definitions within those can leave access control implicit, where appropriate:

```swift
internal struct TheFez {
	var owner: Person = Joshaber()
}
```

_Rationale:_ It's rarely appropriate for top-level definitions to be specifically `internal`, and being explicit ensures that careful thought goes into that decision. Within a definition, reusing the same access control specifier is just duplicative, and the default is usually reasonable.

#### When specifying a type, always associate the colon with the identifier

When specifying the type of an identifier, always put the colon immediately
after the identifier, followed by a space and then the type name.

```swift
class SmallBatchSustainableFairtrade: Coffee { ... }

let timeToCoffee: NSTimeInterval = 2

func makeCoffee(type: CoffeeType) -> Coffee { ... }
```

_Rationale:_ The type specifier is saying something about the _identifier_ so
it should be positioned with it.

Also, when specifying the type of a dictionary, always put the colon immediately
after the key type, followed by a space and then the value type.

```swift
let capitals: [Country: City] = [ Sweden: Stockholm ]
```

#### Only explicitly refer to `self` when required

When accessing properties or methods on `self`, leave the reference to `self` implicit by default:

```swift
private class History {
	var events: [Event]

	func rewrite() {
		events = []
	}
}
```

Only include the explicit keyword when required by the language—for example, in a closure, or when parameter names conflict:

```swift
extension History {
	init(events: [Event]) {
		self.events = events
	}

	var whenVictorious: () -> () {
		return {
			self.rewrite()
		}
	}
}
```

_Rationale:_ This makes the capturing semantics of `self` stand out more in closures, and avoids verbosity elsewhere.

#### Prefer structs over classes

Unless you require functionality that can only be provided by a class (like identity or deinitializers), implement a struct instead.

Note that inheritance is (by itself) usually _not_ a good reason to use classes, because polymorphism can be provided by protocols, and implementation reuse can be provided through composition. Structs are lightweight and can avoid several bugs due to shared access to mutations.

For example, this class hierarchy:

```swift
class Vehicle {
    let numberOfWheels: Int

    init(numberOfWheels: Int) {
        self.numberOfWheels = numberOfWheels
    }

    func maximumTotalTirePressure(pressurePerWheel: Float) -> Float {
        return pressurePerWheel * Float(numberOfWheels)
    }
}

class Bicycle: Vehicle {
    init() {
        super.init(numberOfWheels: 2)
    }
}

class Car: Vehicle {
    init() {
        super.init(numberOfWheels: 4)
    }
}
```

could be refactored into these definitions:

```swift
protocol Vehicle {
    var numberOfWheels: Int { get }
}

func maximumTotalTirePressure(vehicle: Vehicle, pressurePerWheel: Float) -> Float {
    return pressurePerWheel * Float(vehicle.numberOfWheels)
}

struct Bicycle: Vehicle {
    let numberOfWheels = 2
}

struct Car: Vehicle {
    let numberOfWheels = 4
}
```

_Rationale:_ Value types are simpler, easier to reason about, and behave as expected with the `let` keyword.


#### Prefer closure-based methods (functional) over traditional iterations

Swift provide several functional-like methods that work based on closures. This methods are `.map`, `.foreach`, `.reduce` and several others. Always prefer using them rather than using traditional style iterations `for .. in ..`.

Prefer using:

```swift
myArray.forEach { element in
    // code
}
```

rather than:

```swift
for element in myArray {
    // code
}
```

_Rationale:_ Swift closures provide good encapsulation of the context, providing better (and clearer) access to `self`. Also, several of this methods provide inmutability and result in shorter code than their traditional alternatives. 

#### Constants

Constants should be defined in his own classes, use `struct` to define them. Where to store this constants can be in different places. Always prefer them to be as near as possible to where they are used.

For example, API urls:

```swift
struct API {

    static let baseURL: String = "https://apiurl.com/"
    static let apiVersion: String = "1.1"

}
```

For those cases that you need another level of constants define them as a sub `struct` of the main category on the same file.

```swift
struct API {

    static let baseURL: String = "https://apiurl.com/"
    static let apiVersion: String = "1.1"

    struct Actions {

    	static let awesomeAction: String = "awesome-action"

    }

}
```

_Rationale:_ Having constants allows us to be able to modify their values quickly.

#### Singletons

The preferred way of doing a Singleton in swift is the following:

```swift
final class Singleton: NSObject {

    static let sharedInstance = Singleton()

    private override init() {
    }

}
```

_Note:_ The Singleton class is final so nobody can extend from that class and create another instance, and the name of the uniq instance is *sharedInstance*.

#### Prefer extensions to general `Helper` classes

```Swift
extension Double {
  func round(nearest: Double) -> Double {
    let nearestNumber = 1 / nearest
    let numberToRound = self * nearestNumber
    return numberToRound.rounded() / nearestNumber
  }
}
```

```Swift
class DoubleHelper  {
  static func round(oldNumber: Double, nearest: Double) -> Double {
    let nearestNumber = 1 / nearest
    let numberToRound = oldNumber * nearestNumber
    return numberToRound.rounded() / nearestNumber
  }
}
```

_Rationale:_ Extensions are a tool provided by the language that allows to make code clearer, adding functionality instead of having to relay to other abstractions that may not be clear at first glance. Using extension makes it look as another method from the object.

#### Omit type parameters where possible

Methods of parameterized types can omit type parameters on the receiving type when they’re identical to the receiver’s. For example:

```swift
struct Composite<T> {
	…
	func compose(other: Composite<T>) -> Composite<T> {
		return Composite<T>(self, other)
	}
}
```

could be rendered as:

```swift
struct Composite<T> {
	…
	func compose(other: Composite) -> Composite {
		return Composite(self, other)
	}
}
```

_Rationale:_ Omitting redundant type parameters clarifies the intent, and makes it obvious by contrast when the returned type takes different type parameters.

#### Always prefer Type Inference

Let the compiler infer the type for constants or variables of single instances. Type inference is also appropriate for small, non-empty arrays and dictionaries. Only use them when necessary.

Prefer:

```swift
let message = "Click the button"
let currentBounds = computeViewBounds()
var names = ["Mic", "Sam", "Christine"]
let maximumWidth: CGFloat = 106.5
```

To this:

```swift
let message: String = "Click the button"
let currentBounds: CGRect = computeViewBounds()
var names = [String]()
```

_Rationale:_ Always compact code and let the compiler do work for you.

#### Always lock the version dependencies

No matter which package manager is being used, the dependency version should always be locked to a specific version

Prefer `pod 'Alamofire', '~> 5.0.0` to `pod 'Alamofire'`

_Rationale:_ There can be breaking changes between versions. If there's no version specified, whenever someone installs them there could be another version installed, different than the one intended.

#### Code Organization

Use extensions to organize your code into logical blocks of functionality. Each extension should be set off with a `// MARK: -` comment to keep things well-organized.

#### Protocol Conformance

In particular, when adding protocol conformance to a model, prefer adding a separate extension for the protocol methods.

Prefer: 

```swift
class MyViewController: UIViewController {
  // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewController: UITableViewDataSource {
  // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewController: UIScrollViewDelegate {
  // scroll view delegate methods
}
```

To:

```swift
class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // all methods
}
```

_Rationale:_ This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a class with its associated methods.

## Styleguide rules

We extensively use [Swiftlint](https://github.com/realm/SwiftLint) as our linter tool. This allows us to automate the style-checking process, and provides an excellent integration with Xcode. 

The rules defined by SwiftLint can be found [here](https://github.com/realm/SwiftLint/blob/master/Rules.md). Please feel free to check them out. We also have the list of rules defined here. 

There are several rules that are changed/added because we found them convinient due to our experience. This are the following:

```yml
  - empty_count
  - closure_spacing
  - closure_end_indentation
  - force_unwrapping
  - multiline_arguments
  - multiline_parameters
  - operator_usage_whitespace
  - sorted_imports
```


The complete setup can be found in the [.swiftlint.yml](https://github.com/moove-it/Swift-Project-Template/blob/master/.swiftlint.yml) file of the base project. 

### SwiftLint setup

* Always have SwiftLint installed as a dependency (Cocoapods, Carthage) to avoid depending on the enviroment

### Rules

The entire list of rules can be found [here](./RULES.md)