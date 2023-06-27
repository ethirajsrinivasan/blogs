## From Blueprint to Reality: Unleashing the Potential of Creational Patterns
A Comprehensive Exploration of Design Patterns for Crafting Objects with Precision and Elegance

![Factory](https://images.unsplash.com/photo-1585252155261-cff31944d781)
> Photo by Andreas Felske on Unsplash

In my previous article [Demystifying Creational Patterns: A Roadmap to Effective Object Creation](https://ethigeek.com/blogs/demystifying-creational-patterns-a-roadmap-to-effective-object-creation.html), I stated the need for creational patterns and illustrated them using two creational patterns: Factory Method Pattern and Builder Pattern. So in short creational patterns focus on creating objects in a way that promotes encapsulation, decoupling, and flexibility.

> https://ethigeek.com/blogs/demystifying-creational-patterns-a-roadmap-to-effective-object-creation.html

In this article, we will look at the following two design patterns in detail
* Singleton pattern
* Prototype pattern

### Singleton Pattern
Singleton pattern are used to make sure that only one instance of a class is created and it provides global access to it. It uses a private constructor to prevent direct object instantiation and the static method provides access to the single instance.

![Same User behind every door](https://github.com/ethirajsrinivasan/blogs/assets/7569031/0e2a3c35-4501-4455-9cf2-063e509a90e0)
> Singleton Class (Same User behind every door)

Lets us look at an example:

```java
public class Logger {
    private static Logger instance;
    
    // Private constructor to prevent direct instantiation
    private Logger() {
        // Initialization code, if needed
    }
    
    // Static method to provide access to the single instance
    public static Logger getInstance() {
        if (instance == null) {
            instance = new Logger();
        }
        return instance;
    }
    
    // Method to log a message
    public void log(String message) {
        System.out.println("Logging message: " + message);
        // Log message implementation goes here
    }
}
```
In the example above Logger class has a private constructor `Logger` and `getInstance` method which provides a single instance of the class.

```java
public class Main {
    public static void main(String[] args) {
        Logger logger = Logger.getInstance();
        logger.log("An important message");

        // Trying to create a new instance will give us the existing instance
        Logger anotherLogger = Logger.getInstance();
        anotherLogger.log("Another message");

        // Output:
        // Logging message: An important message
        // Logging message: Another message
    }
}
```

The above example states that only one instance of the class gets created regardless of the number of times `getInstance()` is called. 

The singleton pattern ensures that all the components share the same logger instance. This helps in centralized logging and ensures all messages are recorded by the same logger object. 

Thus singleton pattern is used when one instance of an object needs to be created and has to be shared across other components.

### Prototype pattern

Prototype pattern allows the creation of objects by cloning existing instances thereby reducing the need to create new instances from scratch. It has a prototype object and uses that object to create new objects by cloning. It is used when new object creation is expensive and complex.

![Same type of scooter in production line](https://images.unsplash.com/photo-1599486858190-a56a25d4616b)
> Photo by Kumpan Electric on Unsplash

Let's consider a scenario where we have a design application that can create and customize shapes. Prototype pattern is used to clone the existing shape objects and modify them as per requirement instead of creating each shape object from scratch.

```java
// Prototype interface
public interface Shape extends Cloneable {
    void draw();
    Shape clone();
}

// Concrete implementation of Shape
public class Circle implements Shape {
    private int radius;

    public Circle(int radius) {
        this.radius = radius;
    }

    @Override
    public void draw() {
        System.out.println("Drawing a circle with radius " + radius);
    }

    @Override
    public Shape clone() {
        try {
            return (Shape) super.clone();
        } catch (CloneNotSupportedException e) {
            return null;
        }
    }
}

// Client code
public class GraphicDesignApp {
    public static void main(String[] args) {
        // Create a prototype circle
        Circle circlePrototype = new Circle(10);

        // Clone the circle and customize
        Shape circle1 = circlePrototype.clone();
        circle1.draw();

        ((Circle) circle1).setRadius(15);
        circle1.draw();

        // Clone the circle again and customize
        Shape circle2 = circlePrototype.clone();
        ((Circle) circle2).setRadius(8);
        circle2.draw();
    }
}
```

The `shape` interface has the `clone` and `draw` method. The `circle` class implements the interface and has its modification of the `radius` field.

In the `GraphicDesignApp` `circlePrototype` object is created and new circle objects are created by cloning and modifying the radius and then `draw()` is called to visualize it. 

Do note that `clone()` in Java performs a shallow copy. If there are mutable fields a deep copy must be performed to create a cloned object.

Thus prototype pattern is useful when creating complex objects and each object requires a variation. It makes use of the existing object to reduce the overhead of object creation.

Do note that each creational design pattern has its own set of requirements and constraints.Hope the above explanations and implementation examples give you clarity on the creational designer pattern.Happy creating objects !!!






