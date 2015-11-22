
In this tutorial we will add the functionality that allows our users to modify existing notes. Remember that a user's notes are stored in the `notes` array in the List Notes View Controller and that all note modifications happen in the Display Note View Controller. When a user taps a cell in the table view in the List Notes View Controller, we must pass the corresponding note to the Display Note View Controller so that it can be displayed and modified.

# Determining the Selected Note

When creating a new note, we used the `prepareForSegue()` method to pass a *newly created* note from the Display Note View Controller to the List Notes View Controller. When modifying a note, we will take a similar approach, except we will pass an *existing* note from the List Notes View Controller to the Display Note View Controller.

> [action]
Add the following to the `prepareForSegue()` method in the ListNotesViewController class:
>
    override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
      if let identifier = segue.identifier {
        if identifier == "displayNote" {
          print("Table view cell tapped")
>          
          // 1
          let indexPath = tableView.indexPathForSelectedRow!
          // 2
          let note = notes[indexPath.row]
          // 3
          let destination = segue.destinationViewController as! DisplayNoteViewController
          // 4
          destination.note = note
>          
        } else if identifier == "addNote" {
          print("+ button tapped")
        }
      }
    }

1. Every table view has a property called `indexPathForSelectedRow`. When a user has selected a cell from a table view, we can use this method to access that particular cell's index path, which consists of a `section` and a `row`, and can be used to access the data corresponding to that particular cell. (Notice that we are forcefully unwrapping the `indexPathForSelectedRow` property. For our app, this is safe because we will only execute this code when the segue identifier is set to *displayNote* which indicates that a user has just selected a cell from the table view.)

2. Because our table view only has one section, we can uniquely identify each cell by using the `row` property of its corresponding index path, which we use to retrieve the corresponding note from the `notes` array.

3. We are getting access to the destination view controller, the Display Note View Controller in this case.

4. We are setting the `note` property of the Display Note View Controller to the note corresponding to the cell that the user tapped.

# Displaying the Note

Now that the note is being passed to the Display Note View Controller, we must display it's title and contents.

Recall that the `viewWillAppear()` method is called immediately before the view appears on screen; this is a great place for us to use the note's title and content to populate the text field and text view in the Display Note View Controller.

> [action]
Modify the `viewWillAppear()` method in the *DisplayNoteViewController* class as follows:
>
    override func viewWillAppear(animated: Bool) {
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

1. We are using the optional binding technique to unwrap the value in the `note` property and storing the actual value (if it exists) in a local variable named "note".

2. The `note` property contained a value and, because the value is of type `Note`, we are setting the text field and text view properties to the note's title and content, respectively.

3.  The `note` property was `nil` and we are creating a new note, so we set the text field and text view to empty strings to ensure that our users can immediately begin typing their new note.

# Updating the Table View

If you were to run your app now, you would be able to add new notes and modify existing notes; however, there is a small problem: when saving modifications to existing notes, they are added to the `notes` array as new notes! (Try it out and see for yourself.) Similar to how we checked in the `viewWillAppear()` method, when a user taps the **Save** button, we are going to need to check wether we are creating a new note or modifying an existing note. If we are creating a new note, then we need to add it to the `notes` array, but if we are modifying an existing note, we only need to update the title and content of the note.

> [action]
Update the `prepareForSegue()` in the *DisplayNoteViewController* class as follows:
>
    override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
      let destinationViewController = segue.destinationViewController as! ListNotesViewController
      if segue.identifier == "Save" {
>
        if let note = note {
          // 1
          note.title = noteTitleTextField.text ?? ""
          note.content = noteContentTextView.text ?? ""
        } else {
          // 2
          let note = Note()
          note.title = noteTitleTextField.text ?? ""
          note.content = noteContentTextView.text ?? ""
          note.modificationTime = NSDate()
          destinationViewController.notes.append(note)
        }
        // 3
        destinationViewController.tableView.reloadData()
      }
    }

Notice that we are once again using the `note` property to take different actions depending on whether we are creating a new note or modifying an existing note.

1. The `note` property contained a value (indicating that we are modifying an existing note) and we only need to update the title and content of the note. (Notice that we do not have to pass the modified note back to the table view because class types are *passed by reference* in Swift.)

2. The `note` property was `nil` (indicating that we are creating a new note) so we create the new note and add it to the `notes` array.

3. Because we changed the contents of the `notes` array (by either adding a new note or modifying an existing note), we must force the table view to reload its data. Notice that the call to the `reloadData()` method is outside of the entire optional binding block, this is because we want to reload the data regardless of whether we are creating a new note or modifying an existing note.

# Running the App!

Congratulations! We now have a note taking app that can add new notes and edit existing notes!



...But what if we no longer need a note? In the next tutorial, let's add the functionality that allows our users to delete notes.
