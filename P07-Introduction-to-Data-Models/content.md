---
title: "Introduction to Data Models"
slug: intro-data-models
---

Now that we have finished building our app's interface, let's focus on building the logic. In this tutorial we will define a *data model* and demonstrate how to use it in our app.


Oftentimes when programming, we will need a way to represent an object that can be found in the physical world - like a person, city, or animal - we can represent these things in our programs by defining data models. In the physical world, these objects have different characteristics (a person has a name, birthdate, and height) and actions (a person can walk, run, and jump). In our programs, we can represent a physical world object as a class. The characteristics of the physical world object correspond to the properties of the class and the actions of the physical world object correspond to the methods of the class. For instance, if we wanted to define a data model for a person, we could do something like this:

    class Person {

      // properties (characteristics)
      var name = ""
      let birthdate = NSDate()
      var height = 0

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

Before we define our note data model, we need to first create a `Note.swift` file. In the Project navigator notice that there is an empty folder called *Models*. This folder was included in the starter project and will store all of our data models. (In Make School Notes we only have one data model, but in larger projects you could have many.)

We can add new source code files to our project as follows:

1. Select the folder where you want the new file to be created.
2. Press `command-n` or select `File > New > File` from the Xcode menu options at the top of the screen.
3. Select `iOS > Source > Swift File`.
4. Specify the file name and click *Create*.

> [action]
Add a new file called *Note* to the *Models* folder:
>
<video width="100%" controls>
    <source src="https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/addFile.mov" type="video/mp4">
</video>

#The Note Data Model

Because our List Notes Table View Controller displays the title and modification time of our notes and the Display Note View Controller displays a note's title and content, our note data model will need to have properties for *title*, *content*, and *modification time*.

> [action]
Define the Note class as follows inside `Note.swift`:
>
    class Note {
      var title = ""
      var content = ""
      var modificationTime = NSDate()
    }

#Using our Note Data Model

Prior to this point, the table view in our List Notes Table View Controller had been using hard coded values to populate its cells; however, we would like the table view to populate its cells based on a user's notes. This means that the List Notes Table View Controller is going to need to store all of a user's current notes.

> [action]
> Select the `ListNotesTableViewController.swift` file and add the following just below the declaration of the ListNotesTableViewController class:
>
    var notes = [Note]()

The line above is creating a variable called `notes` that has been initialized to an empty array of *Note* objects. When a user creates a new note, we will add an element to our `notes` array, and when a user deletes a note, we will delete the corresponding note in our `notes` array.

Now when we populate the cells of the table view in the List Notes Table View Controller, we will use the note objects in our `notes` array, instead of the hard coded values!

Let's update our code to reflect this change.

> [action]
Modify `func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int` as follows:
>
    override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
      return notes.count
    }

Remember that the above method is used by the table view to determine its number of cells. In our previous code, we hard coded the return value to 10, but now we are returning the number of notes that are in the `notes` array.

> [action]
Modify `func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell` as follows:
>
    override func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
      let cell = tableView.dequeueReusableCellWithIdentifier("listNotesTableViewCell", forIndexPath: indexPath) as! ListNotesTableViewCell
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

Remember that this method is called for each individual cell in the table view and is used to populate the cell with the appropriate data.

1. We are getting the row property from the indexPath argument which was passed into the method.
2. We are using the row to index into our `notes` array to get the appropriate data.
3. We are setting the title of the cell to be the title of the note.
4. We are converting the modificationTime of the note (which is of type `NSDate`) to a `String` using a method that was included in the starter project. We are then setting the modification time of the cell to be the modification time of the note.

# Running the App!

At this point, your app should look something like this:

<video width="100%" controls>
    <source src="https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/P07-complete.mov" type="video/mp4">
</video>

Remember that our table view is now populating its cells based on a user's notes. Currently, we don't have any notes, so our table view has nothing to display!

In the next tutorial, let's fix this by building the functionality that will allow us to add new notes.
