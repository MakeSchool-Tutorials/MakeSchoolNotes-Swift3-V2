## Introducing Protocols

Protocols are like contracts: Contracts formalize agreements between people by stating the agreement in writing. Similarly, protocols formalize the agreement between objects by stating the type of information that an object can request from another object. Specifically, protocols declare properties and methods. Consider this snippet of the `UITableViewDataSource` protocol:

```
public protocol UITableViewDataSource : NSObjectProtocol {

public func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int

public func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell

optional public func numberOfSectionsInTableView(tableView: UITableView) -> Int

optional public func tableView(tableView: UITableView, titleForHeaderInSection section: Int) -> String?

optional public func tableView(tableView: UITableView, titleForFooterInSection section: Int) -> String?
}
```

The protocol declares, but does not implement, two *required* methods (the top two) and three *optional* methods (prefixed with the word *optional*). The methods in the code above serve as the contract.

*Conforming to a protocol* is like signing a contract: Once someone signs a contract, that person is legally obligated to uphold the agreement stated within the contract. If someone breaks the contract, they could get fined or go to jail. Similarly, when an object *conforms to a protocol*, it is obligated to implement any properties and methods *required* by the protocol. If an object conforms to a protocol (signs the contract), but does not define the required properties and methods (breaks the contract), the program will not compile (goes to jail).

To conform to a protocol, an object must add the name of the protocol to its definition. For example, if we had a class named `CustomViewController` (assuming its a subclass of `UIViewController`) and wanted it to conform to the `UITableViewDataSource` protocol, we would define `CustomViewController` as follows:

```
class CustomViewController: UIViewController, UITableViewDataSource {
//...
}
```

The name of the protocol follows immediately after the name of the superclass, and is separated by a comma.

Although Swift does not support multiple inheritance, it does support objects conforming to multiple protocols, like so:

```
class CustomViewController: UIViewController, UITableViewDataSource, UITableViewDelegate, SomeCustomProtocol {
//...
}
```

When objects conform to many different protocols, as above, it is very easy to lose track of which properties and methods belong to which protocol. To combat this problem and keep our code base organized, we often use *extensions* to conform to protocols, like so:

```
class CustomViewController: UIViewController {
//...
}

extension CustomViewController: UITableViewDataSource {
//...
}

extension CustomViewController: UITableViewDelegate {
//...
}

extension CustomViewController: SomeCustomProtocol {
//...
}
```

For more information on *extensions* checkout this [quick discussion--BROKEN LINK](link to extensions discussion).
