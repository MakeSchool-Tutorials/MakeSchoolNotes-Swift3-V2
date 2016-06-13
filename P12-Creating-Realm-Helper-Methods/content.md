---
title: "Creating Realm Helper Methods"
slug: realm-helpers
---

In this section of the tutorial we will create Realm helper methods that will make it easier to integrate Realm into our project in the next section. The Realm helper methods will allow us to easily add new notes, update existing notes, delete old notes, and retrieve existing notes.

##Updating the Note Class

**What do we need to do to update the Note class so that it can be used with Realm?** Don't be afraid to refer back to the previous page; the "Realm's Object Type" section in particular.

> [solution]
We need to import the `RealmSwift` header, make `Note` a subclass of Realm's `Object` class, and make the properties are `dynamic`. The `Note.swift` file should look as follows:
>
	import Foundation
	import RealmSwift
>	
	class Note: Object {
	  dynamic var title = ""
	  dynamic var content = ""
	  dynamic var modificationTime = NSDate()
	}


#Creating Realm Helper Methods

> [action]
Create a new file called `RealmHelper.swift` in the *Helpers* group.

In `RealmHelper.swift` we will define *static methods* that we can call to add, update, retrieve, and delete notes. 

First we will want to import RealmSwift and define a RealmHelper class as follows:

	import RealmSwift
	
	class RealmHelper {
	  //static methods will go here
	}

##Static Methods

Static methods are methods that can be called directly on the class, without having to instantiate an instance of the class first. For example, if we add a static method called `doSomething()` to the `RealmHelper` class, we can call it like this:
 
	RealmHelper.doSomething()

If `doSomething()` were not static, calling it would look like more like this:
 
 	let realmHelper = RealmHelper()
 	realmHelper.doSomething()
 	
To declare a static method is very easy, just add the `static` keyword to the front of the declaration. Here's how our `RealmHelper` would look with a static `doSomething()` method:

	class RealmHelper {
		static func doSomething() {
		
		}
	}

##Add Note

We want to define a static method that accepts a `Note` object as its one parameter and then saves that note to the default Realm.

> [action]
**Try it on your own and then compare with the solution below.** It's okay to look back to the last section to see how it's done.

<!-- html comment to break boxes -->

> [solution]
>
	static func addNote(note: Note) {
		let realm = try! Realm()
		try! realm.write() {
			realm.add(note)
		}
	}

##Delete Note

We want to define a static method that accepts a `Note` object as its one parameter and then deletes that note from the default Realm.

> [action]
> **Try it on your own first!**


<!-- html comment to break boxes -->

> [solution]
>
	static func deleteNote(note: Note) {
		let realm = try! Realm()
		try! realm.write() {
			realm.delete(note)
		}
	}

##Update Note

We want to define a static method that accepts a `noteToBeUpdated` and a `newNote`. The `noteToBeUpdated` is assumed to already be in the default Realm - we want to update that note's contents with the contents of the `newNote`, and we want to do it in such a way that the it's updated in Realm too.

> [action]
**Try it on your own and then compare with the solution below.**

<!-- html comment to break boxes -->

> [solution]
> If you couldn't figure it out on your own, that's okay. It's a little hard to explain, but reading the code should make it pretty clear what's happening:
>
	static func updateNote(noteToBeUpdated: Note, newNote: Note) {
		let realm = try! Realm()
		try! realm.write() {
			noteToBeUpdated.title = newNote.title
			noteToBeUpdated.content = newNote.content
			noteToBeUpdated.modificationTime = newNote.modificationTime
		}
	}

##Retrieve Notes

We want to define a static method that retrieves all notes from the default Realm. Unlike the other helper methods, this one should return a `Results<Note>` object. For bonus points, see if you can figure out how to sort the notes based on their modification time.

> [action]
**Try it on your own, don't be afraid to refer back to the previous section!w**

<!-- html comment to break boxes -->

> [solution]
>
	static func retrieveNotes() -> Results<Note> {
		let realm = try! Realm()
		return realm.objects(Note).sorted("modificationTime", ascending: false)
	}
>
Notice that I am using a method provided by Realm called `sorted()` to sort the returned objects in ascending order by their `modificationTime` property.

#Wrapping Up

We now have all the helper methods we will need to integrate Realm into Make School Notes! In the next section let's (finally) add persistence to our Make School Notes app!

>[info]
>###On this page, you should have:
>
>1. Learned what a static method is, and how to make one.
>2. Created the `RealmHelper` class.
>3. Implemented four different static helper methods, ones to add, delete, update and retrieve notes from Realm.