# SOLID Principle

## Single Responsibility Principle (SRP) 

A class should have only one reason to change. In other words, a class should have only one responsibility. This principle helps to create classes that are easy to understand, modify, and maintain.

For example, consider a class called Order that handles both the order details and payment processing. This violates the SRP as it has two responsibilities. It would be better to separate payment processing into a separate class.

Another example, suppose you have a class called Email that is responsible for sending emails. However, this class is doing more than one responsibility - it's not only responsible for sending emails, but also for composing and formatting the email message. This violates the SRP.

To apply the SRP, you should:
- Identify the responsibilities of a class or module
- Separate them into different classes or modules
- Avoid creating classes or modules that are too large or complex

```
// Before applying SRP
public class Order {
    private List<Item> items;
    private Customer customer;
    private double totalPrice;
    private Payment payment;
    private Shipping shipping;
    
    // methods for adding and removing items, calculating total price, handling payment, and shipping
}

// After applying SRP
public class Order {
    private List<Item> items;
    private Customer customer;
    private double totalPrice;
    
    // methods for adding and removing items, calculating total price
}

public class PaymentProcessor {
    private Payment payment;
    
    // methods for handling payment
}

public class ShippingProcessor {
    private Shipping shipping;
    
    // methods for handling shipping
}
```
In the example above, the original Order class had multiple responsibilities, including handling items, calculating total price, handling payment, and shipping. After applying SRP, the Order class only handles the responsibility of managing the order items and calculating the total price, while the `PaymentProcessor` and `ShippingProcessor` classes handle the responsibilities of handling payment and shipping, respectively.



## Open-Closed Principle (OCP)

A software entity should be open for extension but closed for modification. This principle encourages the use of interfaces, abstract classes, and inheritance to allow new functionality to be added without changing existing code.

Example: Consider a class called `Product` that has a method to calculate the price. If a new discount is introduced, you shouldn't modify the existing Product class. Instead, you can create a new class that extends Product and overrides the price calculation method to apply the discount.

To apply the OCP, you should:
- Use abstraction to define contracts that can be extended
- Avoid modifying existing code when adding new functionality
- Use inheritance or composition to extend functionality

Example 1:
```
public class Product {
    private double price;
    
    public Product(double price) {
        this.price = price;
    }
    
    public double calculatePrice() {
        return price;
    }
}

public class DiscountedProduct extends Product {
    private double discount;
    
    public DiscountedProduct(double price, double discount) {
        super(price);
        this.discount = discount;
    }
    
    @Override
    public double calculatePrice() {
        double discountedPrice = super.calculatePrice() * (1 - discount);
        return discountedPrice;
    }
}
```

Example 2:
```
// Before applying OCP
public class Shape {
    private String type;
    private double size;
    
    public void draw() {
        if (type.equals("circle")) {
            drawCircle(size);
        } else if (type.equals("square")) {
            drawSquare(size);
        } else if (type.equals("triangle")) {
            drawTriangle(size);
        }
    }
    
    // methods for drawing a circle, square, and triangle
}

// After applying OCP
public abstract class Shape {
    private double size;
    
    public Shape(double size) {
        this.size = size;
    }
    
    public abstract void draw();
}

public class Circle extends Shape {
    public Circle(double size) {
        super(size);
    }
    
    @Override
    public void draw() {
        drawCircle(size);
    }
}

public class Square extends Shape {
    public Square(double size) {
        super(size);
    }
    
    @Override
    public void draw() {
        drawSquare(size);
    }
}

public class Triangle extends Shape {
    public Triangle(double size) {
        super(size);
    }
    
    @Override
    public void draw() {
        drawTriangle(size);
    }
}
```

In the example above, the original Shape class had a draw() method that handled different types of shapes using conditional statements. After applying OCP, the Shape class was made abstract, and each shape type was implemented as its own class that extends the Shape class and overrides the draw() method. This allows for easy extension by adding new shapes, without modifying the existing Shape class.



## Liskov Substitution Principle (LSP)

Subtypes should be substitutable for their base types. This means that objects of a subclass should be able to replace objects of the parent class without affecting the correctness of the program. This principle helps to ensure that inheritance is used correctly.

Example: Consider a class hierarchy with Animal as the parent class and Dog and Cat as the subclasses. If you have a method that takes an Animal object as a parameter, you should be able to pass a Dog or Cat object without any issues.

To apply the LSP, you should:
- Ensure that subclasses conform to the contract of the parent class
- Avoid changing the behavior of the parent class in the subclasses
- Use interfaces to define contracts that can be implemented by different classes

Example 1:
```
public class Animal {
    public String makeSound() {
        return "Generic animal sound";
    }
}
```

```
public class Dog extends Animal {
    @Override
    public String makeSound() {
        return "Bark";
    }
}
```

```
public class Cat extends Animal {
    @Override
    public String makeSound() {
        return "Meow";
    }
}
```

```
public class AnimalSound {
    public void animalSound(Animal animal) {
        System.out.println(animal.makeSound());
    }
}
```

According to the Liskov Substitution Principle, we should be able to pass any subclass of Animal to this method without any issues. So we can pass a Dog or a Cat object to this method and it should print the correct sound:

```
AnimalSound animalSound = new AnimalSound();

Animal animal = new Animal();
Dog dog = new Dog();
Cat cat = new Cat();

animalSound.animalSound(animal); // prints "Generic animal sound"
animalSound.animalSound(dog);    // prints "Bark"
animalSound.animalSound(cat);    // prints "Meow"
```

Example 2:
```
public class Rectangle {
    protected int width;
    protected int height;

    public void setWidth(int width) {
        this.width = width;
    }

    public void setHeight(int height) {
        this.height = height;
    }

    public int getWidth() {
        return width;
    }

    public int getHeight() {
        return height;
    }

    public int getArea() {
        return width * height;
    }
}

public class Square extends Rectangle {
    public void setWidth(int width) {
        this.width = width;
        this.height = width;
    }

    public void setHeight(int height) {
        this.width = height;
        this.height = height;
    }
}

public class LSPDemo {
    public int getArea(Rectangle r) {
        return r.getArea();
    }
}

public class Main {
    public static void main(String[] args) {
        LSPDemo demo = new LSPDemo();
        Rectangle rect = new Rectangle();
        rect.setWidth(5);
        rect.setHeight(10);
        System.out.println("Rectangle area: " + demo.getArea(rect)); // Output: Rectangle area: 50

        Square square = new Square();
        square.setWidth(5);
        square.setHeight(10);
        System.out.println("Square area: " + demo.getArea(square)); // Output: Square area: 50
    }
}
```
In this example, the Square class extends the Rectangle class. However, it violates the LSP because when the setWidth or setHeight methods are called on a Square object, it changes both the width and height attributes, whereas in a Rectangle object, these methods only change the corresponding attribute. When we pass a Square object to the getArea method in LSPDemo, it returns an incorrect value because the Square object doesn't behave like a proper Rectangle. To adhere to the LSP, we should not have a Square class that extends Rectangle, since a square is not a rectangle.



## Interface Segregation Principle (ISP)

A client should not be forced to implement an interface that it doesn't use. This principle encourages the creation of small, focused interfaces that are specific to the needs of the client.

Example: Consider a class called Printer that has methods to print, scan, and fax. If a client only needs to print, it shouldn't be forced to implement the scan and fax methods as well. It would be better to separate the interface of Printer into smaller interfaces for printing, scanning, and faxing.

To apply the ISP, you should:
- Identify the methods that are used by clients
- Separate the interface of a class into smaller, more specific interfaces
- Avoid creating interfaces that are too large or complex
```
// Bad example
public interface Animal {
    void walk();
    void run();
    void swim();
    void fly();
}

public class Dog implements Animal {
    @Override
    public void walk() {
        System.out.println("Dog is walking");
    }
    @Override
    public void run() {
        System.out.println("Dog is running");
    }
    @Override
    public void swim() {
        System.out.println("Dog is swimming");
    }
    @Override
    public void fly() {
        // This method is not applicable to Dog
        throw new UnsupportedOperationException();
    }
}
```

```
// Good example
public interface Animal {
    void walk();
    void run();
}

public interface Swimmable {
    void swim();
}

public interface Flyable {
    void fly();
}

public class Dog implements Animal, Swimmable {
    @Override
    public void walk() {
        System.out.println("Dog is walking");
    }
    @Override
    public void run() {
        System.out.println("Dog is running");
    }
    @Override
    public void swim() {
        System.out.println("Dog is swimming");
    }
}

public class Bird implements Animal, Flyable {
    @Override
    public void walk() {
        System.out.println("Bird is walking");
    }
    @Override
    public void run() {
        System.out.println("Bird is running");
    }
    @Override
    public void fly() {
        System.out.println("Bird is flying");
    }
}
```
In this example, we have separated the abilities of Animal into separate interfaces, and implemented only the relevant interfaces in each class. This makes the code more maintainable and prevents code bloat.


## Dependency Inversion Principle (DIP) 

High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions. This principle encourages the use of dependency injection, inversion of control, and the creation of loosely coupled components.

To follow the DIP, you should:
- Use abstractions to decouple high-level and low-level modules.
- Avoid depending on concrete implementations and instead depend on abstractions.
- Ensure that abstractions do not depend on details. Details should depend on abstractions.


Example: Consider a class called OrderService that depends on a low-level class called Database. This violates the DIP as the high-level module depends on the low-level module. It would be better to define an interface or abstract class for the database and use it to define the contract between the OrderService and
```
public interface UserRepository {
    void save(User user);
}

public class DatabaseUserRepository implements UserRepository {
    public void save(User user) {
        // save user to database
    }
}

public class UserService {
    private UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public void save(User user) {
        userRepository.save(user);
    }
}
```
In the example above, the UserService class depends on the UserRepository interface, which is an abstraction. We have provided a concrete implementation of the UserRepository interface called DatabaseUserRepository that deals with the details of how to save the user to the database.

By using the Dependency Inversion Principle, we can easily swap out the DatabaseUserRepository with another implementation without changing the UserService class. This makes the code more flexible and easier to maintain.





Overall, the SOLID principles help to create code that is easier to understand, modify, and maintain. They encourage the use of good design practices such as encapsulation, abstraction, and separation of concerns. By following these principles, developers can create code that is more robust, flexible, and reusable.
