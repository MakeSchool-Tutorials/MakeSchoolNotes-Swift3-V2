---
title: "Displaying Notes"
slug: displaying-notes
---

In this section we will make it so users can modify existing notes. Remember that the user's notes are stored in the `notes` array in the List Notes Table View Controller, but any modifications they would make will happen in the Display Note View Controller. When a user taps a cell in the List Notes Table View Controller, we must pass the corresponding note to the Display Note View Controller so that it can be displayed and modified.

Currently, our Display Note View Controller doesn't contain any property to hold a note, so let's add one now.

> [action]
Add the following near the top of the Display Note View Controller class to create a `note` property:
>
    var note: Note?

**Why did we declare the `note` as an `Note?` (optional Note) and not just `Note`?**

> [solution]
The `note` must be of type `Note?` because it could either contain a value (in the case where an existing note is being modified) or it could be `nil` (in the case where we are creating a new note).

#Determining the Selected Note

When creating a new note, we used the `prepareForSegue()` method to pass a *newly created* note from the Display Note View Controller to the List Notes Table View Controller. When modifying a note, we will take a similar approach, except we will pass an *existing* note from the List Notes Table View Controller to the Display Note View Controller.

> [action]
Add the following to the `prepareForSegue(_:sender:)` method in the ListNotesTableViewController class:
>
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
      if let identifier = segue.identifier {
        if identifier == "displayNote" {
          print("Table view cell tapped")
>          
          // 1
          let indexPath = tableView.indexPathForSelectedRow!
          // 2
          let note = notes[indexPath.row]
          // 3
          let displayNoteViewController = segue.destination as! DisplayNoteViewController
          // 4
          displayNoteViewController.note = note
>          
        } else if identifier == "addNote" {
          print("+ button tapped")
        }
      }
    }

What's going on here?

1. Every table view has a property called `indexPathForSelectedRow`. When a user has selected a cell from a table view, we can use this method to access that particular cell's index path. Remember, an index path has `section` and `row` properties, which we can use to associate the selected table view cell with the data model backing it, in this case the `notes` array.

	> [info]
	> Notice that we are forcefully unwrapping the `indexPathForSelectedRow` property. We have to be sure that `indexPathForSelectedRow` is not nil, otherwise this will cause a crash. In this case, we are sure that this is safe because we will only execute this code when the segue identifier is set to *displayNote*, which can only happen when a user has just selected a cell from the table view.

2. Because our table view only has one section, we can uniquely identify each cell only using the `row` property of its corresponding index path. We use `indexPath.row` to retrieve the note from the `notes` array that corresponds to the touched cell.

3. We are get access to the Display Note View Controller using the segue's `destination` property. Notice we downcast it, just like we did before for the segue from the Display Note View Controller.

4. We are setting the `note` property of the Display Note View Controller to the note corresponding to the cell that the user tapped.

#Displaying the Note

Now that the note is being passed to the Display Note View Controller, we must display its title and contents.

Recall that the `viewWillAppear()` method is called immediately before the view appears on screen; this is a great place for us to use the note's title and content to populate the text field and text view in the Display Note View Controller.

> [action]
Modify the `viewWillAppear()` method in the *DisplayNoteViewController* class as follows:
>
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        // 1
        if let note = note {
            // 2
            noteTitleTextField.text = note.title
            noteContentTextView.text = note.content
        } else {
            // 3
            noteTitleTextField.text = ""
            noteContentTextView.text = ""
        }
    }

Because the Display Note View Controller is used to both create new notes and modify existing notes, we must check whether the `note` property is `nil` (which would indicate that we are creating a new note) or if it contains a value (which would indicate that we are modifying an existing note) to determine the appropriate action.

1. We are using the optional binding technique to unwrap the value in the `note` property and storing the actual value (if it exists) in a local variable named `note`.

2. This code will only execute if the `note` property was not `nil`. We are setting the text field and text view properties to the note's title and content, respectively.

3.  This code is executed if the `note` property was `nil`. This happens if we are creating a new note, so we set the text field and text view to empty strings to ensure that our users can immediately begin typing their new note.

#Updating the Table View

We've done it! You can now add new notes and modify existing notes. However, there is a small problem: when saving modifications to existing notes, they are added to the `notes` array as new notes! Try it out and see for yourself.

To solve this problem, we are going to need to check wether we are creating a new note or modifying an existing note. We can do this the same way we did in the `viewWillAppear()` method. When the user taps the **Save** button, if we are creating a new note, then we need to add it to the `notes` array, but if we are modifying an existing note, we only need to update the title and content of the note.

> [action]
Update `prepareForSegue(_:sender:)` in the *DisplayNoteViewController* class as follows:
>
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        let listNotesTableViewController = segue.destination as! ListNotesTableViewController
        if segue.identifier == "Save" {
            if let note = note {
                // 1
                note.title = noteTitleTextField.text ?? ""
                note.content = noteContentTextView.text ?? ""
                // 2
                listNotesTableViewController.tableView.reloadData()
            } else {
                // 3
                let newNote = Note()
                newNote.title = noteTitleTextField.text ?? ""
                newNote.content = noteContentTextView.text ?? ""
                newNote.modificationTime = Date()
                listNotesTableViewController.notes.append(newNote)
            }
        }
    }

Notice that we are once again using the `note` property to take different actions depending on whether we are creating a new note or modifying an existing note.

1. We only reach this code if the `note` property contains a value (indicating that we are modifying an existing note) so we only need to update the title and content of the note. (Notice that we do not have to pass the modified note back to the table view because class types are *passed by reference* in Swift.)

2. Here we tell the table view to reload its data, so that any edits to an existing note we've made will be reflected in the notes list.

3. This code is executed if the `note` property was `nil` (indicating that we are creating a new note) so we create the new note and add it to the `notes` array. Notice we don't have to tell the ListNotesTableViewController to refresh in this case, because our `didSet` property observer takes care of that for us!

#Running the App!

Congratulations! We now have a note taking app that can add new notes and edit existing notes! Run it and try it out!

![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/P09-complete.mov)

...But what if we no longer need a note? In the next section, let's add the functionality that allows users to delete notes.

>[info]
>###On this page, you should have:
>
>1. Added the `note` property to the Display Note View Controller, so it can be passed a note to be modified.
>2. Added code to the `prepareForSegue(_:sender:)` method of the List Notes Table View Controller so that when a cell is selected, the appropriate note is sent to the Display Note View Controller so it can be edited.
>3. Modified the `viewWillAppear()` method in the Display Note View Controller to display the contents note passed to it when a note is being edited.
>4. Updated the `prepareForSegue(_:sender:)` method of the Display Note View Controller to ensure we only add a note to the List Notes Table View Controller if the note is a new one.
>5. Run the app to play with it!
