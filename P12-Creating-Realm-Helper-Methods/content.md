---
title: "Creating Realm Helper Methods"
slug: realm-helpers
---

In this tutorial we will create Realm helper methods that will make it easier to integrate Realm into our project in the next tutorial. Our Realm helper methods will allow us to easily add new notes, update existing notes, delete old notes, and retrieve existing notes.

# Updating our Note Model

**How do we update our Note Model to be used with Realm?**

> [solution]
We need to import the `RealmSwift` header, subclass Realm's `Object` class, and indicate that our properties are `dynamic`. The `Note.swift` file should look as follows:
>
```
import Foundation
import RealmSwift
>
class Note: Object {
  dynamic var title = ""
  dynamic var content = ""
  dynamic var modificationTime = NSDate()
}
```

# Creating Realm Helper Methods

Let's create a new file called `RealmHelper.swift` in the *Helpers* folder.

In `RealmHelper.swift` we will define static methods that we can call to add, update, retrieve, and delete notes.

First we will want to import RealmSwift and define a RealmHelper class as follows:

```
import RealmSwift

class RealmHelper {
  //static methods will go here
}
```

## Add Note

We want to define a static method that accepts a `Note` object as its one parameter and then saves that note to the default Realm.

**Try it on your own and then compare with the solution below.**

> [solution]
>
```
static func addNote(note: Note){
  let realm = try! Realm()
  try! realm.write(){
    realm.add(note)
  }
}
```

## Update Note

We want to define a static method that accepts an old note and new note and updates the contents of the old note (which is assumed to already be in the default Realm) to the contents of the new note.

**Try it on your own and then compare with the solution below.**

> [solution]
>
```
static func updateNote(oldNote: Note, newNote: Note){
  let realm = try! Realm()
  try! realm.write(){
    oldNote.title = newNote.title
    oldNote.content = newNote.content
    oldNote.modificationTime = newNote.modificationTime
  }
}
```

## Delete Note

We want to define a static method that accepts a `Note` object as its one parameter and then deletes that note from the default Realm.

**You should know the drill by now ;]**

> [solution]
>
```
static func deleteNote(note: Note){
  let realm = try! Realm()
  try! realm.write(){
    realm.delete(note)
  }
}
```

## Retrieve Notes

We want to define a static method that retrieves all notes from the default Realm. Unlike the other helper methods, this one should return a `Results<Note>` object.

**Try it on your own first!**

> [solution]
>
```
static func retrieveNotes() -> Results<Note> {
  let realm = try! Realm()
  return realm.objects(Note).sorted("modificationTime", ascending: false)
}
```

Notice that I am using a method provided by Realm called `sorted()` to sort the returned objects in ascending order by their `modificationTime` property.

# Wrapping Up

We now have all the helper methods we will need to integrate Realm into Make School Notes! In the next tutorial let's (finally) add persistence to our Make School Notes app!

<!-- ACTION: Add a tl;dr info box containing all steps they should have completed on this page of the tutorial.  For an example, see page 1 of tutorial.   -->
