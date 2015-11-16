
Now that we have our table view setup correctly, let's add note taking functionality!

# Removing the Default Text

Currently, when we tap the **+** button and segue to the Display Note View Controller, we are greeted with this screen:

![image of Display Note View Controller with lorem ipsum text](./images/lorem.png)

Instead, we would like to see an empty text view so that our users can immediately begin taking notes.

> [action]
1. Create an IBOutlet from the text view to the Display Note View Controller with the name "noteContentTextView".
2. Create an IBOutlet for the text field as well, name it "noteContentTextField".
3. Add the following method:
>
        override func viewWillAppear() {
          // 1
          noteTitleTextField.text = ""
          noteContentTextView.text = ""
        }


So what is going on with this whole viewWillAppear() thing?

Every time a view controller is about to be displayed on screen, the operating system will call the `viewWillAppear()` method. This gives us the opportunity to execute some view controller specific code before our user's see the view controller. In the code above, we used that opportunity to remove the "Lorem ipsum..." text from the Display Note View Controller.

# Saving the Note

In Make School Notes, users can create new notes by tapping the **+** button in the List Notes View Controller. Recall that once the **+** button is tapped, we segue to the Display Note View Controller to allow users to compose their new notes. When a user is finished creating their new note and would like to save it, we need a way to store the new note into the `notes` array (where all of a user's current notes are saved) in the List Notes View Controller. To solve this problem we need a way to pass data (the new note) from one view controller (the Display Note View Controller) to another view controller (the List Notes View Controller).

To enable this functionality we are going to have to do three three:

1. Determine when a user taps the **Cancel** or **Save** button in the Display Note View Controller by adding *identifiers* to our segues.
2. If the user tapped **Save**, pass the new note back to the List Notes View Controller using the `prepareForSegue()` method.
3. Add the saved note to the `notes` array in the List Notes View Controller using the `unwindToListNotesViewController()` unwind segue that we created earlier..

## Adding Segue Identifiers







## Capturing the Text

> [action]
Add the following method to the Display Note View Controller class:
>
        override func viewWillAppear() {
          // 1
          noteTitleTextField.text = ""
          noteContentTextView.text = ""
        }




<!-- Remember that the table view is using the `notes` property of the List Notes View Controller to populate its cells. Whenever we create a new note, we will need to add it to the `notes` array for it to be displayed in the table view. Recall that when a user wants to add a new note, they will have to tap the **+** button in the List Notes View Controller, which will subsequently segue to the Display Note View Controller. Now the user will create the new note in the Display Note View Controller, but we need to add the note to the `notes` array in the List Notes View Controller, so what do we do? -->
