## Introducing Delegates

The concept of a delegate in iOS is very similar to the concept of a delegate in the real world. When we delegate tasks in the real world, we are effectively entrusting someone to do something on our behalf. For instance, if you were the CEO of a company, you may delegate someone to be your secretary, whom you entrust to schedule all of your meetings. Although you don’t specifically know your schedule for any given day, to find out, all you have to do is ask your secretary.

This is very similar to what’s happening with delegates in iOS. Consider a table view: Table views do not explicitly know what information they should display, but they have an "information secretary" which they can use to find out. This information secretary is called the *dataSource*.

--

When hiring your secretary, you won’t just hire the first person that walks in your door, you will want to look for someone with a specific skill set, most likely someone that is really good at scheduling.

The same thing is true for table views: To be a table view’s *dataSource*, you must have the right “skill set”. Specifically, you must be able to tell the table view how many cells it has and what information to display for each cell. In iOS, to ensure that the table view’s *dataSource* has the right skill set, we use a concept called **protocols**. (For more information on *protocols* checkout this [quick explanation-BROKEN LINK](link to protocol explanation)). Specifically, a table view's *dataSource* must conform to the `UITableViewDataSource` protocol.

--

After finding the right secretary, you will want to hire them. This process usually consists of signing some paperwork, filling out tax forms, and assigning a start date.

In iOS, we can assign an information secretary (a *dataSource*) to our table view by setting the table view's `dataSource` property:

```
someTableView.dataSource = someInformationSecretary
```

Note that the `someInformationSecretary` object must conform to the `UITableViewDataSource` protocol or we cannot assign it as the `dataSource` of `someTableView`.

--

As CEO, when you want to ask your secretary about your schedule, you will probably send them an email.

The `someTableView` table view would talk to its *dataSource* (the `someInformationSecretary` object) through methods and properties defined by the `UITableViewDataSource` protocol. Namely, the `someTableView` table view would ask how many cells it should display using the `tableView(_:numberOfRowsInSection:)` method and would ask what information should be displayed in each cell by using the `tableView(_:cellForRowAtIndexPath:)` method. The `someInformationSecretary` object would define these method and the `someTableView` table view would automatically use them in the background.
