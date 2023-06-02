# Demystifying Creational Patterns: A Roadmap to Effective Object Creation
## Harnessing the Power of Design Patterns for Streamlined and Scalable Object Instantiation

In [my previous article](https://medium.com/r/?url=https%3A%2F%2Fbootcamp.uxdesign.cc%2Ffault-tolerance-design-patterns-in-distributed-systems-49853ad237b4), I spoke about fault tolerance design patterns in distributed systems. In this article, I will talk about the design patterns used for object creation.

> https://bootcamp.uxdesign.cc/fault-tolerance-design-patterns-in-distributed-systems-49853ad237b4

Creational patterns are a subset of design patterns that describes the object creation mechanisms. It helps to create objects in a flexible and efficient way and provides loose coupling between classes. These design patterns provide abstraction in the object creation process, hiding the specific details during object instantiation.

Creational patterns are useful when the object creation is complex, has multiple steps, or requires creating different types of objects based on certain constraints. Creational Patterns aim to make the code maintainable, easier, and extensible.

Some of the creational patterns are
* Factory method pattern
* Builder pattern
* Singleton pattern
* Abstract factory pattern
* Prototype pattern

Each creational pattern has its own requirement and scenarios. We will look at the Factory method pattern and Builder pattern in detail

### Factory Method Pattern

Factory Method Pattern provides an interface for object creation and allows subclasses to decide which class to instantiate. The object creation logic is encapsulated in a separate method called the factory method implemented inside each of the subclasses.

![Factory](https://images.unsplash.com/photo-1578776349090-de61da00ff1a?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1587&q=80)
> Photo by Alex Simpson on Unsplash

The factory method provides loose coupling as the client code depends only on the interface of the factory rather than any specific class. Thus new subclasses can be added without modifying any of the client code as long as it follows the factory method's interface.

```java
// Fruit interface
interface Fruit {
    void display();
}

// Orange class implementing Fruit
class Orange implements Fruit {
    @Override
    public void display() {
        System.out.println("This is an orange.");
    }
}

// Apple class implementing Fruit
class Apple implements Fruit {
    @Override
    public void display() {
        System.out.println("This is a apple.");
    }
}

// Factory class for creating fruits
class FruitFactory {
    public Fruit createFruit(String fruitType) {
        if (fruitType.equalsIgnoreCase("orange")) {
            return new Orange();
        } else if (fruitType.equalsIgnoreCase("apple")) {
            return new Apple();
        }
        // Handle additional fruit types here
        
        return null; // Return null if the requested fruit type is not supported
    }
}

// Game class
class Game {
    private FruitFactory fruitFactory;
    
    public Game() {
        fruitFactory = new FruitFactory();
    }
    
    public void collectFruit(String fruitType) {
        Fruit fruit = fruitFactory.createFruit(fruitType);
        if (fruit != null) {
            System.out.println("Collected a fruit!");
            fruit.display();
        } else {
            System.out.println("Sorry, the requested fruit is not available.");
        }
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        Game game = new Game();
        
        game.collectFruit("apple"); // Output: Collected a fruit! This is an apple.
        game.collectFruit("orange"); // Output: Collected a fruit! This is a orange.
        game.collectFruit("banana"); // Output: Sorry, the requested fruit is not available.
    }
}
```

In the example above the Fruit interface describes the common behaviour of fruits. `Apple` and `Orange` are implementations of the Fruit interface.

`FruitFactory` class is the factory and `createFruit()` is the factory method that takes fruit type as the parameter. It instantiates specific fruit class based on the parameter.

The `Game` class uses the fruit factory to create and collect fruits. The `collectFruit()` method with the specific fruit type as a parameter is responsible for creating and collecting fruits.

In the `main()` method, we create an instance of the Game class and call the `collectFruit()` method. The FruitFactory creates the fruits and displays the fruit.

Thus a Factory Method Pattern helps to instantiate different fruits with the flexibility to add new fruits to the game without modifying the Game class ( client code)

### Builder Pattern

The builder pattern helps to build a complex object step by step. The construction and representation of the object is separated using this pattern and allows to the same process to create different object representations.

![Car factory](https://images.unsplash.com/photo-1631475012097-d074e8a92597?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2070&q=80)
> Photo by Baron on Unsplash

It helps in readabity and maintenance of the code by providing a clear way to get the required properties of the object during instantiation.

```java
// Car class
class Car {
    private String engine;
    private boolean hasSunroof;
    private String transmission;
    private int numWheels;
    private String color;

    
    private Car(Builder builder) {
        this.color = builder.color;
        this.numWheels = builder.numWheels;
        this.hasSunroof = builder.hasSunroof;
        this.engine = builder.engine;
        this.transmission = builder.transmission;
    }
    
    // Getters for the car properties
    public String getTransmission() {
        return transmission;
    }
    
    public boolean hasSunroof() {
        return hasSunroof;
    }

    public String getColor() {
        return color;
    }

    public int getNumWheels() {
        return numWheels;
    }
    
    public String getEngine() {
        return engine;
    }
    
    // Builder class
    static class Builder {
        private String color;
        private boolean hasSunroof;
        private String transmission;
        private int numWheels;
        private String engine;
        
        public Builder setColor(String color) {
            this.color = color;
            return this;
        }

        public Builder setNumWheels(int numWheels) {
            this.numWheels = numWheels;
            return this;
        }

        public Builder setTransmission(String transmission) {
            this.transmission = transmission;
            return this;
        }
        
        public Builder setHasSunroof(boolean hasSunroof) {
            this.hasSunroof = hasSunroof;
            return this;
        }

        public Builder setEngine(String engine) {
            this.engine = engine;
            return this;
        }
        
        public Car build() {
            return new Car(this);
        }
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        Car car = new Car.Builder()
                    .setColor("Black")
                    .setEngine("V8")
                    .setTransmission("Manual")
                    .setNumWheels(4)
                    .setHasSunroof(true)
                    .build();
                    
        System.out.println("Color: " + car.getColor());
        System.out.println("Engine: " + car.getEngine());
        System.out.println("Transmission: " + car.getTransmission());
        System.out.println("Number of Wheels: " + car.getNumWheels());
        System.out.println("Has Sunroof: " + car.hasSunroof());
    }
}
```
In the example above the `Car.Builder` class acts as the builder for the car construction. The car has different properties like color, engine, transmission, number of wheels and sunroof.

The `Car` class has a private constructor with the builder object as it parameter. It then constructs the car based on the builder values
The builder class providers the setter methods for the desired car properties and these methods return the builder object to support method chaining
In the `main()` method, an instance of the car is created with the desired properties using the builder and `build()` method is called to get the constructed car object.

This example shows how builder pattern can be used to construct a object with the desired properties using a flexible and readable manner. It allows the client code to configure the desired properties while ignoring other properties. 

Hope the above explanations and implementation examples gives you a clarity on the creational designer pattern. Do note that each creational design pattern has its own set of requirements and constraints. Happy creating objects !!!



