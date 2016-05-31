![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png)

#Ruby <3's OOP

### Why is this important?
<!-- framing the "why" in big-picture/real world examples -->
*This workshop is important because:*

OOP Enables you write code that is:

* Easy to understand
* Better organized to handle complexity
* More consistant
* Modular

### What are the objectives?
<!-- specific/measurable goal for students to achieve -->
*After this workshop, developers will be able to:*

* Distinguish between hashes & classes
* Initialize instances of a class
* Explain how `.new()` and `initialize()` are related
* Define getter and setter methods
* Compare and contrast instance and class variables
* Use inheritance to extend the behavior of a class
* Define `private` methods on a class

### Where should we be now?
<!-- call out the skills that are prerequisites -->
*Before this workshop, developers should already be able to:*

- Write basic Ruby
- Have exposure to OOP concepts

##Review: Hashes & Objects

Let's create a new Hash

* Hashes are simple key value stores.

>How can I organize my data using key/value pairs in Ruby? Like so:

```ruby
{name: "Napoleon", fav_food: "steak", skills: ["archery", "combat", "egg farming"]}
```

Everything in Ruby is an Object; however, we almost never use plain vanilla Objects because there are more sophisticated implementations of them such a `String`, `Integer`, and `Hash`.

>How can we prove that the Hash we just created inherited from `Basic Object`? Hint: try `.superclass`.

## Why OOP?

#### Easy to Understand

Objects help us build programs that model how we tend to think about the world. Instead of a bunch of variables and functions (procedural style), we can group relevant data and functions into objects, and think about them as individual, self-contained units. This grouping of properties (data) and methods is called *encapsulation*.

#### Managing Complexity

This is especially important as our programs get more and more complex. We can't keep all the code (and what it does) in our head at once. Instead, we often want to think just a portion of the code.

Objects help us organize and think about our programs. If I'm looking at code for a Squad object, and I see it has associated *people*, and those people can dance when the squad dances, I don't need to think about or see all the code related to a person dancing. I can just think at a high level "ok, when a squad dances, all it's associated people dance". This is a form of *abstraction*... I don't need to think about the details, just what's happening at a high-level.

#### Ensuring Consistency

One side effect of *encapsulation* (grouping data and methods into objects) is that these objects can be in control of their data. This usually means ensuring consistency of their data.

Consider the bank account example... I might define a bank account object such that you can't directly change it's balance. Instead, you have to use the `withdraw` and `deposit` methods. Those methods are the *interface* to the account, and they can enforce rules for consistency, such as "balance can't be less than zero".

#### Modularity

If our objects are well-designed, then they interact with each other in
well-defined ways. This allows us to refactor (rewrite) any object, and it should not impact (cause bugs) in other areas of our programs.

## The OOP Process

Putting your idea in a nutshell gives you a starting place for what those objects may be:

> "Tic Tac Toe is a game where players try to get three squares in a row."

- Game
- Players
- Squares

> "Garnet is a site where students can track their homework and attendance."

- Students
- Homework
- Attendance

> "Amazon is a site where people can order products."

- People
- Orders
- Products

##Classes

### Define and Instantiate a Class

Class definitions require three basic components: the reserved word `class`, a name of the class, and the reserved word `end`. By convention, the class name begins with a capital letter.

```ruby
class Car
end
```

To create instances of our class, we will create a variable and assign it the return value of the `Car` class's `Car.new` method.

```ruby
fiat = Car.new
fiat.class
#=> Car
```

### Instance methods

We are able to create instances of a `Car`, but our class is *very* simple and our instances currently don't give us much. We'd like to add properties and behaviors (attributes and methods) for our car instances. We'll start by looking at **instance methods** and **instance variables**. An instance method represents a function that is accessible on every instance of a class. To create an instance method, we just create a regular method inside our class definition. Here's the syntax:

```ruby
class Car
  def drive
    "You're going places!"
  end
end
```

Now every car we create will have a "drive" behavior.

```ruby
fiat.drive
```

###Challenge: Race (5m)

**Enable this code:**

```ruby
car.race
#=> "And I'm off!"
```

>Classes are analogous to constructors in JavaScript.

##Attributes & Instance Variables

What should we do if we want to set attributes on the car, such as a paint color and year?

Let's give each instance of `Car` a color using instance variables.

An instance variable, which begins with the `@` symbol, has the capability of storing data for each instance of a class.

To do this, we'll create getter and setter methods, first by hand and then with a shortcut. We do this instead of accessing instance variables directly. We can then access the getters and setters with dot notation.  Let's look at an example.

```ruby
class Car
  def color=(color)
    @color = color
  end

  def color
    @color
  end
end

vw = Car.new
vw.color = "black"
vw.color
#=> "black"

bmw = Car.new
bmw.color = "yellow"
bmw.color
#=> "yellow"
```

>Note: `color=` is a method. Additionally ruby doesn't require parenthesis for arguments passed into a method. As a result, calling this method looks very much like an assignment (which is the point).

###Challenge: Engine Size (5m)

**Enable this code:**

```ruby
fiat = Car.new
fiat.cc = "500"
fiat.cc
#=> "500"
```

Every time an instance of `Car` is assigned a color, it will have its *own* instance variable named `@color`.

##Initialization

>Goal: let's create a Car that goes "Vroom" when it's first *initialized*

In Ruby there's a built-in method named `initialize` that is invoked every time the `.new` class method is called.

```ruby
class Car
  def initialize
    "Vroom"
  end

  def color=(color)
    @color = color
  end

  def color
    @color
  end
end

bmw = Car.new
audi = Car.new
```

>Goal: let's give the car a color when it's created.  

```ruby
class Car
  def initialize(color)
    @color = color
  end

  def color=(color)  
    @color = color
  end

  def color
    @color
  end
end

bmw = Car.new("black")
bmw.color
```

Since we don't *have* to put parentheses around method parameters when we call them, we can change a car's color with familiar syntax:

```ruby
bmw.color=("red")
bmw.color="red with FLAME decals"
# both work to change the car's color
```


## `attr_*`

If we add more methods to our class, we will start to notice a lot of repetition:

```ruby
class Car
  def initialize(color, make)
    @color = color
    @make = make
  end

  def make=(make)
    @make = make
  end

  def make
    @make
  end

  def color=(color)
    @color = color
  end

  def color
    @color
  end
end
```

Every getter has a method with a name (`make` and `color`) that is included in the instance variable (`@make` and `@color`). The same is true for setters.  

Ruby provides a syntax to shorten this common pattern: attributes. Attributes allow for the introduction of a different syntax for creating getters and setters. Let's demonstrate this by using Ruby's attributes to create a getter for `make`.


``` ruby
class Car
  attr_reader :make  # getter

  def initialize(color, make)
    @color = color
    @make = make
  end

  def make=(make)
    @make = make
  end

  def color=(color)
    @color = color
  end

  def color
    @color
  end
end

bmw = Car.new("black", "bmw")
bmw.make
```

`attr_reader` does the same thing as our former method `make`. We can also use this shorthand syntax to create setters with attributes:

``` ruby
class Car
  attr_reader :make  # getter
  attr_writer :make  # setter

  def initialize(color, make)
    @color = color
    @make = make
  end

  def color=(color)
    @color = color
  end

  def color
    @color
  end
end
```

Finally, if we want both a getter (reader) and setter (writer), we can use attr_accessor:

```ruby
class Car
  attr_accessor :make, :color  # getters and setters!

  def initialize(color, make)
    @color = color
    @make = make
  end
end
```

###Challenge: Animals (10m)

**Enable this code:**

```ruby
# Create a new animal
zazu = Animal.new("hornbill")
zazu.type
#=> "hornbill"
# Animal is awake by default
zazu.state
#=> "awake"
# Make the animal sleep
zazu.sleep
#=> "asleep"
# Make the animal wake up
zazu.wake
#=> "awake"
# Feed the animal
zazu.eat("bugs")
#=> "Yum! I, as a hornbill love to eat bugs!"
```

<details><summary>Example solution</summary>

```ruby
class Animal
  attr_accessor :state, :type
  def initialize(kind)
    @type = kind
    @state = "awake"
  end

  def sleep
    @state = "asleep"
  end

  def wake
    @state = "awake"
  end
  
  def eat (food)
    "Yum! I, as a #{@type} love to eat #{food}!"
  end
end
```

</details>


##Class variables, class methods, and `self`

Now, there may be moments where we want to get or set properties and behaviors that relate to all instances of a class, as a group. In this case, we need to explore class methods and class variables.

Class methods and class variables are used when data pertains to more than just an instance of a class. Let's imagine that we want to keep count of all cars that were instantiated. We use a class variable, indicated by `@@`.


```ruby
class Car
  attr_accessor :make, :color
  @@count = 0

  def initialize(color, make)
    @color = color
    @make = make
  end
end
```

Adding the class variable was easy enough.  Next, we'll define a getter for it using the keyword `self`. We will explore `self` more during the next several days. For now, know that if you place the word `self` next to a method name, it places the method on the class instead of on an instance of the class.

```ruby
class Car
  attr_accessor :make, :color
  @@count = 0

  def initialize(color, make)
    @color = color
    @make = make
  end

  def self.count
    @@count
  end
end

Car.count
#=> 0
```

Our car count isn't actually counting anything yet!  We'll update the count in the `initialize` method so that it increases each time a car is created.

```ruby
class Car
  attr_accessor :make, :color
  @@count = 0

  def initialize(color, make)
    @color = color
    @make = make
    @@count += 1
  end

  def self.count
    @@count
  end
end

Car.count
#=> 0

bmw = Car.new('bmw', 'white')
Car.count
#=> 1

audi = Car.new('audi', 'silver')
Car.count
#=> 2
```

##Inheritance

Inheritance lets us reuse code from one class as we create subtypes of that class.


### Base Class

We'll use our `Car` class as a *baseclass* and make a new `Pickup` class that inherits from `Car`.  Another way to say this is that `Pickup` will be a *subclass* of `Car`.  You can also think of `Car` as `Pickup`'s *parent* and `Pickup` as `Car`'s *child* (as in a class inheritance tree).

Notice that this `Car` class is spruced up with a new `@speed` instance variable and an `accelerate` instance method.

```ruby
class Car
  attr_accessor :make, :color, :speed
  @@count = 0

  def initialize(color, make)
    @speed = 0
    @color = color
    @make = make
    @@count += 1
  end

  def self.count
    @@count
  end

  def accelerate(change)
    @speed += change
  end
end
```


### Subclass

The syntax for inheritance uses `<` in the class definition.  Our pickup trucks will have another property that cars don't: the capacity of their truck beds (`@bed_capacity`). They'll also have a behavior that allows for riding in the back (`ride_in_back`).

```ruby
class Pickup < Car

  def initialize(color, make, bed_capacity)
    @speed = 0
    @color = color
    @make = make
    @bed_capacity = bed_capacity
    @@count += 1
  end

  def ride_in_back
    "Bumpy but breezy!"
  end
end
```

Even though we didn't define the `accelerate` method again, a pickup truck will inherit the behavior from the `Car` class.

```ruby
truck_one = Pickup.new("red", "Ford", 100)
truck_one.speed
#=> 0
truck_one.accelerate(40)
truck_one.speed
#=> 40
```

Inheritance doesn't go the other way, new cars don't know how to use the `ride_in_back` behavior.

```ruby
focus = Car.new("green", "Ford")
#=> #<Car:0x007f8c4c1b3520 @speed=0, @color="green", @make="Ford">
focus.ride_in_back
#=> NoMethodError: undefined method `ride_in_back' for #<Car:0x007f8c4c1b3520 
#=> @speed=0, @color="green", @make="Ford">
```

>In this example all subclasses share the same `@@count` class variable, so changing a class variable within a subclass changes the class variable for the base class and all other subclasses.

## Super

We can refactor the `Pickup` class with the super keyword, that is a placeholder for anything the car has previously defined. The example below is equivalent to above.

```ruby
class Pickup < Car

  def initialize(color, make, bed_capacity)
    super(color, make)
    @bed_capacity = bed_capacity
    @@count += 1
  end

  def ride_in_back
    "Bumpy but breezy!"
  end
end
```

###Challenge: People (15m)

**Enable this code**

```ruby
# Create people
justin = Person.new(33, "male", "Justin")
jimmy = Person.new(27, "male", "Jimmy")
# Inherit from animals
justin.state
#=> "awake"
# Greet the world
justin.greet
#=> "Hi, I'm Justing. I'm a male, and 33 years old."
# Eat food
justin.eat('carrots')
#=> "Yum! I am eating carrots!"
# But not other people
jimmy.eat('person')
#=> "Sir! I am NOT a cannibal!"
# Keep track of the people count
Person.total
#=> 2
```

<details><summary>Example solution</summary>

```ruby
class Person < Animal
  @@count = 0
  attr_accessor :age, :gender, :name
  def initialize(age, gender, name)
    super("person")
    @age = age
    @gender = gender
    @name = name
    @@count += 1
  end

  def self.total
    @@count
  end

  def eat(food)
    food == "person" ? 
      "Sir! I am NOT a cannibal!" :
      "Yum! I am eating #{food}!"
  end

  def greet
    "Hi, I'm #{@name}. I'm a #{@type}, and #{@age} years old."
  end
end
```
</details>

##Private Methods

By default all instance and class methods are *public*. This means they're visible to other objects. An analogy: they're functions that have their own buttons on the outside of the machine, like a car's turn signal.

There may be methods other objects don't need to know about. Let's imagine a `User` class that has a password that gets encrypted when initialized...

>Exercise: Turn to a partner and identify any parts of the code you are unclear about.

```rb
require 'base64'
class User
  attr_accessor :firstname, :lastname
  @@all = []

  def initialize(firstname, lastname, password)
    @firstname = firstname
    @lastname = lastname
    @password = encrypt(password)
    @@all.push(self)
  end

  def full_name
    "#{@firstname.capitalize} #{@lastname.capitalize}"
  end

  def User.all
    @@all
  end

  private
  def encrypt(password)
    Base64.encode64(password)
  end
end
```
```rb
juan = User.new("Juan", "Juanson", "wombat")
# #<User @firstname="Juan" @password="tabmow">
juan.encrypt("wombat")
# Error! Private method `encrypt`
```

![ruby, ruby, ruby, ruby!](http://treasure.diylol.com/uploads/post/image/261103/resized_all-the-things-meme-generator-ruby-ruby-ruby-ruby-204707.jpg)
[ruby, ruby, ruby, ruby!](https://youtu.be/qObzgUfCl28?t=50s)

##Additional Resources

* [Tutorial Point: Object Oriented Ruby](http://www.tutorialspoint.com/ruby/ruby_object_oriented.htm)
* [Bastards Book of Ruby: Object Oriented Concepts](http://ruby.bastardsbook.com/chapters/oops/)
* [Sandi Metz: Object Orient Design (video)](https://vimeo.com/12350535)