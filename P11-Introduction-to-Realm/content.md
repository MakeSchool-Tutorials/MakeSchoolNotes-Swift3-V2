---
title: "Introduction to realm"
slug: intro-realm
---

With the ability to add, modify, and delete notes, our app is nearly finished! The last step is to persist data between app launches so that user's notes won't get deleted every time they close the app.

[Realm](https://realm.io/) is a fast and easy-to-use mobile database that's used by companies like Google and Amazon. Realm is used to save and retrieve objects locally on our user's device. In Make School Notes, we will need want to save all of a user's notes when they close the app, and retrieve all of their notes when they reopen the app.

#Importing the Realm Framework

When using the Realm framework in Swift you must use the following import statement:

```
import RealmSwift
```

We will assume that `RealmSwift` has been properly imported for the remainder of this tutorial.

#Realm's Object Type

When using Realm to store objects, the object we are storing must inherit from a Realm provided class called `Object`. For instance, if we wanted to store and retrieve objects of type `Person`, we would declare the `Person` class as follows:

```
class Person: Object {
    //...
}
```

If our Person class had `name` and `age` properties, they would need to be declared as `dynamic` like so:

```
class Person: Object {
  dynamic var name = ""
  dynamic var age = 0
}
```

Although we added a special superclass and a fancy keyword to the properties of our `Person` class, functionally it is the same as a regular class. We would create an instance of the `Person` class like this:

```
var chris = Person()
chris.name = "Chris"
chris.age = 23
```

## Realm's Realm Type

Although Realm is the name of the framework, the framework also has an object that has type `Realm`. We save and retrieve objects from the *default Realm*. Realm (the framework) can be configured in many different ways, but for our purposes, the default Realm will suffice.

Before adding, modifying, retrieving, or deleting objects in Realm, we must get access to the default Realm:

```
let realm = try! Realm()
```

The `try` keyword in the above code signals that the call to `Realm()` can *throw an error*. (Throwing an error is a fancy way of saying that a method can fail.) By using the `try!` keyword (note the exclamation point) we are indicating that we know the `Realm()` method can throw an error, but that we are sure that it will not, and therefore will not handle the error case.

In Swift, errors are handled using the do/try/catch paradigm. For more information on the do/try/catch paradigm of error handling, check out this quick [tutorial](link to error handling tutorial).

## Write Transactions in Realm

*Write transactions* are a special way to inform Realm that we want to change something in the default Realm. For instance, if we wanted to add, modify, or delete an object we would do so inside of a write transaction. (We do not need to retrieve objects inside write transactions because object retrieval does not change anything in the default Realm.)

To start a write transaction, we do the following:

```
//write transaction
try! realm.write(){
  // save, modify, or delete some object(s)
}
```

## Saving Objects

Once inside the write transaction, we can save an object using the `add()` method. We pass the object we want to save to the `add()` method. If we wanted to save the `chris` variable from above, we would do the following:

```
try! realm.write(){
  realm.add(chris)
}
```

The above code saves the `chris` variable to the default realm.

## Updating Objects

Updating objects in the default Realm is really easy. All we have to do is create a write transaction and modify the object as desired. If we wanted to change the age property of the `chris` variable, we would do the following:

```
try! realm.write(){
  chris.age = 100
}
```

The above code will both update the age of `chris` and save it in the default realm.

## Retrieving Objects

We use the `objects()` method to retrieve objects. We pass the *type* of the desired object to the `object()` method to specify what kind of objects we want to retrieve. If we wanted to retrieve all of our *Person* objects we would do the following:

```
let realm = try! Realm()
let people = realm.objects(Person)
```

The call to `realm.objects(Person)` will return all of the objects of type *Person* that have been saved to the default realm.

## Realm's Results Type

In the above code, the call to `realm.objects(Person)` returns an object of type `Results<Person>`. The `objects()` method is special because it returns a template type, indicated by the angled brackets <>. Template types are useful because they can take on many different types. For instance, the return type in the code above is `Results<Person>` because we requested all Person objects from our default Realm. However, if we had a `Dog` type and requested all Dogs from our default Realm, the return type would be `Results<Dog>`.

The `Results` type is similar to arrays and can be indexed using square bracket [] notation or using `for..in` loops.  

## Filtering Objects

Now what if we wanted to retrieve a specific instance of the *Person* class?

In Realm, there is no direct way to retrieve a single object. We must retrieve all the objects of the required type and then *filter* the returned `Results` object. For instance, if we wanted to retrieve the *Person* object where the `name` property is set to "Chris" and the `age` property is set to 23, we would do the following:

```
let realm = try! Realm()
let chris = realm.objects(Person).filter("name = 'Chris' AND age = 23")
```

# Deleting Objects

Once inside the write transaction, we can delete an object using the `delete()` method. We pass the object we want to delete to the `delete()` method. If we wanted to delete the `chris` variable, we would do the following:

```
try! realm.write(){
  realm.delete(chris)
}
```

The above code deletes the `chris` variable from the default realm.

#Wrapping Up

We have discussed how to add, modify, retrieve, and delete objects in Realm and are ready to add data persistence to Make School Notes!

If you would like more practice with Realm, check out this [demo project](http://github.com/orcudy/RealmDemo).

If you are interested in some of the more advanced features that Realm offers, check out the [full documentation](https://realm.io/docs/swift/latest/).
