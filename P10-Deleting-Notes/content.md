---
title: "Deleting Notes"
slug: deleting-notes
---

In this section, we'll implement the functionality for deleting notes.

We'll use the built-in functionality of `UITableView` for deleting objects. It's super easy! (Really)

> [action]
In `ListNotesTableViewController`, add the following method with your other table view data source methods:
>
```
override func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCellEditingStyle, forRowAt indexPath: IndexPath) {
    if editingStyle == .delete {
        notes.remove(at: indexPath.row)
    }
}
```
>
In the code above, we override the table view data source method `tableView(_:commit:forRowAt:)` to implement the table view's built-in slide-to-delete functionality. Within the `tableView(_:commit:forRowAt:)` method, we check for the delete editing mode and delete the note at the corresponding index path.

That was easy!

## Running the App

We've just finished implementing the functionality for deleting notes. Let's create a few notes and try deleting them to make sure everything works.

> [action]
Build and run the app. Create a few new notes and try deleting them.
>
![ms-video(https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p10_deleting_notes/delete_note_checkpoint.mp4)

In the next section, we'll introduce _Core Data_ and learn how to persist our note data between app launches.
