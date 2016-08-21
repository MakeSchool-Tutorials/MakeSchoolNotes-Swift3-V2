---
title: "Deleting Notes"
slug: deleting-notes
---

Now that we can add new notes and modify existing notes, let's make it so users can delete notes.

We would like our users to be able to swipe right on a note and be presented with a delete option:

![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/delete.mov)

Luckily, table views make this incredibly easy.

> [action]
Add the following method to the *ListNotesTableViewController* class:
>
    // 1
    override func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCellEditingStyle, forRowAt indexPath: IndexPath) {
        // 2
        if editingStyle == .delete {
            // 3
            notes.remove(at: indexPath.row)
            // 4
            tableView.reloadData()
        }
    }

1. By implementing `tableView(_:commitEditingStyle:forRowAtIndexPath:)`, we enable the the table view to have additional editing modes, one of which is that the cells display the delete option when a user swipes right. The other mode is an insert mode, but that won't appear without additional configuration.

2. We check to see if the `editingStyle` is the `.Delete` one - there's also an `.Insert` one. We wouldn't want to accidentally delete a user's notes when they intended to insert a new one!

3. We are removing the appropriate note from the `notes` array, using the `row` property of the passed in `indexPath`.

4. Because we modified the `notes` array, we must tell the table view to update itself with `reloadData()`.

And that's all we have to do to delete a note! =]

#Running the App!

You should now be able to add, modify, and delete notes!

![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/P10-complete.mov)

In the next tutorial we will start working on persisting data between app launches!

>[info]
>###On this page, you should have:
>
>1. Implemented `tableView(_:commitEditingStyle:forRowAtIndexPath:)` in the *ListNotesTableViewController* so that users can delete notes by swiping right on them.
>2. Run and tested your app!
