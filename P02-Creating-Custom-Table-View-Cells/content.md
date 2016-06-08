---
title: "Creating Custom Table Views Cells"
slug: custom-table-view-cells
---

Now that we have our table view up and running, in this step, let's customize the table view cells so that we can display the note's title and modification time.

#Resizing our Cell

To display the note's title and modification time, we are going to need two labels. To ensure we have enough room for both of our labels, let's make the cell a little bit bigger.

> [action]
Select the *listNotesTableViewCell* in the *Document Outline*, click the *Size inspector* icon, and change the *Row Height* to 60:
>
![image showing how to change the row height](./images/height.png)

Now let's change the hight of the table view to match.

> [action]
Select the *Table View* in the *Document Outline*, click the *Size inspector* icon, and change the row height to 60:

Notice that we had to change the row height for both our table view and our table view cell!


#Adding Labels to our Cell

<!-- ACTION: Remind them to set the constraints using auto layout before referring them back to the tutorial.  They should know how to do this at this point, but might need a quick reminder as to the steps involved.  A quick video of clicking "reset to suggested constraints "  -->
<!-- LATER ACTION: update link to Tip Calc  -->

Now that we have enough room, let's add our labels! (If you forgot how, revisit the [*Introduction to Interface Builder*](https://www.makeschool.com/tutorials/learn-how-to-build-your-very-first-ios-app/interface-builder) tutorial.)

> [action]
Add two labels to our *listNotesTableViewCell*. Your cell should look something like this when finished:
>
![image showing listNotesTableViewCell with two labels](./images/labels.png)


#Customizing our Label's Text

Let's customize the look of our labels by changing the text size, font, and color.

> [action] Select a label from the Document Outline, click the Attribute inspector icon, and experiment with changing the *Color* and *Font* fields. Your labels might look something like this afterwards:
>
![image showing listNotesTableViewCell with custom text](./images/custom.png)

Our cell is starting to come together, but we haven't finished yet!

#Connecting our Cell to Code

Because we want to be able to access our custom table view cell in code, we will need to set its custom class. We want to set the custom class of our table view cell (named *listNotesTableViewCell*) to the class named *ListNotesTableViewCell*. The *ListNotesTableViewCell* class was included in the starter project and can be found in the `ListNotesTableViewCell.swift` file within the *Views* folder in your Project navigator.

Notice the naming convention we are following: Our cell in Interface Builder is named "listNotesTableViewCell" (first letter is lowercase) and  we are connecting that cell to a class named "ListNotesTableViewCell" (first letter is uppercase). This convention is used heavily throughout iOS development.

> [action]
Select the *listNotesTableViewCell* in the *Document Outline*, click the *Identity inspector* icon, and set the *Class* field to "ListNotesTableViewCell":
>
![image showing listNotesTableViewCell's custom class](./images/custom-class.png)

Now that we have set the class of our *listNotesTableViewCell* to "ListNotesTableViewCell", let's take a look at the contents of the `ListNotesTableViewCell.swift` file.

> [action]
Click on the *Project navigator* icon, expand the *Views* folder, and select the `ListNotesTableViewCell.swift` file:
>
![image showing the NotesTableViewCell.swift file](./images/code.png)

Although we set our custom class to enable us to programmatically alter our table view cell, we currently do not have any way to reference the labels we created in Interface Builder from the *ListNotesTableViewCell* class. To access the labels from this class, we must setup another type of connection called an `IBOutlet`.

#Introducing IBOutlets

`IBOutlets` are used to connect an object's properties in Interface Builder to properties in their respective classes. (The "IB" in "IBOutlet" stands for "Interface Builder".) In our case, we want to connect the two labels in our *listNotesTableViewCell* in Interface Builder, to two label properties in our *ListNotesTableViewCell* class.

When creating `IBOutlets` we will often use the *Assistant Editor*. The *Assistant Editor* provides us a way to simultaneously view our object in Interface Builder and its accompanying source file.

> [action]
Open the *Assistant Editor* as follows:
>
1. Click the *Assistant Editor* icon.
>
  ![image showing the assistant editor icon](./images/assistant.png)
>
2. Hide the *Navigator* and *Utilities* menus.
>
  ![image showing how to setup the assistant editor](./images/hide.png)
>
3. Setup the *Assistant Editor* by selecting: `Manual > MakeSchoolNotes > MakeSchoolNotes > Views > ListNotesTableViewCell.swift`
>
  ![image showing how to setup the assistant editor](./images/setup.png)



With the *Assistant Editor* open, connecting our labels from Interface Builder to the *ListNotesTableViewCell* class is very easy: all we have to do is select the label and ***Control-click*** from the label to somewhere inside the *ListNotesTableViewCell* class definition.

> [action]
Add `IBOutlets` to the the *ListNotesTableViewCell* class as follows:
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/add-iboutlets.mov)

We can now access the labels of our *listNotesTableViewCell* through the `noteTitleLabel` and `noteModificationTimeLabel` instance properties of the *ListNotesTableViewCell* class! One last small change and then we will be ready to run our app!

#Typecasting our Cell to ListNotesTableViewCell

> [action]
Switch back to the *Standard Editor*, show the *Navigator* and *Utilities* menus, and select the `NotesViewController.swift` file.
>
![image showing how to switch back to standard editor](./images/standard-editor.png)

Now that we are using a custom cell with type *ListNotesTableViewCell*, we must make some changes to our table view methods. Let's make the necessary changes and then discuss what changed afterwards.

> [action]
Replace the content of the `tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell` method with the following:
>
    // 1
    let cell = tableView.dequeueReusableCellWithIdentifier("listNotesTableViewCell", forIndexPath: indexPath) as! ListNotesTableViewCell
>    
    // 2
    cell.noteTitleLabel.text = "note's title"
    cell.noteModificationTimeLabel.text = "note's modification time"
>    
    return cell

<!-- ACTION: When casting the cell type to ListNotesTableViewCell, add a sentence to explain (1) why this is okay (2) The benefit:  Help students make the connection between the specified return type and our cast (e.g. "Since ListNotesTableViewCell is a subclass of UITableViewCell, this is allowed.  In this way, we can return custom table view cells instead of the ones provided by Xcode.")   -->

So what did we change in the code above?

1. In this line the only difference between our previous implementation is that we appended `as! ListNotesTableViewCell` to the end. This casts the return type of this method to *ListNotesTableViewCell*. This is necessary because we want the table view to display our custom *listNotesTableViewCell* which has type *ListNotesTableViewCell*.
2. Because `cell` now has type *ListNotesTableViewCell*, we can access the `noteTitle` and `noteModificationTime` instance properties.

#Running the App!

We have now finished making our custom table view cells and are ready to run the app! Your app should look similar to this:

![image of finished custom table view cells](./images/finished-custom-cell.png)

Notice that the first cell is slightly hidden behind the *status bar*, let's fix that in the next tutorial!

<!-- ACTION: Add a tl;dr info box containing all steps they should have completed on this page of the tutorial.  For an example, see page 1 of tutorial. (see P01) -->
