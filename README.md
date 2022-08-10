# SOLID
Just Learning SOLID principles.<br/>
    - [Single Responsibility](#s)<br/>
    - [Open-Closed](#o)<br/>
    - [Liskov Substitution](#l)<br/>
    - [Interface Segregation](#i)<br/>
    - [Dependency Injection](#d)<br/>

# S 
## SRP Single Responsibility Principle

>"Every software component should have one and only one reponsibility".

>"You should be able to describe what each class(or component) does without saying 'and'".



### Cohesion

>"Cohesion is the degree to which the varios parts of a software component are related". 

(Break down stuff.)

>"Higher Cohesion helps attain better adherence to the Single Responsibility Principle".


### Reusability

If you are going to need to reuse the functionality make it on a separate component. (Brek down stuff.)


### Coupling

>"Coupling is defined as the level of inter dependency between various software components".

>"Loose Coupling helps attain better adherence to the Single Responsibility Principle".

(Reduce coupling or dependancy.)



### Reasons for change

>"Every software component should have one and only one ~~responsibility~~ reason to change".



#### Look always forward for High Cohesion and Loose Coupling.


Go from this:

```
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
  // Getter
  get area() {
    return this.calcArea();
  }
  // Method
  calcArea() {
    return this.height * this.width;
  }
}
```

To this:

```
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}

class rectangleAreaCalculator {
   calcArea() {
      return this.height * this.width;
    }
}
```






# O 
## OCP Open-Closed Principle

>"Software components should be closed for modification, but open for extension".

Depend on stable abstractions and modify system's behavior by providing different realizations.<br/>
Protect your existing code from a subset of potential future changes that you can predict.

OCP = principle of polymorphism.

OCP = [PV (Protected Variation)](https://www.martinfowler.com/ieeeSoftware/protectedVariation.pdf)

OCP = Strategy Design Pattern

OCP = Abstract Factory Design Pattern

>"The fact that something can change doesn't imply that it will change, or that it will change according to your predictions".


### Open for extension

Should be possible to extend all modules.

A software component should be extendable to add a new feature or to add a new behavior to it.

### Closed for modification

No implementation changes after modules released to clients.

New features getting added to the software component, should NOT have to modify existing code.

### Protected Variation

Identify points of predicted variation and create a stable interface around them.

### OCP Aplication Framework

Apply OCP initially only when you're certain about the nature of future changes.

    1. Is the anticipated change related to intrinsic instability of business requirements?
    2. What kind of changes usually happen on projects like this one?
    3. If the potential change doesn't correspond to 1 or 2 above, don't apply OCP.
    4. When requirements change, identify opportunities for extraction of meaningful abstractions and then refactor accordingly.

THE RISK OF OVER-ENGINEERING IS EVER-PRESENT!

#### The Open-Closed Principle often requires decoupling, which, in turn, automatically follows the Single Responsibility Principle.


To explain this, let’s look at an example. Below we have a Vehicle class. When a Vehicle instance is created, we pass in the fuel capacity and fuel efficiency. To get our range, we simply multiply our capacity by our efficiency.

```
class Vehicle {
    constructor(fuelCapacity, fuelEfficiency) {
        this.fuelCapacity = fuelCapacity;
        this.fuelEfficiency = fuelEfficiency;
    }

    getRange() {
        return this.fuelCapacity * this.fuelEfficiency;
    }
}
```

But let’s say we add a new type of vehicle; a hybrid vehicle. This vehicle doesn’t just have standard fuel-based range, it also has an electric range which it can use as well. To find out the range now, we need to modify our getRange() method to check if the vehicle is hybrid, and add its electric range if so:

```
class Vehicle {
    constructor(fuelCapacity, fuelEfficiency) {
        this.fuelCapacity = fuelCapacity;
        this.fuelEfficiency = fuelEfficiency;
    }

    getRange() {
        let range = this.fuelCapacity * this.fuelEfficiency;

        if (this instanceof HybridVehicle) {
            range += this.electricRange;
        }
        return range;
    }
}

class HybridVehicle extends Vehicle {
    constructor(fuelCapacity, fuelEfficiency, electricRange) {
        super(fuelCapacity, fuelEfficiency);
        this.electricRange = electricRange;
    }
}
```

This violates the open-closed principle, because whilst adding our new HybridVehicle class we have had to go back and modify the code of our Vehicle class in order to make it work. Going forward, every time we add a new type of vehicle that might have different parameters for its range, we’ll have to continually modify that existing getRange function.

Instead what we could do, is to override the getRange method in the HybridVehicle class, giving the correct output for both Vehicle types, without every modifying the original code:

```
class Vehicle {
    constructor(fuelCapacity, fuelEfficiency) {
        this.fuelCapacity = fuelCapacity;
        this.fuelEfficiency = fuelEfficiency;
    }

    getRange() {
        return this.fuelCapacity * this.fuelEfficiency;
    }
}

class HybridVehicle extends Vehicle {
    constructor(fuelCapacity, fuelEfficiency, electricRange) {
        super(fuelCapacity, fuelEfficiency);
        this.electricRange = electricRange;
    }

    getRange() {
        return (this.fuelCapacity * this.fuelEfficiency) + this.electricRange;
    }
}
```



# L 
## LSP Liskov Substitution Principle

>"Objects should be replaceble with their subtypes without affecting the correctness of the program".

>"If it looks like a duck and quacks like a duck but it needs batteries, you probably have the wrong abstraction!"

>"If S is a subtype of T, then objects of type T should be replaceble with objects of type S without altering any of the desirable properties of the program (correctness, task performed, etc.)"

Subclassing or implementation doesn't imply subtype relation.

Subtype: Subclass or realization which can be substituted for the type it extends or implements.

["Abstractions can be used to encapsulate potential modifications. for example, suppose we want a program to run on different machines. We can accomplish this by inventing abstractions that hide tha differences between machines so that to move the program to a different machine only those abstractions need to be reimplemented. A good design principle is to think about expected modifications and organize the design by using abstractions that encapsulate the changes"<br/><br/>- Barbara H. Liskov](https://www.semanticscholar.org/paper/Data-Abstraction-and-Hierarchy-Liskov/36bebabeb72287ad9490e1ebab84e7225ad6a9e5?p2df)
(Open-Closed Principle)

[Behavioral Subtyping Using Invariants and Constrains](http://reports-archive.adm.cs.cmu.edu/anon/1999/CMU-CS-99-156.pdf)

### LSP Questions 

     1. Are there specific criteria for replaceability of supertypes by subtypes?
     2. Is there a framework we can use to identify LSP violations ahead of time?
     3.What's so important about LSP?

### LSP Rules 

#### Method Signature Rules

##### Contravariance of arguments

If a subclass implements a method from its superclass, then the number of arguments should be the same.

The type os each argument in subclass method should be the supertype of the type of the respective argument in superclass method.


##### Covariance of result

Either both superclass' and subclass' methods return result, or neither does.

If there is a a result, then the type of the result in the subclass is a subtype of the type of the result in the superclass.


##### Exception

Exceptions thrown by a method in the subclass should be contained in the set of exceptions thrown by the respective method in the superclass.


#### Method Pre- and Post-Condition Rules

##### Pre-condition
 
 Pre-condition: An assertion about the state of the system before the method is called.
 
 Pre-conditions required by methods of a subclass mustn't be stronger than pre-conditions required by methods of a superclass.
 
 
##### Post-condition
 
 Post-condition: An assertion about the state of the system after the method execution completes.
 
 Post-conditions guaranteed by methods of a subclass mustn't be weaker than post-conditions guaranteed by methods of a superclass.
 
 
#### Class Property Rules

##### Invariant

Invariant: An assertion about a specific class property which is always true.

Invariants guaranteed by a subclass must include all invariants guaranteed by a superclass.


##### Constraint

Constraint: An assertion about how a class property evolves over time.

Constraints enforced by a subclass must include all constraints enforced by a superclass.


### Break the hierarchy if it fails the substitution test.

```
class Rectangle {
    constructor(height, width) {
        this.height = height;
        this.width = width;
    }

    setHeight(newHeight) {
        this.height = newHeight;
    }
}

class Square extends Rectangle {}
```

In this example we initialise a Rectangle and Square, and output their dimensions. We then call the Rectangle.setHeight() on the Square object, and output its dimensions again. What we find is that the square now has a different height than its length, which of course makes for an invalid square.
1.
This can be solved, using polymorphism, an if statement in the Rectangle class, or a variety of other methods. But the real cause of the issue is that Square is not a good child class of Rectangle, and that in reality, perhaps both shapes should inherit from a Shape class instead.



### Tell, don't ask. 

Lot's of times you don't need to check for things just act upon them directly. 
Take a look at the example code for calculating how much to charge for a new carpet.

```
function area(room) {
    return room.length * room.width;
}

function priceForRoom(roomArea, carpet) {
    let sqrMetres = roomArea;
    if(carpet.roundUp){
        sqrMetres = Math.ceil(sqrMetres);
    }
    return sqrMetres * carpet.pricePerSqrMetre;
}

function quote(room, carpet) {
    return priceForRoom(area(room), carpet);
}

module.exports = quote;
```
Here’s the thing about functions: when they act on data structures (like JSON objects or tuples), the relationship between data and the logic that acts on that data inherently becomes unencapsulated. That’s just a fancy way of saying that carpet_quote.js knows too much. It knows how to calculate the area of a room based on the length and the width. It knows how to round up the area of a carpet if required. And it knows how to combine these things to get a price for that carpet in that room.

How could we encapsulate each of these jobs so that carpet_quote knows less? The answer may lie in closures.

We can encapsulate calculating the area of a room inside an outer function that takes the length and width as parameters.

```
function room(length, width) {
    function area() {
        return length * width;
    }
    return area;
}

module.exports = room;

```
And we can encapsulate the knowledge of calculating the price for that area of carpet inside an outer function that takes the price per square metre and whether or not to round up to the nearest square metre as parameters.
```
function carpet(pricePerSqrMetre, roundUp) {
    function priceForRoom(area) {
        let sqrMetres = area;
        if (roundUp) {
            sqrMetres = Math.ceil(sqrMetres);
        }
        return sqrMetres * pricePerSqrMetre;
    }

    return priceForRoom;
}

module.exports = carpet;

```
Then we can rewrite quote() as a higher-order function (just a fancy way of saying “inject the functions it uses”) that knows a lot less.
```
function quote(area, priceForRoom) {
    return priceForRoom(area());
}

module.exports = quote;
```
The data required is no longer exposed to carpet_quote. Instead, it’s passed to the outer functions of the closures in the client code – acting effectively as constructors. (Yes, closures are a lot like classes, dontcha think?)

Each module now only has one job, and has no direct implementation dependencies. And it makes the logic of calculating room areas and prices swappable. This design ticks all three boxes of good modular design:

   1. Each unit does one job
   2. Each hides its inner workings (especially its data)
   3. Their dependencies are swappable (by injection)



# I 
## ISP Interface Segregation Principle

>"No client should be forced to depend on methods it does not use".

ISP is a proiniciple of least knowledge and information hiding.

ISP = PSAP = Publish-Subscribe Architectural Pattern || UML sequence diagram. OCP

ISP = ODP = Observer Design Pattern

Careful with event buses. Too many can make code unmaintainable.

There are no interfaces in JavaScript. There is a way to mimic their behavior, but I don't think there's much sense. Let's better adapt the principle to the js world.

Let's define an "abstract" Phone class which will play role of the interface in our case:

```
class Phone {
  constructor() {
    if (this.constructor.name === 'Phone')
      throw new Error('Phone class is absctract')
  }

  call(number) {}

  takePhoto() {}

  connectToWifi() {}
}

```
Can we use it to define an iPhone?
```
class IPhone extends Phone {
  call(number) {
    // Implementation
  }

  takePhoto() {
    // Implementation
  }

  connectToWifi() {
    // Implementation
  }
}

```
Okay, but for an old Nokia 3310 this interface will violate the "I" principle
```
class Nokia3310 extends Phone {
  call(number) {
    // Implementation
  }

  takePhoto() {
    // Argh, I don't have a camera
  }

  connectToWifi() {
    // Argh, I don't know what wifi is
  }
}
```
The solution? Segregate the interface.

```
class Phone {
  constructor() {
    if (this.constructor.name === 'Phone')
      throw new Error('Phone class is absctract')
  }

  call(number) {}
}



class smartPhone extends Phone {
  takePhoto() {}

  connectToWifi() {}
}



class IPhone extends smartPhone {
  call(number) {
    // Implementation
  }

  takePhoto() {
    // Implementation
  }

  connectToWifi() {
    // Implementation
  }
}



class Nokia3310 extends Phone {
  call(number) {
    // Implementation
  }
}
```


# D 
## DIP Dependency Injection Principle

>"High-level modules should not depend on low-level modules. Both should depend on abstractions".

>"Abstractions should not depend on details. Details should depend on abstractions".

Dependency Injection is the technique in which an object receives other object in which it depends on.
The Dependency Inversion principle encourages us to depend on abstractions instead of concretions. This, too, has to do with separation of concerns.

Now lets try to break down the definition a bit. Lets start by object. An object is an instance of a class. For Example

```
// lets define a class dog
class Dog{
  speak(){
    console.log("wuff");
  }
}

//now lets create object dog
const fluffy = new Dog();

```
In the above example, we have a class Dog and fluffy is the object of the class Dog. When we new up a class we create an object of that class. This is one of the ways of creating an object in Javascript (and its the common way to create an object in languages like c# and java).
Lets now see an example where 2 objects are dependent on each other.


```
class Pet{
  whatDoesMyPetSay(){
    const pet = new Dog();
    pet.speak();
  }
}

const fluffy = new Pet();
fluffy.whatDoesMyPetSay();
// response will be "wuff"


```

Here, as we see, the class Pet is dependent on class Dog. So to get what we want, we need to create an instance of Dog inside our Pet class. Now this class is not reusable as it is tied to Dog class. If someone has a cat as Pet, they wont be able to use this class. This is what is called as tightly coupled code.
Now lets change this code and try to satisfy all other pet owners with dependency Injection. But first, lets create a cat class

```
class Cat{
  speak(){
    console.log("meow");
  }
}
```
The cat class must also implement the same method for Dependency Injection to work. In languages like C# and Java this is ensured by using an interface. But we have no such method in JavaScript, so it is up to the developer to remember it. Now lets see the new implementation of the pet class.

```
class Pet{
  //usually we have a private variable that needs 
  //to be accessed only in this class
  #pet;

  //create a constructor that recieves the dependent
  //object
  constructor(pet){
    this.#pet = pet;
  }

  whatDoesMyPetSay(){
    //as long as pet class implements speak method we are fine
    this.#pet.speak();
  }
}

//what does fluffy the dog say?
const fluffy = new Pet(new Dog());
fluffy.whatDoesMyPetSay();
//The response will be "wuff"

//what does milo the cat say?
const milo = new Pet(new Cat());
milo.whatDoesMyPetSay();
//The response will be "meow"
```
Now, we have removed the dependency from inside the pet class and have given it to the caller of the class. This promotes the reusability of the pet class. This is a very simple example and the purpose is to only understand dependency injection and not to implement it. In real world, the dependency is abstracted even from the caller and given to a new object, which is usually called an Injector.



### IOC Inversion Of Control

Inversion of Control is the phenomenon we observe when we delegate behavior to be implemented by someone else, but provide the hooks/plugins/callbacks to do so. We design the current component to invert control to another one. Lots of web frameworks are built on this principle.

Traditional control flow for a program is when the program only does what we tell it to do (today). Inversion of control flow happens when we develop frameworks or only refer to plugin architecture with areas of code that can be hooked into. In these areas, we might not know (today) how we want it to be used, or we wish to enable developers to add additional functionality. That means that every lifecycle hook in React.js or Angular is a good example of Inversion of Control in practice. IoC is also often explained by the "Hollywood Design Principle": Don't call us, we'll call you.

If you've got a huge app and you don't have a plan for how you'll accomplish dependency injection within in your app, it has potential to get out of hand.

It's for that reason that Inversion of Control (IoC) Containers exist.

They work by requiring you to:

    Create a container (that will hold all of your app dependencies)
    Make that dependency known to the container (specify that it is injectable)
    Resolve the depdendencies that you need by asking the container to inject them

Some of the more popular ones for JavaScript/TypeScript are Awilix and InversifyJS.

Let’s look at an example:
```
class Order{
    constructor(){}
    getInfo(){
        console.log ('This is the order information ')
    }
}

Let order = new order ('New order ');
order.getInfo()
```

The above code is the order management module of a system, and the current function is to output order information.
With the development of business, we need to add evaluation function to the order: allow users to evaluate the order to improve the quality of service.
Very simple requirements, right? Modify the original code slightly and add the evaluation module
```
class Rate{
    star(stars){
        console.log ('your evaluation of the order is% s stars';
    }
}
class Order{
    constructor(){
        this.rate = new Rate();
    }
    //Leave out the rest of the module
}

Let order = new order ('New order ');
order.getInfo();
order.rate.star(5);
```
Let's add a module so we can share the orders now. 
```
Class rate () {/ * * the implementation of evaluation module * /}

class Share(){
    shareTo(platform){
        switch (platform) {
            case 'wxfriend':
                console.log ('share to wechat friends');
                break;
            case 'wxposts':
                console.log ('share to wechat circle of friends');
                break;
            case 'weibo':
                console.log ('share to Weibo ');
                break;
            default:
                console.error ('sharing failed, please check platform ');
                break;
        }
    }
}

class Order{
    constructor(){
        this.rate = new Rate();
        this.share = new Share();
    }
    //Leave out the rest of the module
}

const order = new Order();
order.share.shareTo('wxposts');
```
This time, a new sharing module is added, and then it is introduced into the order module. After rewriting and running the single test, QA needs to test the share module and regression test the order module.

There seems to be something wrong? It can be predicted that the order module is still in the early stage of our product life cycle, and its expansion / upgrade or maintenance will be very frequent in the future. If we modify the main module and dependent module every time, although it can meet the requirements, it is not friendly enough for development and testing: we need double single test (if you have any), smoke, regression… And the business logic and dependency relationship of the production environment are far more complex than those in the example. This method that does not completely conform to the open close principle is easy to generate additional bugs.

As the name suggests, the main behavior of IOC is to invert the control of the module. In the above example, we willOrderIt is called high-level moduleRateandShareIt is called low-level module; high-level module depends on low-level module. IOC reverses this dependency relationship: the high-level module defines the interface, and the low-level module implements the interface; in this way, when we modify or add the low-level module, we will not break the open close principle. It is usually implemented by dependency injection, that is, injecting the low-level module into the high-level module.

Define static attributes in high-level modules to maintain dependencies

```
class Order {
    //Map for maintaining dependencies
    static modules = new Map();
    constructor(){
        for (let module of Order.modules.values()) {
            //Calling module init method
            module.init(this);
        }
    }
    //Injecting modules into dependency map
    static inject(module) {
        Order.modules.set(module.constructor.name, module);
    }
    /**The rest is a little bit*/
}

class Rate{
    init(order) {
        order.rate = this;
    }
    star(stars){
        console.log ('your evaluation of the order is% s stars';
    }
}

const rate = new Rate();
//Injection dependency
Order.inject(rate);
const order = new Order();
order.rate.star(4);
```

In the above example, through theOrderClass to maintain their own dependent modules, while the module implementationinitMethods for referenceOrderCalled when the constructor is initialized. hereOrderIt’s called a container, which encapsulates the dependencies.

In the above example, the implementation of IOC is still slightly cumbersome: the module needs to explicitly declare the init method, and the container needs to display the injection dependency and initialize. We can optimize these business independent contents by encapsulating them into base classes and subclasses for inheritance, or simplify them by decorator methods.

Decorators provide a way for us to add annotations on class declarations and members through metaprogramming syntax. The modifier in JavaScript is currently in the second stage of proposal collection, but it has been supported as an experimental feature in typescript.
