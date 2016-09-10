---
title: "Creating CoreData Helper Methods"
slug: CoreData-helpers
---

In this section of the tutorial we will create CoreData helper methods that will make it easier to integrate CoreData into our project in the next section. The CoreData helper methods will allow us to easily add new notes, update existing notes, delete old notes, and retrieve existing notes.

##Updating the Note Class

**What do we need to do to update the Note class so that it can be used with CoreData?** Don't be afraid to refer back to the previous page; the "CoreData's Object Type" section in particular.

> [solution]
We need to import the `CoreData` header, make `Note` a subclass of CoreData's `NSManagedObject` class, and make the properties are `@NSManaged`. The `Note.swift` file should look as follows:
>
	import Foundation
	import CoreData
>
	class Note: NSManagedObject {
		@NSManaged public var title: String?
		@NSManaged public var content: String?
		@NSManaged public var modificationTime: Date?
		convenience init() {
		    let appDelegate = 	UIApplication.shared.delegate as! AppDelegate
	    	let persistentContainer = appDelegate.persistentContainer
	    	let managedContext = persistentContainer.viewContext
    		let entity = NSEntityDescription.entity(forEntityName: "Note", in: managedContext)
	    	self.init(entity: entity!, insertInto: managedContext)
		}
	}

##Add Note entity in the data model
Click MakeSchoolNotes.xcdatamodeld and you will see your data model for CoreData. Follow the following steps to create an entity in CoreData

1. On the bottom left, you will see "Add Entity". Click it.
2. On the top left, you will see the entity you just created called "Entity". Double click "Entity" to change the name to "Note"
3. On the right side, you will see a section called "Attributes". You will need to create 3 attributes for Note. Click the "+" sign 3 times and add the following attributes.

		title with type String

		content with type String

		modificationTime with type Date

4. On the right side, change the class from the default NSManagedObject to the Note class you defined.


#Creating CoreData Helper Methods

> [action]
Create a new file called `CoreDataHelper.swift` in the *Helpers* group.

In `CoreDataHelper.swift` we will define *static methods* that we can call to add, update, retrieve, and delete notes.

First we will want to import CoreData and define a CoreDataHelper class as follows:

	import CoreData

	class CoreDataHelper {
	  //static methods will go here
	}

##Static Methods

Static methods are methods that can be called directly on the class, without having to instantiate an instance of the class first. For example, if we add a static method called `doSomething()` to the `CoreDataHelper` class, we can call it like this:

	RCoreDataHelper.doSomething()

If `doSomething()` were not static, calling it would look like more like this:

 	let CoreDataHelper = CoreDataHelper()
 	CoreDataHelper.doSomething()

To declare a static method is very easy, just add the `static` keyword to the front of the declaration. Here's how our `CoreDataHelper` would look with a static `doSomething()` method:

	class CoreDataHelper {
		static func doSomething() {

		}
	}

##Save Note

We want to define a static method that accepts a `Note` object as its one parameter and then saves that note to the default CoreData.

> [action]
**Try it on your own and then compare with the solution below.** It's okay to look back to the last section to see how it's done.

<!-- html comment to break boxes -->

> [solution]
>
	static func addNote(note: Note) {
		do {
			try managedContext.save()
		} catch let error as NSError {
			print("Could not save \(error)")
		}
	}

##Delete Note

We want to define a static method that accepts a `Note` object as its one parameter and then deletes that note from the default CoreData.

> [action]
> **Try it on your own first!**


<!-- html comment to break boxes -->

> [solution]
>
	static func deleteNote(note: Note) {
		managedContext.delete(note)
		saveNote()
	}


##Retrieve Notes

We want to define a static method that retrieves all notes from the default CoreData. Unlike the other helper methods, this one should return a `[Note]` object.

> [action]
**Try it on your own, don't be afraid to refer back to the previous section!w**

<!-- html comment to break boxes -->

> [solution]
>
	static func retrieveNotes() -> Results<Note> {
		let fetchRequest = NSFetchRequest<Note>(entityName: "Note")
		do {
			let results = try managedContext.fetch(fetchRequest)
			return results
		} catch let error as NSError {
			print("Could not fetch \(error)")
		}
		return []
	}
>

#Wrapping Up

We now have all the helper methods we will need to integrate CoreData into Make School Notes! In the next section let's (finally) add persistence to our Make School Notes app!

>[info]
>###On this page, you should have:
>
>1. Learned what a static method is, and how to make one.
>2. Created the `CoreDataHelper` class.
>3. Implemented four different static helper methods, ones to add, delete, update and retrieve notes from CoreData.
