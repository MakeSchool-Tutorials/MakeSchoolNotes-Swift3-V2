---
title: "Introduction to CoreData"
slug: intro-coredata
---

With the ability to add, modify, and delete notes, our app is nearly finished! The last step is to persist data between app launches so that user's notes won't get deleted every time they close the app.

CoreData is a native mobile database that is built by Apple. CoreData is used to save and retrieve objects locally on our user's device. For Make School Notes, we want to save all of a user's notes when they close the app, and retrieve all of their notes when they reopen the app.

##Importing the CoreData Framework

When using the CoreData framework in Swift you must use the following import statement:

```
import CoreData
```

#CoreData's Object Type
As of Xcode 8, CoreData now generates code for you, based on a data model that you specify. It used to be that we had to both give CoreData a model AND write all of the code ourselves, but now Xcode will do it for us! That being said, it's important for you to know a bit about what Xcode is doing for you, so we're going to explain some of how CoreData works. This will help when you go on to build your own app, and will help prevent any issues you might run into.

When CoreData stores and object, the object inherits from a CoreData provided class called `NSManagedObject`. For instance, if we wanted to store and retrieve objects of type `Person`, we would declare the `Person` class as follows:

```
class Person: NSManagedObject {
	//...
}
```

If our hypothetical Person class had `name` and `age` properties, they would need to be declared `@NSManaged` like so:

```
class Person: NSManagedObject {
	@NSManaged var name = ""
	@NSManaged var age = 0
}
```

Because we added a special superclass and a fancy keyword to the properties of our `Person` class, it will not always behave the same as a regular class.


## Creating an instance of a CoreData Object
Now that we've given you a behind-the-scenes look at CoreData, let's talk about what code you will need to write yourself. The rest of the code we will go over in this section will be code you _will_ have to use in your projects. In the future, we would create an instance of the `Person` class like this:

```
var chris = NSEntityDescription.insertNewObject(forEntityName: "Person", into: managedContext) as! Person
chris.name = "Chris"
chris.age = 23
```
While this looks scary, we'll break it down so that it makes sense. Let's take a look at it bit by bit:

```
var chris = NSEntityDescription.insertNewObject
```
To create a new variable with the name chris, we're calling a class method on something called NSEntityDescription, which is just an object that is stored in CoreData. We're simply adding a new object.

```
(forEntityName: "Person", into: managedContext)
```
The parameters that are passed into the insertNewObject function are the name of the entity that we are adding, and where it's going. We have to specify these, because in a larger app, you may have multiple objects being stored into CoreData, and multiple managedContexts, or places that those objects could go.

```
as! Person
```
We then force a cast of the object that is retrieved into a Person object, because CoreData doesn't directly store your object as itself, it is converted into a special type of data that can easily be stored and retrieved from your phone's storage.

##CoreData's NSManagedContext Type
We save and retrieve objects from the *NSManagedContext*. NSManagedContext can be configured in many different ways, but for our purposes, the default NSManagedContext will suffice.

Before adding, modifying, retrieving, or deleting objects in CoreData, we must get access to the default NSManagedContext:

```
// needs import of UIKit and CoreData
let appDelegate = UIApplication.shared.delegate as! AppDelegate
let persistentContainer = appDelegate.persistentContainer
let managedContext = persistentContainer.viewContext
```

##Save transactions in CoreData

*Save transactions* are a special way to inform CoreData that we want to change something in the default managedContext. For instance, if we wanted to add, modify, or delete an object we would do so inside of a write transaction. (We do not need to retrieve objects inside write transactions because object retrieval does not change anything in the default managedContext.)

To start a save transaction, we do the following:

```
//save transaction
do {
	try managedContext.save()
} catch let error as NSError {
	print("Could not save \(error)")
}
```

The `try` keyword in the above code signals that the call to `managedContext.save()` can *throw an error*. (Throwing an error is a fancy way of saying that a method can fail.) By using the `try!` keyword (note the exclamation point) we are indicating that we know the `managedContext.save()` method can throw an error, but that we are sure that it will not, and therefore will not handle the error case.

In Swift, errors are handled using the do/try/catch paradigm. For more information on the do/try/catch paradigm of error handling, check out these [lecture slides](https://www.makeschool.com/tutorials/advanced-ios-development/error-handling-swift) or read the [official documentation](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html).

##Saving Objects

In order to save an object in CoreData, we just need to create a instance of the class and then make the save transaction shown above. If we wanted to save the `chris` variable from above, we would do the following:

```
//save transaction
do {
	try managedContext.save()
} catch let error as NSError {
	print("Could not save \(error)")
}
```

The above code saves the `chris` variable to the default CoreData.

##Updating Objects

Updating objects in CoreData. All we have to do is modify the instance of the Object and make a save transaction. If we wanted to change the age property of the `chris` variable, we would do the following:

```
chris.age = 100
do {
	try managedContext.save()
} catch let error as NSError {
	print("Could not save \(error)")
}
```

The above code will both update the age of `chris` and save it in CoreData.

##Retrieving Objects

We use a *NSFetchRequest* to retrieve NSManagedObjects. We have to specify which kind of objects we want and which entity we should get from. If we wanted to retrieve all of our *Person* objects we would do the following:

```
let fetchRequest = NSFetchRequest<Person>(entityName: "Person")
do {
	let results = try managedContext.fetch(fetchRequest)
} catch let error as NSError {
	print("Could not fetch \(error)")
}
```

The call to `managedContext.fetch(fetchRequest)` will return all of the objects of type `Person` that have been saved to the managedContext.

##Deleting Objects

Once inside the write transaction, we can delete an object using the `delete()` method. We pass the object we want to delete to the `delete()` method. If we wanted to delete the `chris` variable, we would do the following:

```
managedContext.delete(chris)
```

The above code deletes the `chris` variable from the default managedContext.

#Wrapping Up

We have discussed how to add, modify, retrieve, and delete objects in CoreData and are ready to add data persistence to Make School Notes!


>[info]
>###On this page, you should have:
>
>1. Learned about what code CoreData generates for you, and what code you will have to write yourself.
>1. Learned the basic ways you can interact with CoreData to persist data.
