
Now that we have finished building our app's interface, let's focus on building the logic. In this tutorial we will define a *data model* and demonstrate how to use it in our app.

## The Note Data Model

Oftentimes when programming, we will need a way to represent an object that can be found in the real world - like a person or city or animal - we can represent these things in our programs by defining a data model. In the real world these objects have different characteristics (a person has a name, birthdate, and height) and valid actions (a person can walk, run, and jump). In our programs, we can represent a real world object as a class. The characteristics of the real world object correspond to the properties of the class and the valid actions of the real world object correspond to the methods of the class.

In **Make School Notes**, we need to create a data model that represents a note, thus we need to create a custom Note class.  

What properties should we include in our custom Note class?

> [answer]
Because our List Notes View Controller displays the title and modification time of our notes and the Display Note View Controller displays a note's title and content, our note data model will need to have *title*, *content*, and *modification time* properties.

Let's create our Note data model now.

## Adding New Files to Our Project

In the Project navigator notice that there is an empty folder called *Models*. This folder was included in the starter project and will store all of our data models. (In **Make School Notes** we only have one data model, but in larger projects you could have many.)

We can add new source code files to our project as follows:

1. Select the folder where you want the new file to be created.
2. Press `command-n` or select `File > New > File` from the Xcode menu options at the top of the screen.
3. Select `iOS > Source > Swift File`.
4. Specify the file name and click *Create*.

Now, let's add a new file for our Note model.

> [action]
Add a new file called *Note* to the *Models* folder:
>
![BROKEN LINK - play video/addFile.mov](video destination)

Great! Now let's define our Note class.

> [action]
Add the following to the `Note.swift` file:
>
    class Note {
      var title: String = ""
      var content: String = ""
      var modificationTime: NSDate = NSDate()
    }

In the above code we are creating a new class called *Note* which has three properties: title, content, and modificationTime.

## Using our Note Data Model

Prior to this point, the table view in our List Notes View Controller has been using hard coded values to populate its cells; however, we would like the table view to populate its cells based on a user's notes. This means that the List Notes View Controller is going to need to store all of a user's current notes.

> [action]
Select the `ListNotesViewController.swift` file and add the following just below the declaration of the ListNotesViewController class:
>
    var notes = [Note]()

The line above is creating a variable called `notes` that has been initialized to an empty array of *Note* objects. When a user creates a new note, we will add an element to our `notes` array, and when a user deletes a note, we will delete the corresponding note in our `notes` array.

Now that we have a storage mechanism for all of a user's notes, let's modify the code which is populating our table view.

> [action]
Modify `func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int` as follows:
>
    func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
      return notes.count
    }

Remember that the above method is used by the table view to determine its number of cells. In our previous code, we hard coded the return value to 10, but now we are returning the number of notes that are in the `notes` array.

> [action]
Modify `func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell` as follows:
>
    func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
      let cell = tableView.dequeueReusableCellWithIdentifier("noteTableViewCell", forIndexPath: indexPath) as! NoteTableViewCell
>
      // 1
      let row = indexPath.row
>
      // 2
      let note = notes[row]
>
      // 3
      cell.titleLabel.text = note.title
      cell.modificationTimeLabel.text = note.modificationTime.convertToString()
>      
      return cell
    }

Remember that this method is called for each individual cell in the table view and is used to populate the cell with the appropriate data.

1. We are getting the row property from the indexPath argument which was passed into the method.
2. We are using the row to index into our `notes` array to get the appropriate data.
3. We are setting the cell's title label and modification time label to the title and modification time of the note respectively.
