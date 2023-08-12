
# Dart Programming Guidance

Welcome to the Dart Programming Guidance. This Guidance aims to provide step-by-step explanations and code examples for those interested in learning the Dart programming language. Below, you will find essential topics to get started with Dart programming.

## 1. Introduction and Basics

### What is Dart?

Dart is an open-source programming language developed by Google. It can be used for both web and mobile application development.

### Features of Dart

- Supports object-oriented programming.
- Has a static type system, where variable types are fixed.
- Includes a fast virtual machine.

### Applications of Dart

- Developing mobile applications with the Flutter framework.
- Building web applications.
- Creating server-side applications.

## 2. Environment and Setup

### Dart Installation

To install Dart on your computer, follow these steps:

1. Download the Dart SDK from the [official website](https://dart.dev/get-dart) and initiate the installation.
2. Follow the installation steps to install Dart on your computer.

### Dart Development Environments

- You can use Dart in popular IDEs like VS Code, Android Studio.
- You can also develop Dart code using the `dart` command from the terminal.

### Your First Dart Application

An example "Hello World" application:

```dart
void main() {
  print("Hello, World!");
}
```

This code snippet prints "Hello, World!" to the screen.

## 3. Basic Syntax and Data Types

### Variables and Data Types

In Dart, variables are associated with a specific data type. Examples:

```dart
int number = 42;
String name = "John";
bool isTrue = true;
```

Adding an item to a list:

```dart
fruits.add("Strawberry");
```

Getting the length of a list:

```dart
int length = fruits.length;
```

### Maps and Collections

Maps are used to store key-value pairs:

```dart
Map<String, int> ages = {
  "John": 25,
  "Jane": 30,
  "Alice": 28,
};
```

Adding a new key-value pair:

```dart
ages["Bob"] = 22;
```

Getting the value for a specific key:

```dart
int age = ages["John"];
```

## 4. Control Flow and Loops

### if-else Statements

Using conditional statements to execute code based on conditions:

```dart
if (condition) {
  // Execute this block if the condition is true.
} else {
  // Execute this block if the condition is false.
}
```

### switch-case Statements

Using switch-case to handle multiple conditions:

```dart
switch (variable) {
  case value1:
    // Code for value1.
    break;
  case value2:
    // Code for value2.
    break;
  default:
    // Code for other cases.
}
```

### for Loops

Using for loops to iterate over a range of values:

```dart
for (int i = 0; i < 5; i++) {
  print("Loop iteration: $i");
}
```

### while and do-while Loops

Using while and do-while loops to repeat actions based on conditions:

```dart
while (condition) {
  // Execute this block as long as the condition is true.
}

do {
  // Execute this block at least once, then check the condition.
} while (condition);
```

## 5. Functions

### Function Definition and Invocation

Functions are blocks of code that perform specific tasks. Defining and invoking functions:

```dart
void greet() {
  print("Hello!");
}

// Invoking the function:
greet();
```

### Parameters and Arguments

Passing data to functions using parameters and arguments:

```dart
void sayHello(String name) {
  print("Hello, $name!");
}

// Calling the function with an argument:
sayHello("John");
```

### Nested Functions

Defining functions within functions:

```dart
void outerFunction() {
  void innerFunction() {
    print("Inner function executed");
  }

  print("Outer function executed");
  innerFunction();
}

outerFunction();
```

### Anonymous Functions

Using anonymous functions for inline operations:

```dart
var add = (int x, int y) {
  return x + y;
};

print(add(5, 3)); // 8
```

## 6. Object-Oriented Programming (OOP)

### Classes and Objects

In Dart, classes are used to create objects. An example of a student class:

```dart
class Student {
  String name;
  int age;

  Student(this.name, this.age);

  void greet() {
    print("Hello, I'm $name, $age years old.");
  }
}

void main() {
  var student = Student("John", 25);
  student.greet(); // Hello, I'm John, 25 years old.
}
```

### Constructor Methods

Special methods that are called when an object is created:

```dart
class Car {
  String brand;
  String model;

  Car(this.brand, this.model);

  void showInfo() {
    print("Brand: $brand, Model: $model");
  }
}

void main() {
  var car = Car("Toyota", "Corolla");
  car.showInfo(); // Brand: Toyota, Model: Corolla
}
```

### Inheritance and Subclasses

Using inheritance to reuse properties and methods from another class:

```dart
class Product {
  String name;
  double price;

  Product(this.name, this.price);

  void showInfo() {
    print("Product: $name, Price: $price");
  }
}

class Phone extends Product {
  String brand;

  Phone

(String name, double price, this.brand) : super(name, price);

  @override
  void showInfo() {
    print("Phone: $brand $name, Price: $price");
  }
}

void main() {
  var phone = Phone("iPhone", 800, "Apple");
  phone.showInfo(); // Phone: Apple iPhone, Price: 800
}
```

### Encapsulation

Controlling access to class properties:

```dart
class Person {
  String _name; // Property is hidden using an underscore.
  int _age;

  Person(this._name, this._age);

  String get name => _name; // Only read access.
  set age(int newAge) {
    if (newAge > 0) {
      _age = newAge;
    }
  }

  void showInfo() {
    print("Name: $_name, Age: $_age");
  }
}

void main() {
  var person = Person("John", 25);
  person.showInfo(); // Name: John, Age: 25
  print(person.name);   // John
  person.age = 30;
  person.showInfo(); // Name: John, Age: 30
}
```

## 7. Collections

### Lists

Using lists to store and manage multiple items:

```dart
List<int> numbers = [1, 2, 3, 4, 5];
List<String> fruits = ["Apple", "Orange", "Banana"];
```

Adding an item to a list:

```dart
fruits.add("Strawberry");
```

Getting the length of a list:

```dart
int length = fruits.length;
```

### Maps

Using maps to store key-value pairs:

```dart
Map<String, int> ages = {
  "John": 25,
  "Jane": 30,
  "Alice": 28,
};
```

Adding a new key-value pair:

```dart
ages["Bob"] = 22;
```

Getting the value for a specific key:

```dart
int age = ages["John"];
```

## 8. Error Handling

### Exceptions

Managing errors using the exception mechanism in Dart:

```dart
try {
  // Code that might throw an exception
} catch (e) {
  // Code executed when an exception is caught
} finally {
  // Code executed regardless of whether an exception occurred or not
}
```

### Debugging

Using `print` statements for debugging purposes:

```dart
print("This is a debugging message.");
```

Additionally, you can use the `debugger` statement to add breakpoints for advanced debugging.

---

## Real-Life Scenario: Banking Application

Imagine you're building a banking application using Dart. Here's a simplified example of how you might structure your code for managing customer accounts:

```dart
class Customer {
  String name;
  double balance;

  Customer(this.name, this.balance);

  void deposit(double amount) {
    balance += amount;
    print("$name deposited $$amount. New balance: $$balance");
  }

  void withdraw(double amount) {
    if (balance >= amount) {
      balance -= amount;
      print("$name withdrew $$amount. New balance: $$balance");
    } else {
      print("Insufficient funds for $name.");
    }
  }

  void displayBalance() {
    print("Current balance for $name: $$balance");
  }
}

void main() {
  var customer1 = Customer("John Doe", 1000);
  var customer2 = Customer("Jane Smith", 500);

  customer1.deposit(300);
  customer1.withdraw(200);
  customer1.displayBalance();

  customer2.withdraw(700);
  customer2.displayBalance();
}
```

In this scenario, the `Customer` class represents bank customers with their names and account balances. The methods within the class allow customers to deposit, withdraw, and check their account balances.

---

This concludes our Dart Programming Guidance. You have covered essential topics and gained insights into real-life scenarios. Feel free to expand upon this Guidance and explore more advanced topics as you continue your journey in Dart programming. Happy coding!
