## Flutter Naming Conventions Guidance

In Flutter projects, following a consistent and meaningful naming convention is crucial for code readability and maintainability. This guide provides a set of commonly accepted naming conventions for Flutter projects.

### Package Names

Package names should be written in lowercase, and words should be separated by underscores (_).

Example: `com.example.my_flutter_app`

### Project Names

Project names should be written in lowercase, and words should be separated by spaces.

Example: `my_flutter_app`

### File Names

Dart files and other files should be written in lowercase, and words should be separated by underscores (_). File names should be descriptive enough to convey their content.

Examples:

- `user_profile.dart`
- `constants.dart`

### Class and Function Names

Class and function names should follow the "camelCase" convention. Class names should start with an uppercase letter and be descriptive of their purpose.

Examples:

```dart
class User {
  // class implementation
}

void fetchUserData() {
  // function implementation
}
```

### Variable Names

Variable names should follow the "camelCase" convention and should be descriptive and meaningful.

Examples:

```dart
String userName = 'John Doe';
int userAge = 30;
```

### Constant Names

Constants should be written in uppercase letters and words should be separated by underscores (_).

Examples:

```dart
const int MAX_LENGTH = 100;
const int DEFAULT_TIMEOUT = 5000;
```

### Private Variable and Method Names

Private variables and methods should be written with an underscore (_) followed by "camelCase."

Example:

```dart
String _privateVariable = 'private data';

void _privateMethod() {
  // method implementation
}
```

### Boolean Variables

Boolean variables should have names that convey a positive meaning if true and a negative meaning if false.

Example:

```dart
bool isLoading = true;
bool hasError = false;
```

### Argument Names

Function arguments should have descriptive and meaningful names.

Example:

```dart
void calculateArea(double width, double height) {
  // function implementation
}
```

### Widget Names

Custom widgets should have descriptive names that clearly convey their purpose and functionality.

Example:

```dart
class CustomButton extends StatelessWidget {
  // widget implementation
}
```

### Enum Names

Enum tipleri, büyük harfle başlamalı ve "camelCase" kullanılmalıdır. Enum değerleri de büyük harfle yazılmalıdır.

```dart
enum Color {
  RED,
  GREEN,
  BLUE,
}
```

### Constant Names with Prefix

When declaring constants in a class, use a prefix to indicate their purpose or category.

```dart
class APIEndpoints {
  static const String BASE_URL = "https://api.example.com/";
  static const String AUTH_URL = "https://api.example.com/auth";
}
```

### Constructor Names

Constructors should use "camelCase" with the class name in lowercase.

```dart
class Person {
  String name;
  
  Person(this.name);
}
```

### Factory Constructor Names

Factory constructors should be named appropriately and use "camelCase."

```dart
class APIResponse {
  // factory constructor
  factory APIResponse.fromJson(Map<String, dynamic> json) {
    // create object from JSON data
  }
}
```

### Named Constructor Names

Named constructors should be descriptive and use "camelCase."

```dart
class Rectangle {
  int width;
  int height;
  
  Rectangle(this.width, this.height);
  
  // named constructor
  Rectangle.square(int sideLength) : width = sideLength, height = sideLength;
}
```

### Setters and Getters

Use "camelCase" for getter and setter methods, with a prefix like "get" and "set."

```dart
class Person {
  String _name;
  
  // setter
  void set name(String newName) {
    _name = newName;
  }
  
  // getter
  String get name {
    return _name;
  }
}
```

### Boolean Getter Names

Boolean getter methods should be descriptive and start with "is" or "has."

```dart
class NetworkResponse {
  bool _hasError = false;
  
  bool get hasError {
    return _hasError;
  }
}
```

### Widget Constructor Arguments

When creating custom widgets, constructor arguments should be descriptive and follow "camelCase."

```dart
class CustomButton extends StatelessWidget {
  final String text;
  final VoidCallback onPressed;
  
  CustomButton({required this.text, required this.onPressed});
  
  // Widget implementation
}
```

### Utility Class Names

Utility classes should have descriptive names that indicate their purpose.

```dart
class DateUtils {
  static DateTime addDays(DateTime date, int days) {
    // implementation
  }
  
  static DateTime subtractDays(DateTime date, int days) {
    // implementation
  }
}
```

### Widget Key Names

When using widget keys, use descriptive names that represent their purpose.

```dart
class MyWidget extends StatelessWidget {
  final Key loginButtonKey = Key('login_button');
  final Key signUpButtonKey = Key('signup_button');
  
  // Widget implementation
}
```

### Constant Values in Switch Statements

When using constants in switch statements, capitalize the constant names.

```dart
enum Day {
  MONDAY,
  TUESDAY,
  WEDNESDAY,
  // ...
}

void printDay(Day day) {
  switch (day) {
    case Day.MONDAY:
      print('Today is Monday');
      break;
    case Day.TUESDAY:
      print('Today is Tuesday');
      break;
    // ...
  }
}
---


### Abbreviations

Avoid using confusing or obscure abbreviations whenever possible. Names should be clear and easy to understand.

### Logical Grouping

Organize files, classes, and variables logically to keep related code together and maintain a clean project structure.

---

This guide provides a general set of naming conventions for Flutter projects. Remember that each project may have specific requirements, so it's essential to adapt these conventions to suit the project's needs. Consistent and meaningful naming practices will improve code collaboration and make your Flutter project more maintainable.
```
