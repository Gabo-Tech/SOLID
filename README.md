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
