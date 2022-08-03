# SOLID
Just Learning SOLID principles.

# S 
## SRP SIngle Responsibility Principle

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
