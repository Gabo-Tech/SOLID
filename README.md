# SOLID
Just Learning SOLID principles.

# S 
## SRP Single Responsibility Principle

>"Every software component should have one and only one reponsibility".



### Cohesion

>"Cohesion is the degree to which the varios parts of a software component are related". 

(Break down stuff.)

>"Higher Cohesion helps attain better adherence to the Single Responsibility Principle".



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

### Open for extension

A software component should be extendable to add a new feature or to add a new behavior to it.

### Closed for modification

New features getting added to the software component, should NOT have to modify existing code.

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

This can be solved, using polymorphism, an if statement in the Rectangle class, or a variety of other methods. But the real cause of the issue is that Square is not a good child class of Rectangle, and that in reality, perhaps both shapes should inherit from a Shape class instead.



### Tell, don't ask. 

Lot's of times you don't need to check for things just act upon them directly. 



# I 
## ISP Interface Segregation Principle

>"No client should be forced to depend on methods it does not use".

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
