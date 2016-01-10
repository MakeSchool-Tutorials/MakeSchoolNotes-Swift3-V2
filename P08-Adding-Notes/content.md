---
title: "Adding Notes"
slug: adding-notes
---

Now that we have our table view setup correctly, let's add note taking functionality!

#Removing the Default Text

Currently, when we tap the **+** button and segue to the Display Note View Controller, we are greeted with this screen:

![image of Display Note View Controller with lorem ipsum text](./images/lorem.png)

Instead, we would like to see an empty text view so that our users can immediately begin taking notes.

> [action]
1. Create an IBOutlet from the text view to the Display Note View Controller with the name "noteContentTextView".
2. Create an IBOutlet for the text field as well, name it "noteContentTextField".
3. Add the following method:
>
        override func viewWillAppear(animated: Bool) {
          super.viewWillAppear(animated)
          // 1
          noteTitleTextField.text = ""
          noteContentTextView.text = ""
        }


So what is going on with this whole `viewWillAppear(animated: Bool)` thing?

Every time a view controller is about to be displayed on screen, the operating system will call the `viewWillAppear()` method. This gives us the opportunity to execute some view controller specific code before our user's see the view controller. In the code above, we used that opportunity to remove the "Lorem ipsum..." text from the Display Note View Controller.

#Creating the Note

Great! We are finally ready to create our first note!

Currently, our Display Note View Controller doesn't contain a property which can store a note, let's add one now.

> [action]
Add the following near the top of the Display Note View Controller class to create a `note` property:
>
    var note: Note?

**Why did we declare the `note` as an `Optional Note` and note just `Note`?**

> [solution]
The `note` must be of type `Optional Note` because it could either contain a value (in the case where an existing note is being modified) or it could be `nil` (in the case where we are creating a new note).

We can now use the text from the text field and text view to create a new note and store it in the `note` property.

> [action]
Update the `prepareForSegue()` method in the Display Note View Controller class to the following:
>
    override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
      if let identifier = segue.identifier {
        if identifier == "Cancel" {
          print("Cancel button tapped")
        } else if identifier == "Save" {
          print("Save button tapped")
>
          // 1
          let note = Note()
          // 2
          note.title = noteTitleTextField.text ?? ""
          note.content = noteContentTextView.text
          // 3
          note.modificationTime = NSDate()
        }
      }
    }

So what did we change in the code above?

1. We are creating a new instance of the Note class because we want to create a new note.

2. We are setting the note's title and content to the text contained in the noteTitleTextField and noteContentTextView respectively. Recall that the `??` is the nil coalescing operator and provides a default value if the object is `nil`.

3. We are setting the note's modification time to the current time.

Notice that the code we added is inside of the `else if identifier == "Save"` block and will only be executed if the user taps the **Save** button.

#Saving the Note

When a user is finished creating their note, we need a way to store the new note in the `notes` array in the List Notes Table View Controller. Whenever we need to pass data between view controllers, we use segues (or unwind segues)!

> [action]
Update the `prepareForSegue()` method in the Display Note View Controller class to the following:
>
    override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {
      if let identifier = segue.identifier {
        if identifier == "Cancel" {
          print("Cancel button tapped")
        } else if identifier == "Save" {
          print("Save button tapped")
>
          let note = Note()
          note.title = noteTitleTextField.text ?? ""
          note.content = noteContentTextView.text
          note.modificationTime = NSDate()
>
          // 1
          let destinationViewController = segue.destinationViewController as! ListNotesTableViewController
          // 2
          destinationViewController.notes.append(note)
          // 3
          destinationViewController.tableView.reloadData()
        }
      }
    }

The above code is identical to the previous implementation of `prepareForSegue()` except that we are now adding the new note to the `notes` array in List Notes Table View Controller.

1. Since a segue is like a link between two view controllers, you can access both the source and destination view controllers of a segue using the `sourceViewController` and `destinationViewController` properties. Notice that we must forcefully cast the property to the correct type. Since we are segueing to the List Notes Table View Controller, the `destinationViewController` property has type `ListNotesViewController`. (The `sourceViewController` property would have type `DisplayNoteViewController`.)

2. We are adding the new note to the `notes` array.

3. Remember that the table view is using the `notes` array to determine how many cells it has and the data to be displayed in each cell. After adding the new note to our `notes` array, the table view becomes outdated because it no longer displays all of a user's notes. Specifically, the newly added note is not displayed. We can force the table view to update by calling the `reloadData()` method.

#Running the App!

<video width="100%" controls>
    <source src="https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/P08-complete.mov" type="video/mp4">
</video>

Great! Now we can add new notes to our app, but what happens if we wanted to modify one of our notes? If we were to tap a note in the table view, we would successfully segue to the Display Note View Controller, but the note's title and content would not be display! Let's fix that in the next tutorial. =]

(Also, our current app doesn't persist note data between app launches. This means that if you create a new note and then relaunch your app, your note will be gone! But do not worry, we will fix this problem in a later tutorial!)
