Great! Now, let’s move on to Object-Oriented PHP (OOP). Understanding OOP is crucial because Laravel heavily relies on it.

Step 1: Introduction to OOP in PHP

Object-Oriented Programming (OOP) is a paradigm where you organize your code using classes and objects.

Key Concepts of OOP
	1.	Class: A blueprint for creating objects.
	2.	Object: An instance of a class.
	3.	Properties: Variables within a class.
	4.	Methods: Functions within a class.
	5.	Encapsulation: Restricting direct access to some of an object’s components.
	6.	Inheritance: A class can inherit properties and methods from another class.
	7.	Polymorphism: Using the same method in different ways.

Step 2: Basic Syntax

Defining and Using a Class
	1.	Define a class:

<?php
class Car {
    // Properties
    public $brand;
    public $color;

    // Constructor
    public function __construct($brand, $color) {
        $this->brand = $brand;
        $this->color = $color;
    }

    // Method
    public function drive() {
        echo "The $this->color $this->brand is driving.";
    }
}

// Create an object
$myCar = new Car("Toyota", "red");

// Access properties and methods
echo $myCar->brand; // Output: Toyota
$myCar->drive();    // Output: The red Toyota is driving.
?>



Access Modifiers
	•	public: Accessible from anywhere.
	•	protected: Accessible within the class and its child classes.
	•	private: Accessible only within the class.

Example:

<?php
class Person {
    private $name;

    public function setName($name) {
        $this->name = $name;
    }

    public function getName() {
        return $this->name;
    }
}

$person = new Person();
$person->setName("John");
echo $person->getName(); // Output: John
?>

Step 3: Inheritance

What is Inheritance?

Inheritance allows a class to use properties and methods of another class.

Example:

<?php
class Animal {
    public function makeSound() {
        echo "Some generic sound";
    }
}

class Dog extends Animal {
    public function makeSound() {
        echo "Bark!";
    }
}

$dog = new Dog();
$dog->makeSound(); // Output: Bark!
?>

Step 4: Polymorphism

Polymorphism allows a method to behave differently based on the object.

Example:

<?php
class Shape {
    public function area() {
        return 0;
    }
}

class Circle extends Shape {
    private $radius;

    public function __construct($radius) {
        $this->radius = $radius;
    }

    public function area() {
        return pi() * pow($this->radius, 2);
    }
}

class Rectangle extends Shape {
    private $width;
    private $height;

    public function __construct($width, $height) {
        $this->width = $width;
        $this->height = $height;
    }

    public function area() {
        return $this->width * $this->height;
    }
}

$circle = new Circle(5);
echo $circle->area(); // Output: Area of the circle

$rectangle = new Rectangle(4, 6);
echo $rectangle->area(); // Output: Area of the rectangle
?>

Step 5: Practice Exercises
	1.	Define a Person Class:
	•	Properties: name, age.
	•	Methods: A constructor to initialize the properties and a method to display “Hello, my name is [name], and I am [age] years old.”
	2.	Create a Vehicle Class:
	•	Create two child classes: Car and Bike.
	•	Add a method move() in the parent class and override it in child classes to display different messages (e.g., “Car is moving fast”, “Bike is moving”).

Let me know once you’ve tried these, and we’ll proceed to Intermediate Laravel!