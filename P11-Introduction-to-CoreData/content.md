---
title: "Introduction to Core Data"
slug: intro-coredata
---

With the ability to add, modify, and delete notes, our app is nearly finished! The last step is to persist data between app launches so that user's notes won't get deleted every time they close the app.

Core Data is a native mobile database that is built by Apple. Core Data is used to save and retrieve objects locally on our user's device. For Make School Notes, we want to save all of a user's notes when they close the app, and retrieve all of their notes when they reopen the app.

##Importing the CoreData Framework

When using the CoreData framework in Swift you must use the following import statement:

	import CoreData


#CoreData's Object Type

When using CoreData to store objects, the object we are storing must inherit from a CoreData provided class called `NSManagedObject`. For instance, if we wanted to store and retrieve objects of type `Person`, we would declare the `Person` class as follows:

	class Person: NSManagedObject {
	    //...
	}

If our hypothetical Person class had `name` and `age` properties, they would need to be declared `@NSManaged` like so:

	class Person: NSManagedObject {
	  @NSManaged var name = ""
	  @NSManaged var age = 0
	}

Despite the fact that we added a special superclass and a fancy keyword to the properties of our `Person` class, it will still behave the same as a regular class. We would create an instance of the `Person` class like this:

	var chris = Person()
	chris.name = "Chris"
	chris.age = 23

##CoreData's NSManagedContext Type

We save and retrieve objects from the *NSManagedContext*. NSManagedContext can be configured in many different ways, but for our purposes, the default NSManagedContext will suffice.

Before adding, modifying, retrieving, or deleting objects in CoreData, we must get access to the default NSManagedContext:

	let appDelegate = UIApplication.shared.delegate as! AppDelegate
	let persistentContainer = appDelegate.persistentContainer
	let managedContext = persistentContainer.viewContext

##Initializing an entry in CoreData
We have to have a convenience initializer to make our NSManagedObject know which entity and managedContext it belongs to. So the following would be the full implmentation of the Person class.

	class Person: NSManagedObject {
		@NSManaged var name = ""
		@NSManaged var age = 0
		convenience init() {
        let appDelegate = UIApplication.shared.delegate as! AppDelegate
        let persistentContainer = appDelegate.persistentContainer
        let managedContext = persistentContainer.viewContext
        let entity = NSEntityDescription.entity(forEntityName: "Person", in: managedContext)
        self.init(entity: entity!, insertInto: managedContext)
    }
	}

##Save transactions in CoreData

*Save transactions* are a special way to inform CoreData that we want to change something in the default managedContext. For instance, if we wanted to add, modify, or delete an object we would do so inside of a write transaction. (We do not need to retrieve objects inside write transactions because object retrieval does not change anything in the default managedContext.)

To start a save transaction, we do the following:

	//save transaction
	do {
      try managedContext.save()
	} catch let error as NSError {
      print("Could not save \(error)")
	}

The `try` keyword in the above code signals that the call to `managedContext.save()` can *throw an error*. (Throwing an error is a fancy way of saying that a method can fail.) By using the `try!` keyword (note the exclamation point) we are indicating that we know the `managedContext.save()` method can throw an error, but that we are sure that it will not, and therefore will not handle the error case.

In Swift, errors are handled using the do/try/catch paradigm. For more information on the do/try/catch paradigm of error handling, check out these [lecture slides](https://www.makeschool.com/tutorials/advanced-ios-development/error-handling-swift) or read the [official documentation](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html).
##Saving Objects

In order to save an object in CoreData, we just need to create a instance of the class and then make the save transaction shown above. If we wanted to save the `chris` variable from above, we would do the following:

	//save transaction
	do {
		try managedContext.save()
	} catch let error as NSError {
		print("Could not save \(error)")
	}

The above code saves the `chris` variable to the default Realm.

##Updating Objects

Updating objects in CoreData. All we have to do is modify the instance of the Object and make a save transaction. If we wanted to change the age property of the `chris` variable, we would do the following:

	chris.age = 100
	do {
		try managedContext.save()
	} catch let error as NSError {
		print("Could not save \(error)")
	}

The above code will both update the age of `chris` and save it in CoreData.

##Retrieving Objects

We use a *NSFetchRequest* to retrieve NSManagedObjects. We have to specify which kind of objects we want and which entity we should get from. If we wanted to retrieve all of our *Person* objects we would do the following:

	let fetchRequest = NSFetchRequest<Person>(entityName: "Person")
	do {
      let results = try managedContext.fetch(fetchRequest)
	} catch let error as NSError {
      print("Could not fetch \(error)")
	}

The call to `managedContext.fetch(fetchRequest)` will return all of the objects of type `Person` that have been saved to the managedContext.

##Deleting Objects

Once inside the write transaction, we can delete an object using the `delete()` method. We pass the object we want to delete to the `delete()` method. If we wanted to delete the `chris` variable, we would do the following:

	managedContext.delete(chris)
        saveNote()

The above code deletes the `chris` variable from the default managedContext.

#Wrapping Up

We have discussed how to add, modify, retrieve, and delete objects in CoreData and are ready to add data persistence to Make School Notes!


>[info]
>###On this page, you should have:
>
>1. Learned the basic ways you can interact with Realm to persist data.
