# Introduction to Error Handling

When programming, it is common to see functions that have the possibility of failing. For instance, consider the scenario where a user wants to send data over the internet, but doesn't have an internet connection. Should the application crash? Probably not. It makes more sense for the application to display a message to the user indicating that they aren't currently connected to the internet.

But how would an app developer know that a user isn't currently connected to the internet? What if the user really was connected to the internet, but just had airplane mode turned on?

Using the above example, we might handle errors like so:

```
if (errorOccurred){
  //handle error
  if (notConnectedError){
    //display message indicating that the user isn't currently connected to the internet
  } else if (airplaneModeError) {
    //prompt user to turn off airplane mode
  } else {
    //display default message that some unknown error occurred
  }
} else {
  //continue as normal
}
```

Notice that we want to do something different for each specific error. Generally, error handling is characterized by detecting when an error occurred, determining the specific error, and taking the appropriate action depending on the specific error.

# Error Handling in Swift

In Swift, when an error occurs in a function, the function is said to have *thrown* an error. Functions that can throw errors are called *throwing function*.

## Defining Throwing Functions

We can indicate that a function has the possibility of throwing an error by using the `throws` keyword as follows:

```
func sendDataAcrossInternet() throws -> Int {
  //do something
}
```

We can define the types of errors that a function can throw by creating an `enum` that conforms to the `ErrorType` protocol. So let's say we wanted to define some errors that could be thrown by the `sendDataAcrossInternet()` function, we would do something similar to the following:

```
enum InternetError: ErrorType {
  case NotConnected
  case AirplaneModeEnabled
}
```

We can now define our `sendDataAcrossInternet()` function to throw specific errors like so:

```
func sendDataAcrossInternet() throws -> Int {
  if (userNotConnectedToInternet){
    throw InternetError.NotConnected
  } else if (userInAirplaneMode){
    throw InternetError.AirplaneModeEnabled
  }
}
```

## Calling Throwing Functions Using Do-Catch

Because throwing functions have the possibility of throwing an error, we have to call them in a special way.

Using what is called a `do-catch` block we can call a throwing function and handle all the possible errors that a specific throwing function can throw. For instance, to call the `sendDataAcrossInternet()` method, we would do the following:

```
//1
do {
  //2
  try sendDataAcrossInternet()
  //3
  print("Data sent successfully.")
} catch InternetError.NotConnected {
  //4
  print("You are not currently connected to the internet.")
} catch InternetError.AirplaneModeEnabled {
  //5
  print("Disable airplane mode to proceed.")
}
```

1. We are indicating the start of a `do-catch` block using the `do` keyboard.

2. We are trying to call `sendDataAcrossInternet()` method using the `try` keyword. The `try` keyword indicates that the following function is a throwing function.

3. This line will only be printed if the call to `sendDataAcrossInternet()` does not throw an error. If an error is thrown execution jumps to the appropriate `catch` block.

4. This line will only be printed if the the call to `sendDataAcrossInternet()` throws the `InternetError.NotConnected` error.

5. This line will only be printed if the the call to `sendDataAcrossInternet()` throws the `InternetError.AirplaneModeEnabled` error.

The `do-catch` block is similar to a `switch` statement in that only one of the above code blocks will be executed.

## Calling Throwing Functions Using Try!

Alternatively, we can call a throwing function using the `try!` keyword. The `try!` keyword allows us to call a throwing function without having to consider the possible errors that could be thrown. The `try!` keyword should only be used when you know that a throwing function will not throw an error during runtime. (If an error is thrown during runtime, your program will crash.) For instance, if we had a throwing function called `loadImage()` and we are guaranteed that the image we want to load exists (maybe because it was included in the application), we could call the `loadImage()` function as follows:

```
let image = try! loadImage(pathToIncludedImage)
```

This covers the basics of error handling in Swift. For an in-depth look, take a look at Apple's great [documentation](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html).
