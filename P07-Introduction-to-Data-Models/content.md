---
title: "Introduction to Data Models"
slug: intro-data-models
---

Now that we have finished building our app's interface, let's focus on building the logic. In this tutorial we will define a *data model* and demonstrate how to use it in our app.

Oftentimes when programming, we will need a way to represent an object that can be found in the physical world - like a person, city, or animal - we can represent these things by defining data models. In the physical world, these objects have different characteristics (a person has a name, birthdate, and height) and actions (a person can walk, run, and jump). In our programs, we can represent a physical world object as a class. The characteristics of the physical world object correspond to the properties of the class and the actions of the physical world object correspond to the methods of the class. For instance, if we wanted to define a data model for a person, we could do something like this:

    class Person {

      // properties (characteristics)
      var name = "Daniel"
      let birthdate = NSDate()
      var height = 74

      // methods (actions)
      func walk() {
        ...
      }

      func run() {
        ...
      }

      func jump() {
        ...
      }

    }

#Adding New Files to Our Project

Before we define our note data model, we need to first create a `Note.swift` file. If you look in the Project navigator, you'll notice that there is an empty group called *Models*. This group was included in the starter project and will store all of our data models. In Make School Notes we only have one data model, but in larger projects you could have many.

You can add new source code files to our project as follows:

1. Select the group where you want the new file to be created.
2. Press **command-n** or select **File > New > File** from the Xcode menu at the top of the screen.
3. Select **iOS > Source > Swift File**.
4. Specify the file name, browse to where you want to save the file, and click *Create*.

> [action]
Add a new file called *Note* to the *Models* group, making sure to save it in your starter project's `MakeSchoolNotes/Models` directory.
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/addFile.mov)

#The Note Data Model

Because our List Notes Table View Controller displays the title and modification time of the notes, and the Display Note View Controller displays a note's title and content, our note data model will need to have properties for *title*, *content*, and *modification time*.

> [action]
Define the Note class as follows inside `Note.swift`:
>
    class Note {
      var title = ""
      var content = ""
      var modificationTime = Date()
    }

#Using our Note Data Model

The table view in our List Notes Table View Controller is currently using hard-coded values to populate its cells; however, we would like the table view to populate its cells based on the user's notes. This means that the List Notes Table View Controller is going to need to know about all of a user's current notes.

> [action]
> Select the `ListNotesTableViewController.swift` file and add the following just below the declaration of the ListNotesTableViewController class:
>
    var notes = [Note]()

The line above is creating a property called `notes` that has been initialized to an empty array of *Note* objects. When a user creates a new note, we will add a `Note` to our `notes` array, and when a user deletes a note, we will delete the corresponding note in our `notes` array.

Now when we populate the cells of the table view in the List Notes Table View Controller, we will use the note objects in our `notes` array, instead of the hard-coded values!

Let's update our code to reflect this change.

> [action]
Modify `func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int` as follows:
>
    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
      return notes.count
    }

Remember that the above method is used by the table view to determine its number of cells. In our previous code, we hard coded the return value to 10, but now we are returning the number of notes that are in the `notes` array.

> [action]
Modify `func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell` as follows:
>
    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
      let cell = tableView.dequeueReusableCell(withIdentifier: "listNotesTableViewCell", for: indexPath) as! ListNotesTableViewCell
>
      // 1
      let row = indexPath.row
>
      // 2
      let note = notes[row]
>
      // 3
      cell.noteTitleLabel.text = note.title
>
      // 4
      cell.noteModificationTimeLabel.text = note.modificationTime.convertToString()
>      
      return cell
    }

Remember that the table view calls this method for each row in the table view, asking for a cell to populate the row with.

1. The `indexPath` is an argument that was passed into `cellForRowAtIndexPath` and is how the table view tells us what row it wants a cell for. We access the `row` property of index path to figure out which row.
2. Here we use the row to index into our `notes` array to get the appropriate note object.
3. We set the text of the `noteTitleLabel` in the cell to be the title of the note.
4. We are converting the `modificationTime` of the note (which is of type `NSDate`) to a `String` using a method that was included in the starter project. We are then setting the `text` property of the `noteModificationTimeLabel`'s to be the modification time of the note.

#Running the App!

At this point, your app should look something like this:

![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/P07-complete.mov)

Remember that our table view is now populating its cells based on the user's notes. Currently, we don't have any notes, so our table view has nothing to display!

In the next tutorial, we will fix this by building the functionality that will allow us to add new notes.


>[info]
>###On this page, you should have:
>
>1. Learned the difference between a `property` and `method` in a class.
>2. Created the `Note.swift` file and added it to your *Models* directory.
>3. Created the `Note` class and added the `title`, `content` and `modificationTime` properties.
>4. Added the `notes` property to the List Notes Table View Controller. `notes` is an array of the user's notes.
>5. Updated `tableView(_:numberOfRowsInSection:)` and `tableView(_:cellForRowAtIndexPath:indexPath)`to now work with the `notes` array.
