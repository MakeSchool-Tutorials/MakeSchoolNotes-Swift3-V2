---
title: "Integrating Realm"
slug: integrating-realm
---

Make School Notes is almost finished! In this tutorial let's integrate Realm into our project.

##Updating the List Notes Table View Controller

First, before we forget, let's import the RealmSwift module to the `ListNotesTableViewController.swift` file.

> [action]
Import the RealmSwift module by adding the following above the class definition:
>
```
import RealmSwift
```

##Retrieving Notes

<!-- ACTION: Results<Note>! is a foreign idea for first timers working with Realm and/or iOS.  Add a sentence explaining that results is a custom data type built into the Realm framework, and its relationship to our Note object in this context.  -->

Remember that all of a user's notes are displayed in the ListNotesViewController. When the app is launched, we need to retrieve all the notes from the default Realm and store them in the `notes` property of the ListNotesViewController. However, notice that our `retrieveNotes()` helper method returns a `Results<Note>` object, whereas the `notes` property has type `[Note]`.

To fix this issue, we will update the type of the `notes` property as follows:

```
var notes: Results<Note>!
```

Now we can call the `retrieveNotes()` method and store the result directly in the `notes` property.

> [action]
Update the `viewDidLoad()` method in ListNotesTableViewController as follows:
>
    override func viewDidLoad() {
      super.viewDidLoad()
      notes = RealmHelper.retrieveNotes()
    }


We call the `retrieveNotes()` method in the ListNotesViewController `viewDidLoad()` method because we want to update the `notes` property every time the ListNotesViewController is loaded.

Finally, let's ensure that the table view is always in sync with our `notes` property.

> [action]
Update the `notes` property as follows:
>
    var notes: Results<Note>! {
      didSet {
        tableView.reloadData()
      }
    }

We are using the `didSet` property observer to update the table view whenever our `notes` property is changed.

<!-- ACTION: didSet and other property observers are likely a new concept to students unfamiliar with iOS.  Expand on your example of how didSet works, so that students can generalize the use case for other times they might need to use property observers. -->

##Deleting Notes

Remember that our notes are deleted in the
`func tableView(tableView: UITableView, commitEditingStyle editingStyle: UITableViewCellEditingStyle, forRowAtIndexPath indexPath: NSIndexPath)` method.

We need to update the above method to delete the note from the default Realm as well.

> [action]
Update the `func tableView(tableView: UITableView, commitEditingStyle editingStyle: UITableViewCellEditingStyle, forRowAtIndexPath indexPath: NSIndexPath)` method as follows:
>
    override func tableView(tableView: UITableView, commitEditingStyle editingStyle: UITableViewCellEditingStyle, forRowAtIndexPath indexPath: NSIndexPath) {
      //1
      RealmHelper.deleteNote(notes[indexPath.row])
      //2
      notes = RealmHelper.retrieveNotes()
    }

1. We are deleting the note from the default Realm using the helper method we defined earlier.

2. We are updating the `notes` property to reflect the changes.

#Updating the DisplayNoteViewController

Once again, let's import the RealmSwift module before we forget!

> [action]
Add the following above the class definition in DisplayNoteViewController:
>
```
import RealmSwift
```

Remember that we both create new notes and modify existing notes in the DisplayNoteViewController. Now that we are using Realm, we must update our code to add and update notes in the default Realm as well.

> [action]
Update the `prepareForSegue()` method as follows:
>
    override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
      let destinationViewController = segue.destinationViewController as! ListNotesTableViewController
      if segue.identifier == "Save" {
        // if note exists, update title and content
        if let note = note {
          // 1
          let newNote = Note()
          newNote.title = noteTitleTextField.text ?? ""
          newNote.content = noteContentTextView.text ?? ""
          RealmHelper.updateNote(note, newNote: newNote)
        } else {
          // if note does not exist, create new note
          let note = Note()
          note.title = noteTitleTextField.text ?? ""
          note.content = noteContentTextView.text ?? ""
          note.modificationTime = NSDate()
          // 2
          RealmHelper.addNote(note)
        }
        // 3
        destinationViewController.notes = RealmHelper.retrieveNotes()
      }
    }

For the most, the above code is identical to what we had before, except for 3 things:

1. Because our updateNote RealmHelper method takes in two parameters (an old note and new note), we must create a new note and pass it to the helper.

2. We are using our Realm helper to create a new note when appropriate.

3. We are setting the `notes` property in the ListNotesViewController to be the updates objects in the default Realm.

#Running the App

Congratulations -- you have just built a fully functioning note taking app!

<!-- ACTION: Add a tl;dr info box containing all steps they should have completed on this page of the tutorial.  For an example, see page 1 of tutorial.   -->
