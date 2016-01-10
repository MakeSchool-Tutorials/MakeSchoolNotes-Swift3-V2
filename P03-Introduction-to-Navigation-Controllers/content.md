---
title: "Introduction to Navigation Controllers"
slug: intro-nav-controller
---

The *navigation controller* is a special controller that manages how other view controllers are displayed. Like table views, navigation controllers are very common throughout iOS apps.

To demonstrate the functionality provided by a navigation controller let's use Apple's *Calendar* app: The *Calendar* app has several different layers which can be thought of as screens that are stacked one on top of the other. The top screen of the *Calendar* app consists of a calendar that shows the entire year:

![calendar app year view](./images/year.png)

The top grey section you see in the *Calendar* app is called the *navigation bar* and is provided by the navigation controller. When we tap on a specific month, we go a layer deeper into the app. Since we are no longer on the top layer, we now have the option to go back to the previous layer. This *back button* functionality is provided for free by the navigation controller.

![calendar app month view](./images/month.png)

If we select a specific day, we will once again go another layer deeper into the app. If we wanted to go back to any of the previous layers, all we would have to do is keep hitting the back button.

![calendar app day view](./images/day.png)

Notice that the **Calendar** app has a hierarchical (layered) structure, meaning that the only way to get to the *day screen* is to go through the *year* and *month screens* first. Navigation controllers are best suited for apps that have a similar hierarchical structure.

In Make School Notes we will have two different layers: a top layer that display all the notes, and a bottom layer that displays a specific note after a user taps a specific table view cell. Because we have a layered structure, Make School Notes is a great place to use a navigation controller.

#Setting up the Navigation Controller

Although navigation controllers provide us with some pretty complex functionality, luckily, they are incredibly easy to set up.

> [action]
Set up a navigation controller by selecting the *List Notes Table View Controller* from the Document Outline and selecting Editor > Embed In > Navigation Controller.
>
![adding a navigation controller image](./images/add-nav.png)

You should now see a navigation controller in your storyboard:

![navigation controller in storyboard image](./images/nav.png)

#Naming the Screen

Another cool feature of a navigation controller is the ability to display the title of the current screen in the navigation bar. Let's give our *List Notes Table View Controller* screen the name "Notes".

> [action]
Select the *Navigation Item* in the Document Outline, click on the Attributes inspector, and set the *Title* field to "Notes".
>
![setting the title to "Notes" image](./images/notes.png)

Notice that after setting our title to "Notes" the name of the *List Notes Table View Controller* changed to "Notes" in the Document Outline. However, as this view controller is used to list notes, the name *List Notes Table View Controller* will continue to be used.

#Running the App!

Your app should now look something like this:

![app with navigation controller image](./images/finished.png)

Great job -- our note app is really starting to come along!
