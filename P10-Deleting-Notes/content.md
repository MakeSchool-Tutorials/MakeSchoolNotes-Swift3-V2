---
title: "Deleting Notes"
slug: deleting-notes
---

Now that we can add new notes and modify existing notes, let's add the functionality to delete notes.

# Deleting Notes

We would like our users to be able to swipe right on a note and be presented with a delete option:

<video width="100%" controls>
    <source src="https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/delete.mov" type="video/mp4">
</video>

Luckily, table views make this incredibly easy.

> [action]
Add the following method to the *ListNotesViewController* class:
>
    // 1
    override func tableView(tableView: UITableView, commitEditingStyle editingStyle: UITableViewCellEditingStyle, forRowAtIndexPath indexPath: NSIndexPath) {
      // 2
      notes.removeAtIndex(indexPath.row)
      // 3
      tableView.reloadData()
    }

1. By adding the `tableView(tableView: UITableView, commitEditingStyle editingStyle: UITableViewCellEditingStyle, forRowAtIndexPath indexPath: NSIndexPath)` method we are enabling the cells of our table view to display the delete option when a user swipes right.

2. We are removing the appropriate note from the `notes` array.

3. Because we modified the `notes` array, we are reloading the table view data.

And that's all we have to do to delete a note! =]

# Running the App!

You should now be able to add, modify, and delete notes!

<video width="100%" controls>
    <source src="https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/P10-complete.mov" type="video/mp4">
</video>

In the next tutorial we will start working on persisting data between app launches!
