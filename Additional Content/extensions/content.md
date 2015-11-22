## Introducing Extensions

In iOS, *extensions* are a clean and structured way to add functionality to an existing type. Extensions can be used on any type in iOS, including classes, structs, enums, and protocols.

Extensions are very powerful because not only do they allow us to add functionality to our own custom types, but also to types provided by libraries and frameworks (including Apple's frameworks)!

To extend a type, we use the `extension` keyword. For instance, if we wanted to use a specific blue color in our application, we could extend the `UIColor` class (which is provided by Apple's `UIKit` framework) to include the following type method:

```
extension UIColor {
  static func blueApplicationColor() {
  //...
  }
}
```

Now when we want to access our special blue color, all we have to do is:

```
UIColor.blueApplicationColor()
```

Oftentimes we use extensions to conform to protocols; we do this to keep our code clean and organized. For example, if we had a class named `CustomViewController` and wanted it to conform to the `UITableViewDataSource` protocol, we would define `CustomViewController` as follows:  

```
extension CustomViewController: UITableViewDataSource {
//...
}
```

Now when we implement the properties and methods defined in the `UITableViewDataSource` protocol, we will do so in one centralized place, inside the extension. This makes it easier to find code later on should we need to make changes or debug.
