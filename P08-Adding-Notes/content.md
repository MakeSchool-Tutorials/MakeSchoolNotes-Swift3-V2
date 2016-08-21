---
title: "Adding Notes"
slug: adding-notes
---

Now that we have the table view set up correctly, let's add some note taking functionality!

##Removing the Default Text

Currently, when we tap the **+** button and segue to the Display Note View Controller, we are greeted with this screen:

![image of Display Note View Controller with lorem ipsum text](./images/lorem.png)

Instead, it would be better to show an empty text view so that our users can begin taking notes.

> [action]
1. Create an IBOutlet from the text view to the Display Note View Controller with the name "noteContentTextView".
2. Create an IBOutlet for the text field as well, name it "noteTitleTextField".
3. Add the following method to the Display Note View Controller:
>
	override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        // 1
        noteTitleTextField.text = ""
        noteContentTextView.text = ""
    }


So what's going on with this whole `viewWillAppear(animated: Bool)` thing?

Every time a view controller is about to be displayed on screen, the operating system will call the `viewWillAppear()` method. This gives us the opportunity to execute some view controller specific code before the user sees the view controller. In this case, we used that opportunity to remove the "Lorem ipsum..." placeholder text from the Display Note View Controller.

#Creating the Note

Great! We are finally ready to create our first note!

> [action]
Update the `prepareForSegue(_:sender:)` method in the Display Note View Controller class to the following:
>
	override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
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
                note.modificationTime = Date()
            }
        }
    }

So what did we change in the code above?

1. When the save button is pressed, we need to store the text from the `noteTitleTextField` and `noteContentTextView`. What better place to put it, than in our data model, the `Note` class? So first, we create an instance of `Note`.

2. We are setting the note's title and content to the text contained in the `noteTitleTextField` and `noteContentTextView` respectively. Recall that the `??` is the *nil coalescing operator* and provides a default value if the object is `nil`.

3. We are setting the note's modification time to the current time.

Again, notice that the code we added is inside of the `else if identifier == "Save"` block, and will only be executed if the user taps the **Save** button.

#Saving the Note

Once the user is finished creating their note, we need a way to store the new note in the `notes` array in the List Notes Table View Controller, so we can list it with their other notes. But currently the note only exists in the Display Note View Controller. How can we get the note from there to the List Notes Table View Controller? The answer is segues!

> [action]
Add the lines marked `1` `2` and `3` to the `prepareForSegue(_:sender:)` method in the Display Note View Controller class:
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
                let listNotesTableViewController = segue.destination as! ListNotesTableViewController
                // 2
                listNotesTableViewController.notes.append(note)
            }
        }
    }

We are now adding the new note to the `notes` array in List Notes Table View Controller. Read on to find out how:

1. Remember that a segue is a link between two view controllers. The segue class, `UIStoryBoardSegue` provides access both the source and destination view controllers using the `sourceViewController` and `destinationViewController` properties. Here we're using the `destinatonViewController` property to get a reference to the `ListNotesTableViewController`. However, the `segue` doesn't know that the `destinationViewController` is a `ListNotesTableViewController`, it just knows is that it's some kind of `UIViewController`. But we *do* know that the `destinationViewController` is a `ListNotesViewController` so we tell the compiler that by downcasting with `as! ListNotesTableViewController`.

2. We are adding the new note to the `notes` array.

#Property Observers

This is all well and good, but we have a problem.

> [action]
> Try running the app. Create and a new note and save it. See any problems? Our table view is still blank!

We have to tell the table view to update itself when there are new notes.

> [action]
> Open `ListNotesTableViewController` and modify your `notes` property to look as follows:
>
```
var notes = [Note]() {
	didSet {
   		tableView.reloadData()
   	}
}
```

Look at that fancy syntax!  Ooh. We are using the `didSet` *property observer* to tell the table view to reload all of its data whenever our `notes` property is changed. A property observer is some code you can add to a property that will be triggered whenever the property changes. With `didSet` the code is triggered immediately following a property change. There's a second property observer is called `willSet`, which is triggered immediately before a property is about to change.

#Running the App!

![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/P08-complete.mov)

Great! Try running the app. Now we can add new notes, but what happens if we want to modify one of the notes? Try creating a new note. Then tap on it in the table view - we successfully segue to the Display Note View Controller, but the note's title and content doesn't yet display! Let's fix that in the next section.

Also we still have the issue that our current app doesn't persist note data between app launches. This means that if you create a new note, then force-quit and relaunch the app, your note will be gone! Not to worry, we will also fix this problem later.

>[info]
>###On this page, you should have:
>
>1. Linked the `noteTitleTextField` and `noteContentTextView` to the Display Note View Controller using IBOutlets.
>2. Implemented `viewWillAppear()` in the Display Note View Controller and used it to delete the placeholder text.
>3. Modified Display Note View Controller's `prepareForSegue(_:sender:)` method to create a new `Note()` with the user's input.
>4. Modified `prepareForSegue(_:sender:)` again to place the created note into the List Notes Table View Controller's array.
>5. Tried running the app to see what happens.
