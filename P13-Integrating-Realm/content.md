---
title: "Integrating Realm"
slug: integrating-realm
---

Make School Notes is almost finished! In this section let's integrate Realm into our project.

##Updating the List Notes Table View Controller

First, before we forget, let's import the `RealmSwift` module into `ListNotesTableViewController.swift`.

> [action]
Import the RealmSwift module by adding the following above the class definition:
>
	import RealmSwift


##Retrieving Notes

Remember that we want all of the user's notes to be displayed in the ListNotesViewController. When the app is launched, we need to retrieve all the notes from the default Realm and store them in the `notes` property of the ListNotesViewController. However, notice that our `retrieveNotes()` helper method returns a `Results<Note>` object, whereas the `notes` property has type `[Note]`.

> [action]
> 
To fix this issue, we will update the type of the `notes` property as follows:
>
	var notes: Results<Note>!

<!-- html comment to break boxes -->

> [info]
>
`Results<>` is a special Realm container type that holds the result of a query. For us, it holds notes, so it looks like `Results<Note>`. It behaves similarly to an array of Note objects, but it has some special optimizations that make it perform better when working with a very large number of objects (this won't really matter in our case). It also has some nice methods to allow us to sort the elements. 

Now we can call the `retrieveNotes()` method and store the result directly in the `notes` property.

> [action]
Update the `viewDidLoad()` method in ListNotesTableViewController as follows:
>
    override func viewDidLoad() {
     	super.viewDidLoad()
    	notes = RealmHelper.retrieveNotes()
    }

We're calling the `retrieveNotes()` method in the ListNotesViewController `viewDidLoad()` method because we want to update the `notes` property every time the ListNotesViewController is loaded.

Finally, let's ensure that the table view is always in sync with our `notes` property.

> [action]
Update the `notes` property as follows:
>
    var notes: Results<Note>! {
    	didSet {
    		tableView.reloadData()
    	}
    }

Remember the `didSet` from before?  As a reminder, it's a kind of *property observer* and it allows us to execute some code whenever the property its declared under changes. In this case, every time the `notes` property changes, we tell the table view to reload its data.

##Deleting Notes

Remember that when we implemented the ability to delete notes, we did so in the 
`tableView(_:commitEditingStyle:forRowAtIndexPath:)` method.

We need to update the above method to delete the note from Realm as well.

> [action]
Update the `tableView(tableView:commitEditingStyle:forRowAtIndexPath:)` method as follows:
>
    override func tableView(tableView: UITableView, commitEditingStyle editingStyle: UITableViewCellEditingStyle, forRowAtIndexPath indexPath: NSIndexPath) 
    {
    	if editingStyle == .Delete { 
	  	 	//1
	    	RealmHelper.deleteNote(notes[indexPath.row])
	    	//2
	    	notes = RealmHelper.retrieveNotes()
    	}
    }

1. Here we delete the note from Realm using the helper method we defined earlier. Notice how we can index in to the `notes` object the same way we could before, despite it being a `Results<Note>` now instead of a `[Note]`. Also like before, we use the `indexPath.row` to index into `notes` to delete the correct one.

2. Here we update the `notes` property to reflect the changes.

#Updating the DisplayNoteViewController

Once again, let's import the `RealmSwift` module before we forget!

> [action]
Add the following above the class definition in DisplayNoteViewController:
>
	import RealmSwift

Remember that we both create new notes and modify existing notes in the DisplayNoteViewController. Now that we are using Realm, we must update our code to add and update notes to Realm as well.

> [action]
Update `prepareForSegue(_:sender:)` as follows:
>
    override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
        let listNotesTableViewController = segue.destinationViewController as! ListNotesTableViewController
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
            listNotesTableViewController.notes = RealmHelper.retrieveNotes()
        }
    }

For the most, the above code is identical to what we had before, except for 3 things:

1. Because the `updateNote(_:newNote:)` RealmHelper method takes two parameters (an old note to be updated and new note), we must create a new note with the user's updated note content and pass it to the helper.

2. Here we use our Realm helper to add the newly created note to Realm, so that it's persisted.

3. Here we make sure that the ListNotesTableViewController is up to date with the changes we just made by retrieving all the notes from Realm and assigning them to the `notes` property.

#Running the App

Congratulations -- you have just built a fully functioning note taking app! Run it and test it out! You can even try force quitting Make School Notes, then reopening it to see that beautiful persistance. To force quit, press the home button twice (**shift+command+h**, ⇧⌘H in simulator) and swipe up on Make School Notes. You should see all the notes you saved when it re-opens!

>[info]
>###On this page, you should have:
>
>1. Updated ListNotesViewController to have the Realm `Result<Note>` type for its `notes` property.
>2. Updated ListNotesViewController's `viewDidLoad()` to grab all the notes from Realm.
>3. Added a `didSet` property listener on the `notes` property so that it reloads the table view every time the property is updated
>4. Updated ListNotesViewController's `tableView(tableView:commitEditingStyle:forRowAtIndexPath:)` method to delete the notes from Realm when they're deleted from the table view.
>5. Updated DisplayNoteViewController's `prepareForSegue(_:sender:)` method to save or update the notes in Realm, depending on what the user does. Also it now updates the `notes` property in the ListNotesViewController to make sure it's fully up to date.
>6. Run and tested your fully functioning app!
