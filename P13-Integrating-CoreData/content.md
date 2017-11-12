---
title: "Integrating CoreData"
slug: integrating-CoreData
---

We're at the beginning of the end. It's been quite a journey. In this section, we'll finish our _Notes_ app functionality by implementing persistence with our `CoreDataHelper`.

# Retrieving Notes

When a user first opens our app, we want to retrieve and display all of the user's existing notes. We can do this by retrieve our existing notes and updating our `notes` array in `ListNotesTableViewController` at app launch.

Let's go ahead and implement this with our `CoreDataHelper`!

> [action]
In `ListNotesTableViewController`, update the method `viewDidLoad()` to retrieve the user's previously existing notes:
>
```
override func viewDidLoad() {
    super.viewDidLoad()
>    
    notes = CoreDataHelper.retrieveNotes()
}
```

The `viewDidLoad()` of our `ListNotesTableViewController` happens roughly when the app first launches. We can use our `CoreDataHelper` to retrieve the user's previously existing notes and populate our `notes` array.

Next, we'll also update our _unwind segue_ to retrieve our updated notes.

> [action]
In `ListNotesTableViewController`, update the _unwind segue_ method `unwindWithSegue(_:)` to the following:
>
```
@IBAction func unwindWithSegue(_ segue: UIStoryboardSegue) {
    notes = CoreDataHelper.retrieveNotes()
}
```

Now, each time the user taps the save or cancel bar button item in `DisplayNoteViewController`, we update our `notes` array in `ListNotesTableViewController`. Our `didSet` property observer will take care of reloading our table view. In combination, we can be sure our table view data is always synced with our `NSManagedObjectContext`.

# Deleting Notes

We'll also need to update our delete note functionality.

> [action]
In `ListNotesTableViewController`, update the table view data source method `tableView(_:commitEditingStyle:forRowAtIndexPath:)` to use our _Core Data_ helper:
>
```
override func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCellEditingStyle, forRowAt indexPath: IndexPath) {
    if editingStyle == .delete {
        let noteToDelete = notes[indexPath.row]
        CoreDataHelper.delete(note: noteToDelete)
>
        notes = CoreDataHelper.retrieveNotes()
    }
}
```
>
In the code above, we retrieve the note to be deleted corresponding to the selected index path. Then we use our _Core Data_ helper to delete the selected note. Last we update our `notes` array to reflect the changes in our `NSManagedObjectContext`.

# Updating DisplayNoteViewController

Last, we'll need to update our functionality in `DisplayNoteViewController` for creating new notes or modifying existing ones. Once again, let's use our `CoreDataHelper` to update our code.

> [action]
In `DisplayNoteViewController`, update `prepare(for:sender:)` to use our `CoreDataHelper`:
>
```
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    guard let identifier = segue.identifier else { return }
>
    switch identifier {
    case "save" where note != nil:
        note?.title = titleTextField.text ?? ""
        note?.content = contentTextView.text ?? ""
        note?.modificationTime = Date()
>
        CoreDataHelper.saveNote()
>
    case "save" where note == nil:
        let note = CoreDataHelper.newNote()
        note.title = titleTextField.text ?? ""
        note.content = contentTextView.text ?? ""
        note.modificationTime = Date()
>
        CoreDataHelper.saveNote()
>
    case "cancel":
        print("cancel bar button item tapped")
>
    default:
        print("unexpected segue identifier")
    }
}
```
>
We update our `prepare(for:sender:)` method to make use of our `CoreDataHelper`. Since we're retrieving notes in our `ListNotesTableViewController` _unwind segue_, we no longer need to reference our segue's destination controller.

# Running the App

Wooooooooo! We're done. Phew... I'm just as happy as you are. Congrats, you've just finishing building a fully functioning _Notes_ app with _Core Data_.

Let your hair down, take it for a test run.

You can write notes in a box. <br />
You can write notes with a fox. <br />
You can write notes in a house. <br />
You can write notes with a mouse. <br />

You can write notes here or there. <br />
You can write notes anywhere. <br />

Notes, you can now write.
