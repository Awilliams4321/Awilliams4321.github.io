---
layout: post
title:      "'Object Oriented Ruby'"
date:       2020-04-10 16:27:39 +0000
permalink:  object_oriented_ruby
---


The OO Ruby module consisted of several foreign concepts and it took a lot of repitition from the labs and lessons for me to truly understand the relationship between Classes, methods, variables, and how all of these things interact with objects. It wasnt until the Has Many Objects Through lab that i really started to understand and appreciate object relationships. Below is a quick breakdown of how object relationships work.

The purpose of object relationships is to allow data from different classes to interact and associate with one other. The most basic way to do this is by creating a " Has Many <=> Belongs To" relationship, also know as Inheritance(Parent <=> Child). We can use real world examples to make this easier to understand. Just like how an Owner "has many" Pets and a Pet "belongs to" an Owner, or, an Artist "has many" songs and a Song "belongs to" an Artist, we can set up our class relationships following that same flow. Referring back to our Owner <=> Pets example, in order to enact a relationship between the two classes we would need to give our Pets class a setter(attr_writer) and a getter(attr_reader) for owner. To go into a little more detail, an attr_writer is a method that sets the variables name and makes it "writable" so that we can call it. An attr_reader is a method gets the values assigned to the variable so that we can "read" information stored in it. In the Pets class, this would look like:
```
class Pets
 
  attr_reader :owner
  attr_writer :owner
 
end
```

If we know that we are going to be using a setter and a getter method for the same attribute in a class, we can use the macro 'attr_accessor' instead which is the equivalent function of both our reader + writer methods. We would then refractor our code to look like this:
```
class Pets
 
  attr_accessor :owner
 
end
```
Yay, less code!

Again, we define an attr_accessor as :owner to create a doorway to give our Pets the ability to have an Owner and our Owner class the ability to refer the data in the Pets class. The above examples have been simplified to only show how to build the relationship between the two classes. However, just as pets have multiple attributes in the real world,such as a name, breed, and color, our Pets class would also have multiple other attributes(:name, :breed, :color, etc.).




