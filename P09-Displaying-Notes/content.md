---
title: "Displaying Notes"
slug: displaying-notes
---

In this section, we'll implement the functionality for users to view and edit existing notes.

First, we'll start by adding a `note` property to our `DisplayNoteViewController`. We'll need this to pass an existing note from the `ListNotesTableViewController` to the `DisplayNoteViewController` to be displayed.

> [action]
In `DisplayNoteViewController`, add the following property at the top of your view controller:
>
```
class DisplayNoteViewController: UIViewController {
>    
    // MARK: - Properties
>    
    var note: Note?
>
    // ...
}
```
>
We set our `note` to have a type of optional `Note`, or `Note?`, because it's value can potentially be `nil`. There are two cases for our `note` property:
>
1. When displaying or view an existing note, our `note` property will be a non-nil value.
1. When creating a new note, our `note` property will be `nil`.

# Passing Note Data Via Segues

Previously, we used `prepare(for:sender:)` to pass a newly created note from `DisplayNoteViewController` back to our `ListNotesTableViewController`.

To display or edit an existing note, we'll need to do the same thing in the opposite direction. We'll need to pass an existing note in the `ListNotesTableViewController` to our `DisplayNoteViewController`.

Let's implement this code now.

> [action]
In `ListNotesTableViewController`, update `prepare(for:sender:)` with the following code:
>
```
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    guard let identifier = segue.identifier else { return }
>
    switch identifier {
    case "displayNote":
        // 1
        guard let indexPath = tableView.indexPathForSelectedRow else { return }
>
        // 2
        let note = notes[indexPath.row]
        // 3
        let destination = segue.destination as! DisplayNoteViewController
        // 4
        destination.note = note
>
    case "addNote":
        print("create note bar button item tapped")
>
    default:
        print("unexpected segue identifier")
    }
}
```
>
Step-by-step:
>
1. Get a reference to the index path of the selected row in the table view using the `UITableView` property named `indexPathForSelectedRow`. We'll use this next to retrieve the correct note in our `notes` array.
1. Retrieve the selected note using the index path from the previous step.
1. Get a reference and type cast our segue's destination view controller. This will allow us to set the `note` property of the `DisplayNoteViewController` in the next step.
1. Using the reference to destination view controller, set the `note` property to the selected note.

# Displaying The Note

With our note data successfully passed between view controllers, we can display the existing note.

Previously in our `DisplayNoteViewController`, we added code in our `viewWillAppear()` to remove our storyboard's filler text. To display our existing note, we'll need to update this method to populate the text field and text view (if the note already exists!)

> [action]
In `DisplayNoteViewController`, update `viewWillAppear()` with the following code:
>
```
override func viewWillAppear(_ animated: Bool) {
    super.viewWillAppear(animated)
>
    // 1
    if let note = note {
        // 2
        titleTextField.text = note.title
        contentTextView.text = note.content
    } else {
        // 3
        titleTextField.text = ""
        contentTextView.text = ""
    }
}
```
>
In our `viewWillAppear()` method, we check for an existing note in the `note` property to determine whether the user is displaying an existing note or creating a new one.
>
Step-by-step:
>
1. Check the `note` property for an existing note using if-let optional binding.
1. If the `note` property is non-nil, then our `DisplayNoteViewController` populates the text field and text view with the content of the existing note.
1. If the `note` property is `nil`, then our `DisplayNoteViewController` clear the storyboard's lorem ipsum filler text so the user can quickly begin taking notes.

# Saving Edits

We've successfully implemented the code for displaying an existing note. However, when we try to edit a note, our code creates a new note, instead of editing the existing one.

Next, let's figure out how to save edits to existing notes.

Let's look at our `DisplayNoteViewController` logic in `prepare(for:sender:)` :

```
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    guard let identifier = segue.identifier else { return }

    switch identifier {
    case "save":
        let note = Note()
        note.title = titleTextField.text ?? ""
        note.content = contentTextView.text ?? ""
        note.modificationTime = Date()

        let destination = segue.destination as! ListNotesTableViewController
        destination.notes.append(note)

    case "cancel":
        print("cancel bar button item tapped")

    default:
        print("unexpected segue identifier")
    }
}
```

In the code above, tapping the save bar button item **always** creates a new note and appends it to the `notes` array of our `ListNotesTableViewController`.

Let's update our code to account for saving edits to existing notes. We can use the `note` property to determine whether we're saving a new or existing note.

> [action]
In `DisplayNoteViewController`, update `prepare(for:sender:)` to account for saving existing notes:
>
```
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    // 1
    guard let identifier = segue.identifier,
        let destination = segue.destination as? ListNotesTableViewController
        else { return }
>
    switch identifier {
    // 2
    case "save" where note != nil:
        note?.title = titleTextField.text ?? ""
        note?.content = contentTextView.text ?? ""
>
        destination.tableView.reloadData()
>
    // 3
    case "save" where note == nil:
        let note = Note()
        note.title = titleTextField.text ?? ""
        note.content = contentTextView.text ?? ""
        note.modificationTime = Date()
>
        // 4
        destination.notes.append(note)
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
We've added quite a bit of new code, let's break this down:
>
1. We add a if-let new clause to our guard statement to check that the destination view controller of our segue is of type `ListNotesTableViewController`. This allows us to safely type case our segue's destination view controller and access it for each case of our switch-statement.
1. We split our `save` case for our switch-statement into two. The first for if we're saving an existing note. We're able to know the note already exist because we check the `notes` property of our `DisplayNoteViewController` for a `non-nil` value. If the note exists, we update it's properties and reload the table view of our `ListNotesTableViewController`.
1. If the `note` property is `nil`, we know the user is creating a new note. In which case, we keep the previous code for creating a new note.
1. Notice that since we type cast our destination view controller in our guard-statement, we can use it directly without more downcasting.

<!-- break -->

> [info]
In the case of saving an existing note, we don't have to pass the modified note back to our `ListNotesTableViewController`. This is because in Swift, class types are passed by reference.
>
This means that both view controllers reference the same class instance. If the class instance is modified in one view controller, the other view controller will also access the same modified object.
>
We do, however, need to make sure the table view knows to reload it's cells.

## Running the App

In this section, we implemented the functionality to view and edit existing notes. Once again, let's make sure everything works!

> [action]
Build and run the app. Make sure you can create new notes and edit existing ones.
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p09_displaying_notes/editing_note_checkpoint.mp4)

In the next section, we'll look at how to implement the functionality for deleting existing notes.
