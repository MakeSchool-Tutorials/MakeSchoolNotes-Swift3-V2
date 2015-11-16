# The View Controller Lifecycle

In iOS, view controllers can be found in one of three states:

1. Not yet loaded
2. Currently on screen
3. Currently off screen

Although view controller management (keeping track of a particular view controller's current state) and transitions (transition from one state to some other state) is primarily handled by the operating system, there are three methods which we can use to execute code when

When a view controller is transitioning between states, it will use one of these three methods:

1. `viewDidLoad()`
2. `viewWillAppear()`
3. `viewWillDissappear()`


The `viewDidLoad()` method is called after a view controller has been successfully loaded. Once a view controller is loaded, it will remain in memory until we exit the app. This means that the `viewDidLoad()` method is called only *once* for any particular view controller.

Once a view controller is loaded into memory, we will want to display it on screen

 Once a view controller is loaded, we do not need to load it again, even if we navigate away from the called *once*  after the view has been successfully loaded, and thus is only called **once** during a view controller's lifecycle.

The `viewWillAppear()` is called whenever a view controller is transitioning from offscreen to onscreen, and can thus be called **multiple** times during a view controllers lifecycle.

The `viewWillDissappear()` method is similar to `viewWillAppear()` in that it can also be called **multiple** times throughout the lifecycle of a view controllers; however, it is called when the view controller is transitioning from onscreen to offscreen.

Note that there are also `viewDidAppear()` and `viewDidDisappear()` methods. This methods are called after the view controller has appeared
