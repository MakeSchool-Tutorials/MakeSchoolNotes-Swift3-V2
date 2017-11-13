---
title: "Introduction to CoreData"
slug: intro-coredata
---

In the current implementation of our _Notes_ app, we're able to create, edit and delete notes. However, currently our notes are all saved in-memory and not persisted between app launches. This means that if we terminate our app, all of notes will be deleted from memory.

In this section, we'll introduce _Core Data_. A tool that can help us persist (save) our data between app launches. Finally, we'll be able to save notes and rest assured that they'll still be there later.

# What's Core Data

_Core Data_ is an Apple framework for managing an object graph. That really just means that it's a way to create and manage special data models that come with a lot of built-in functionality.

The functionality that we care most about (at least for this tutorial), is the ability to persist our notes data to our device. _Core Data_ makes this super easy by abstracting the hard-core engineering behind an easy to use API that we can use to save data locally to our device (and retrieve it too!)

In our _Notes_ app, we want to make sure each of our user's notes are saved with _Core Data_. Whenever the app launches, we want to use _Core Data_ to retrieve and display the user's previously existing notes.

In the rest of this section, we'll go over the key concepts of _Core Data_. Please note that this section is purely informational. In other words, you won't add any of the code in this section in your _Notes_ app.

So grab a bucket of popcorn and get ready for... knowledge!

## Importing Core Data

To use the _Core Data_ framework in any Swift source file, you must first add the following import statement:

```
import CoreData
```

Usually this is added at the top of your source file, outside of your class definition, along with your other import statements.

```
import UIKit
import CoreData

class ExampleClass {
	// ...
}
```

# Core Data Object Types

You can use _Core Data_ to generate simple data models for you. You won't see the code in your project, but behind the scenes, _Core Data_ creates a new class definition that subclasses `NSManagedObject`.

For instance, we could use _Core Data_ to automatically generate a data model of type `Person`. _Core Data_ would define the following class definition:

```
class Person: NSManagedObject {

		@NSManaged var name = ""
		@NSManaged var age = 0
}
```

Notice that `Person` subclasses `NSManagedObject`. This is super important in order for our `Person` data model to interact with _Core Data_. Remember, we won't have to write or handle any of this code in our _Notes_ app. _Core Data_ will do it for us.

## Initializing a Core Data Object

Next, let's look at how to initialize a _Core Data_ defined data model. When creating an instance of our `Person` data model, we'll need to do the following:

```
var chris = NSEntityDescription.insertNewObject(forEntityName: "Person", into: context) as! Person
```

Woah! A lot of new stuff is happening. Let's break it down.

1. We create a new `Person` object using the `NSEntityDescription` class method `insertNewObject(forEntityName:into:)`. All this does is create a new _Core Data_ object.
1. The parameters we pass to `insertNewObject(forEntityName:into:)` make sure _Core Data_ creates an instance of the correct class type and saves it to the right place.
1. Last, to use the properties of `Person`, we need to type cast the `NSManagedObject` to type `Person`.

## The NSManagedObjectContext

To save and retrieve data with _Core Data_, we need to use the `NSManagedObjectContext` object. In our _Notes_ app, we'll use the default `NSManagedObjectContext` accessed through our app delegate.

We can access our app's `NSManagedObjectContext` with the following code:

```
// remember to import of UIKit and CoreData
let appDelegate = UIApplication.shared.delegate as! AppDelegate
let persistentContainer = appDelegate.persistentContainer
let context = persistentContainer.viewContext
```

## Saving The NSManagedObjectContext

Saving the `NSManagedObjectContext` saves any unsaved _Core Data_ objects locally on our device.

We can save our managed context like so:

```
// reference to managed object context
let context: NSManagedObjectContext = ...

do {
	try context.save()
} catch let error {
	print("Could not save \(error.localizedDescription)")
}
```

> [info]
The `try` keyword means that calling `save()` on our `NSManagedObjectContext` may fail and _throw_ an error. In which case, we're able to use the catch block to specify what to do if `save()` fails.
>
If you're still interested in the do/try/catch paradigm, you can learn more with these [lecture slides](https://www.makeschool.com/tutorials/advanced-ios-development/error-handling-swift) or read the [official documentation](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html).

## Saving a NSManagedObject

To save an object with _Core Data_, you can simply create a new instance of the `NSManagedObject` subclass and save the managed context.

For example, if we wanted to create and save a new `Person` object, we would write the following code:

```
// reference to managed object context
let context: NSManagedObjectContext = ...

// create a new NSManagedObject subclass instance
var meredithGrey = NSEntityDescription.insertNewObject(forEntityName: "Person", into: context) as! Person

// save the NSManagedObjectContext
do {
	try context.save()
} catch let error {
	print("Could not save \(error.localizedDescription)")
}
```

In the code above, we've created a new `Person` instance and saved it locally using _Core Data_.

## Updating a NSManagedObject

Updating a `NSManagedObject` subclass is just as easy. We just need to modify the object's property and save the `NSManagedObjectContext`.

We can change the `age` property of `meredithGrey`

Let's look at how we would update the `age` property of the `meredithGrey` we created in the last step:

```
let context: NSManagedObjectContext = ...
var meredithGrey: Person = ...

// update an existing NSManagedObject subclass instance
meredithGrey.age = 6

// save the NSManagedObjectContext
do {
	try context.save()
} catch let error {
	print("Could not save \(error.localizedDescription)")
}
```

The code above, updates the `age` property of our existing `Person` instance and saved the change with _Core Data_.

## Retrieving NSManagedObject(s)

To retrieve `NSManagedObject(s)`, we'll use a `NSFetchRequest`. A fetch request allows us to specify what type of object we want to retrieve with _Core Data_.

For instance, we can retrieve all `Person` objects with the following code:

```
// create a NSFetchRequest to retrieve all Person objects
let fetchRequest = NSFetchRequest<Person>(entityName: "Person")

// use the NSManagedObjectContext to execute the NSFetchRequest
do {
	let results = try context.fetch(fetchRequest)
} catch let error {
	print("Could not fetch \(error.localizedDescription)")
}
```

We use the method `fetch(_:)` on our `NSManagedObjectContext` instance to retrieve all objects of type `Person` saved to that `NSManagedObjectContext`.

## Deleting a NSManagedObject

Last, let's learn how to delete a `NSManagedObject` subclass. `NSManagedObjectContext` has a `delete(_:)` method that takes a `NSManagedObject` as a parameters. After deleting the note, we must also save our `NSManagedObjectContext`.

We can use this method to delete objects from a managed object context:

```
// reference to managed object context
let context: NSManagedObjectContext = ...

context.delete(meredithGrey)

// save the NSManagedObjectContext
do {
	try context.save()
} catch let error {
	print("Could not save \(error.localizedDescription)")
}
```

The code above has deleted our `meredithGrey` instance from our `NSManagedObjectContext`.

# Wrapping Up

In this section, we've learned a lot of... knowledge!

We've gone over how to add, modify, retrieve and delete `NSManagedObject` objects with _Core Data_.

Next, let's use our new-found knowledge of _Core Data_ to implement persistence for our notes.

> [info]
"The possession of Knowledge, unless accompanied by a manifestation and expression in Action, is like the hoarding of precious metals-a vain and foolish thing. Knowledge, like wealth, is intended for Use. The Law of Use is Universal, and he who violates it suffers by reason of his conflict with natural forces." - The Kybalion
