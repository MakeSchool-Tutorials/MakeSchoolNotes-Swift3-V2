---
title: "Integrating CoreData"
slug: integrating-CoreData
---

Make School Notes is almost finished! In this section let's integrate CoreData into our project.

##Retrieving Notes

Remember that we want all of the user's notes to be displayed in the ListNotesViewController. When the app is launched, we need to retrieve all the notes from the default CoreData and store them in the `notes` property of the ListNotesViewController.
Now we can call the `retrieveNotes()` method and store the result directly in the `notes` property.

> [action]
Update the `viewDidLoad()` method in ListNotesTableViewController as follows:
>
    override func viewDidLoad() {
     	super.viewDidLoad()
    	notes = CoreDataHelper.retrieveNotes()
    }

We're calling the `retrieveNotes()` method in the ListNotesViewController `viewDidLoad()` method because we want to update the `notes` property every time the ListNotesViewController is loaded.

> [action]
Also, update the `unwindToListNotesViewController()` method in ListNotesTableViewController as follows:
>
    @IBAction func unwindToListNotesViewController(_ segue: UIStoryboardSegue){
        self.notes = CoreDataHelper.retrieveNotes()
    }

We're calling the `retrieveNotes()` method in the ListNotesViewController `unwindToListNotesViewController()` method because we want to update the `notes` property every time the ListNotesViewController is unwinded from another view controller.

Finally, let's ensure that the table view is always in sync with our `notes` property.

> [action]
Update the `notes` property as follows:
>
    var notes: [Note] = [] {
    	didSet {
    		tableView.reloadData()
    	}
    }

Remember the `didSet` from before?  As a reminder, it's a kind of *property observer* and it allows us to execute some code whenever the property its declared under changes. In this case, every time the `notes` property changes, we tell the table view to reload its data.

##Deleting Notes

Remember that when we implemented the ability to delete notes, we did so in the
`tableView(_:commitEditingStyle:forRowAtIndexPath:)` method.

We need to update the above method to delete the note from CoreData as well.

> [action]
Update the `tableView(tableView:commitEditingStyle:forRowAtIndexPath:)` method as follows:
>
    override func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCellEditingStyle, forRowAt indexPath: IndexPath) {
    	if editingStyle == .delete {
	  	 	//1
    		CoreDataHelper.deleteNote(notes[(indexPath as NSIndexPath).row])
    		//2
    		notes = CoreDataHelper.retrieveNotes()
    	}
    }

1. Here we delete the note from CoreData using the helper method we defined earlier. Also like before, we use the `indexPath.row` to index into `notes` to delete the correct one.

2. Here we update the `notes` property to reflect the changes.

#Updating the DisplayNoteViewController

Remember that we both create new notes and modify existing notes in the DisplayNoteViewController. Now that we are using CoreData, we must update our code to add and update notes to CoreData as well.

> [action]
Update `prepareForSegue(_:sender:)` as follows:
>
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        if segue.identifier == "Save" {
            // if note exists, update title and content
            if let note = note {
                // 1
                note.title = noteTitleTextField.text ?? ""
                note.content = noteContentTextView.text ?? ""
								note.modificationTime = Date()
                CoreDataHelper.saveNote()
            } else {
                // if note does not exist, create new note
                let note = Note()
                note.title = noteTitleTextField.text ?? ""
                note.content = noteContentTextView.text ?? ""
                note.modificationTime = Date()
                // 2
                CoreDataHelper.addNote(note)
            }
        }
    }

For the most, the above code is identical to what we had before, except for 3 things:

1. We change the note object and save the changed object.

2. Here we use our CoreData helper to add the newly created note to CoreData, so that it's persisted.

#Running the App

Congratulations -- you have just built a fully functioning note taking app! Run it and test it out! You can even try force quitting Make School Notes, then reopening it to see that beautiful persistance. To force quit, press the home button twice (**shift+command+h**, ⇧⌘H in simulator) and swipe up on Make School Notes. You should see all the notes you saved when it re-opens!

>[info]
>###On this page, you should have:
>
>1. Updated ListNotesViewController's `viewDidLoad()` to grab all the notes from CoreData.
>2. Added a `didSet` property listener on the `notes` property so that it reloads the table view every time the property is updated
>3. Updated ListNotesViewController's `tableView(tableView:commitEditingStyle:forRowAtIndexPath:)` method to delete the notes from CoreData when they're deleted from the table view.
>4. Updated DisplayNoteViewController's `prepareForSegue(_:sender:)` method to save or update the notes in CoreData, depending on what the user does. Also it now updates the `notes` property in the ListNotesViewController to make sure it's fully up to date.
>5. Run and tested your fully functioning app!
