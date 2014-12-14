# Style guide for the Swift programming language

These guidelines build on the coding style used in Apple's official references and sample codes, in particular [The Swift Programming Language](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/) book.

This style guide is suited for projects and collaborations and does not focus on conserving space for print and web. For that check out [The Official raywenderlich.com Swift Style Guide](https://github.com/raywenderlich/swift-style-guide)


## Table of Contents

* [Whitespace](#whitespace)
* [Comments and Documentation](#comments-and-documentation)
* [Naming](#naming)
* [Structures and Classes](#structures-and-classes)
    * [Use of Self](#use-of-self)
    * [Protocol Conformance](#protocol-conformance)
* [Closure Expressions](#closure-expressions)


## Whitespace

* Prefere indent using spaces with a tab and indent width of 4 spaces. Be sure to set this in the Xcode preferences.
* Struct, enum, class, method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.

```swift
struct SomeStructure {
    // structure definition goes here
}

class SomeClass {
    func someMethod() {
        // method definition goes here
    }
}
```

```swift
if user.isHappy {
    // do something
}
else {
    // do something else
}
```

* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but having too many sections in a method often means you should refactor into several methods.


## Comments and Documentation

When they are needed, comments should be used to explain **why** a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Use the `// Mark:` tag to divide functionality into meaningful, easy-to-navigate sections. (`// Mark: -` can optionally be used to provide a horizontal divider in Xcode's outline)

**For example:**

```swift
/**
*  The `ListViewController` class displays awesome stuff
*/
class ListViewController: UIViewController {
    // MARK: Types

    struct MainStoryboard {
        struct SegueIdentifiers {
            // ...
        }
    }

    // MARK: Properties

    /// Set in `configureWithListInfo(_:)`. `nil` otherwise.
    var listInfo: ListInfo?

    // ...

    // MARK: View Life Cycle

    override func viewDidLoad() {
        // ...
    }

    override func viewWillAppear(animated: Bool) {
        // ...
    }

    // ...

    // MARK: Setup

    // ...

    // MARK: UIViewController Overrides

    override func setEditing(editing: Bool, animated: Bool) {
        // ...
    }

    // MARK: IBActions

    // ...

    // MARK: Convenience

    // ...
}
```

Check out [NSHipster's Swift Documentation](http://nshipster.com/swift-documentation/) article on how to use reST ([reStructuredText](http://docutils.sourceforge.net/docs/user/rst/quickref.html)) for documentation.


## Naming

Use descriptive names with camel case notation for classes, methods, variables, etc. Struct, enum and class names should be capitalized, while method names and variables should start with a lower case letter.

**Preferred:**

```swift
struct FixedLengthRange {
    var firstValue: Int
    let length: Int
}
```

**Not Preferred:**

```swift
struct fixed_lengthRange {
    var v: Int
    let LENGTH: Int
}
```

For functions and initialization methods, prefer named parameters for **all** arguments unless the context is very clear. Include external parameter names if it makes function calls more readable.

```swift
func dateFromString(dateString: String) -> NSDate
func convertPointAt(#column: Int, #row: Int) -> CGPoint
func timedAction(#delay: NSTimeInterval, perform action: SKAction) -> SKAction!

// would be called like this:
dateFromString("2014-03-14")
convertPointAt(column: 42, row: 13)
timedAction(delay: 1.0, perform: someOtherAction)
```

## Structures and Classes

### Use of Self

For conciseness, avoid using `self` since Swift does not require it to access an object's properties or invoke its methods.

Only include the explicit keyword when required by the language—for example, in a closure, or when parameter names conflict.

```swift
struct Color {
    let red, green, blue: Double

    init(red: Double, green: Double, blue: Double) {
        self.red = red
        self.green = green
        self.blue = blue
    }

    init(white: Double) {
        red = white
        green = white
        blue = white
    }
}
```

### Protocol Conformance

When adding protocol conformance to a class, prefer extending it to adopt and conform to the new protocol. This keeps the related methods grouped together and maintains structure.

**Preferred:**

```swift
class ListViewController: UIViewController {
    // class definition goes here
}

// MARK: UITableViewDataSource

extension ListViewController: UITableViewDataSource {
    // UITableViewDataSource conformance goes here
}

// MARK: UITableViewDelegate

extension ListViewController: UITableViewDelegate {
    // UITableViewDelegate conformance goes here
}
```

**Not Preferred:**

```swift
class ListViewController: UIViewController, UITableViewDataSource, UITableViewDelegate {
    // class definition goes here
    // UITableViewDataSource, UITableViewDelegate conformance goes here
}
```

## Closure Expressions

Use trailing closure syntax wherever possible. Only use the provided shorthand argument names (`$0, $1, ...`) to refer to the values of the closure's arguments when the context provides an unambiguous assignment. Otherwise give the closure parameters descriptive names.

```swift
someFunctionThatTakesAClosure​() {
    // trailing closure's body goes here
​}

UIView.animateWithDuration(1.0, animations: {
    // animation code goes here
    }) { finished in
        // completion code goes here
    }
```

Omit the the `return` keyword in single-expression closures:

```swift
​reversed​ = ​sorted​(​names​) { ​$0​ > ​$1​ }
```
