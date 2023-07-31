
# Flutter Widget Guidance

Welcome to the Flutter Widget Guide! In this guide, you will find examples of various widgets along with their use cases. Flutter is a powerful UI toolkit, and understanding different widgets and their purposes is essential for building great mobile applications.

## Table of Contents

- [Basic Widgets](#basic-widgets)
- [Text and Images](#text-and-images)
- [Input and Form Widgets](#input-and-form-widgets)
- [List Widgets](#list-widgets)
- [Navigation and Routing Widgets](#navigation-and-routing-widgets)
- [Animation Widgets](#animation-widgets)
- [Layout Widgets](#layout-widgets)
- [Image and Icon Handling](#image-and-icon-handling)
- [Material Design Widgets](#material-design-widgets)
- [Cupertino Design Widgets](#cupertino-design-widgets)
- [Other Important Widgets](#other-important-widgets)

## Basic Widgets

### Container

Purpose: A box that can contain other widgets and style them using properties like color, padding, margin, and borders.

```dart
Container(
  color: Colors.blue,
  padding: EdgeInsets.all(16),
  margin: EdgeInsets.symmetric(vertical: 8),
  child: Text('Hello, Flutter!'),
)
```

### Row

Purpose: A widget that arranges its children widgets in a horizontal line.

```dart
Row(
  children: [
    Icon(Icons.star),
    Text('Flutter'),
  ],
)
```

### Column

Purpose: A widget that arranges its children widgets in a vertical line.

```dart
Column(
  children: [
    Text('Hello'),
    Text('Flutter'),
  ],
)
```

### Center

Purpose: A widget that centers its child widget within itself.

```dart
Center(
  child: Text('Hello, Flutter!'),
)
```

### Expanded

Purpose: A widget that expands to fill the available space within a parent widget.

```dart
Row(
  children: [
    Expanded(
      child: Container(
        color: Colors.blue,
        height: 50,
      ),
    ),
    Expanded(
      child: Container(
        color: Colors.green,
        height: 50,
      ),
    ),
  ],
)
```

### Padding

Purpose: A widget that adds padding around its child widget.

```dart
Padding(
  padding: EdgeInsets.all(16),
  child: Text('Hello, Flutter!'),
)
```

### ListView

Purpose: A scrollable list of widgets that can be displayed vertically or horizontally.

```dart
ListView(
  children: [
    ListTile(title: Text('Item 1')),
    ListTile(title: Text('Item 2')),
    ListTile(title: Text('Item 3')),
  ],
)
```

### GridView

Purpose: A scrollable grid of widgets that can be displayed in a 2D layout.

```dart
GridView.count(
  crossAxisCount: 2,
  children: [
    Container(color: Colors.red),
    Container(color: Colors.blue),
    Container(color: Colors.green),
    Container(color: Colors.yellow),
  ],
)
```

### Stack

Purpose: A widget that overlays its children widgets on top of each other.

```dart
Stack(
  children: [
    Container(color: Colors.blue, height: 200, width: 200),
    Positioned(
      top: 50,
      left: 50,
      child: Container(color: Colors.red, height: 100, width: 100),
    ),
  ],
)
```

### Positioned

Purpose: A widget that positions its child widget at a specific position within a Stack.

```dart
Stack(
  children: [
    Container(color: Colors.blue, height: 200, width: 200),
    Positioned(
      top: 50,
      left: 50,
      child: Container(color: Colors.red, height: 100, width: 100),
    ),
  ],
)
```

## Text and Images

### Text

Purpose: A widget that displays a piece of text.

```dart
Text('Hello, Flutter!')
```

### RichText

Purpose: A widget that displays a paragraph of mixed-style text.

```dart
RichText(
  text: TextSpan(
    text: 'Hello',
    style: DefaultTextStyle.of(context).style,
    children: <TextSpan>[
      TextSpan(text: ' Flutter!', style: TextStyle(fontWeight: FontWeight.bold)),
    ],
  ),
)
```

### Image

Purpose: A widget that displays an image from the local assets or the internet.

```dart
Image.asset('assets/image.png')
```

### Icon

Purpose: A widget that displays a material design icon.

```dart
Icon(Icons.star)
```

### Placeholder

Purpose: A widget that displays a box with a specified width, height, and color.

```dart
Placeholder(
  color: Colors.blue,
  fallbackWidth: 100,
  fallbackHeight: 100,
)
```

### Tooltip

Purpose: A widget that shows a short message when the user long-presses on it.

```dart
Tooltip(
  message: 'This is a tooltip',
  child: IconButton(
    icon: Icon(Icons.info),
    onPressed: () {},
  ),
)
```

## Input and Form Widgets

### TextField

Purpose: A widget that allows users to enter text.

```dart
TextField(
  decoration: InputDecoration(
    labelText: 'Enter your name',
  ),
)
```

### TextFormField

Purpose: A more advanced version of TextField that is used within a Form widget.

```dart
TextFormField(
  decoration: InputDecoration(
    labelText: 'Enter your email',
  ),
  validator: (value) {
    if (value.isEmpty) {
      return 'Please enter your email';
    }
    return null;
  },
)
```

### RaisedButton

Purpose: A widget that displays a button with a raised appearance.

```dart
RaisedButton(
  onPressed: () {},
  child: Text('Click Me'),
)
```

### FlatButton

Purpose: A widget that displays a flat button.

```dart
FlatButton(
  onPressed: () {},
  child: Text('Click Me'),
)
```

### IconButton

Purpose: A widget that displays a button with an icon.

```dart
IconButton(
  icon: Icon(Icons.favorite),
  onPressed: () {},
)
```

### Checkbox

Purpose: A widget that allows users to select multiple options from a list.

```dart
Checkbox(
  value: isChecked,
  onChanged: (value) {
    setState(() {
      isChecked = value;
    });
  },
)
```

### Radio

Purpose: A widget that allows users to select a single option from a list.

```dart
Radio(
  value: selectedValue,
  groupValue: groupValue,
  onChanged: (value) {
    setState(() {
      groupValue = value;
    });
  },
)
```

### Slider

Purpose: A widget that allows users to select a value from a range of values.

```dart
Slider(
  value: sliderValue,
  min: 0,
  max: 100,
  onChanged: (value) {
    setState(() {
      sliderValue = value;
    });
  },
)
```

### Switch

Purpose: A widget that allows users to toggle between two states.

```dart
Switch(
  value: switchValue,
  onChanged: (

value) {
    setState(() {
      switchValue = value;
    });
  },
)
```

## List Widgets

### ListView

Purpose: A scrollable list of widgets that can be displayed vertically or horizontally.

```dart
ListView(
  children: [
    ListTile(title: Text('Item 1')),
    ListTile(title: Text('Item 2')),
    ListTile(title: Text('Item 3')),
  ],
)
```

### ListTile

Purpose: A widget that displays a single fixed-height row with a leading icon, title, and subtitle.

```dart
ListView(
  children: [
    ListTile(
      leading: Icon(Icons.star),
      title: Text('Flutter'),
      subtitle: Text('Beautiful UI toolkit'),
    ),
  ],
)
```

### GridView

Purpose: A scrollable grid of widgets that can be displayed in a 2D layout.

```dart
GridView.count(
  crossAxisCount: 2,
  children: [
    Container(color: Colors.red),
    Container(color: Colors.blue),
    Container(color: Colors.green),
    Container(color: Colors.yellow),
  ],
)
```

### SliverGrid

Purpose: A grid with slivers that can be used with CustomScrollView.

```dart
CustomScrollView(
  slivers: [
    SliverGrid(
      gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
        crossAxisCount: 2,
      ),
      delegate: SliverChildBuilderDelegate(
        (BuildContext context, int index) {
          return Container(
            color: Colors.blue,
            height: 100,
          );
        },
        childCount: 4,
      ),
    ),
  ],
)
```

### ExpansionTile

Purpose: A widget that displays a tile that can be expanded or collapsed to reveal hidden content.

```dart
ExpansionTile(
  title: Text('Expandable Tile'),
  children: [
    Text('Hidden content'),
  ],
)
```

### ListView.builder

Purpose: A more efficient way to create a ListView with a large number of children.

```dart
ListView.builder(
  itemCount: items.length,
  itemBuilder: (context, index) {
    return ListTile(title: Text(items[index]));
  },
)
```

### GridView.builder

Purpose: A more efficient way to create a GridView with a large number of children.

```dart
GridView.builder(
  gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(crossAxisCount: 2),
  itemCount: items.length,
  itemBuilder: (context, index) {
    return Container(color: Colors.blue);
  },
)
```

## Navigation and Routing Widgets

### MaterialApp

Purpose: A widget that sets up the necessary material design elements for an app.

```dart
MaterialApp(
  home: HomePage(),
)
```

### Scaffold

Purpose: A widget that provides a basic structure for implementing material design components.

```dart
Scaffold(
  appBar: AppBar(title: Text('App')),
  body: Center(child: Text('Hello, Flutter!')),
)
```

### AppBar

Purpose: A widget that displays an app bar with a title and optional actions.

```dart
AppBar(
  title: Text('App Title'),
  actions: [
    IconButton(icon: Icon(Icons.settings), onPressed: () {}),
    IconButton(icon: Icon(Icons.search), onPressed: () {}),
  ],
)
```

### BottomNavigationBar

Purpose: A widget that displays a bottom navigation bar.

```dart
BottomNavigationBar(
  items: [
    BottomNavigationBarItem(icon: Icon(Icons.home), label: 'Home'),
    BottomNavigationBarItem(icon: Icon(Icons.favorite), label: 'Favorites'),
    BottomNavigationBarItem(icon: Icon(Icons.settings), label: 'Settings'),
  ],
)
```

### Drawer

Purpose: A widget that displays a side drawer that can be shown by swiping from the left or right.

```dart
Scaffold(
  appBar: AppBar(title: Text('App')),
  body: Center(child: Text('Hello, Flutter!')),
  drawer: Drawer(
    child: ListView(
      children: [
        ListTile(title: Text('Item 1')),
        ListTile(title: Text('Item 2')),
        ListTile(title: Text('Item 3')),
      ],
    ),
  ),
)
```

### Navigator

Purpose: A widget that manages a stack of pages and transitions between them.

```dart
Navigator.push(
  context,
  MaterialPageRoute(
    builder: (context) => SecondPage(),
  ),
)
```

### MaterialPageRoute

Purpose: A class that defines a basic page route that transitions to a new page.

```dart
MaterialPageRoute(
  builder: (context) => SecondPage(),
)
```

### CupertinoPageRoute

Purpose: A class that defines a page route that provides a cupertino-style transition.

```dart
CupertinoPageRoute(
  builder: (context) => SecondPage(),
)
```

## Animation Widgets

### AnimatedContainer

Purpose: A container that automatically animates its changes when the properties change.

```dart
AnimatedContainer(
  duration: Duration(seconds: 1),
  width: _isExpanded ? 200 : 100,
  height: _isExpanded ? 200 : 100,
  color: _isExpanded ? Colors.blue : Colors.red,
)
```

### AnimatedCrossFade

Purpose: A widget that cross-fades between two children when switching between them.

```dart
AnimatedCrossFade(
  duration: Duration(seconds: 1),
  firstChild: Container(color: Colors.blue, height: 100, width: 100),
  secondChild: Container(color: Colors.red, height: 200, width: 200),
  crossFadeState: _showFirst ? CrossFadeState.showFirst : CrossFadeState.showSecond,
)
```

### Hero

Purpose: A widget that animates the shared transition of its tag with another Hero widget.

```dart
Hero(
  tag: 'logo',
  child: Image.asset('assets/logo.png'),
)
```

### AnimatedBuilder

Purpose: A widget that rebuilds when given an Animation object.

```dart
AnimatedBuilder(
  animation: _controller,
  builder: (context, child) {
    return Transform.rotate(
      angle: _controller.value * 2.0 * pi,
      child: child,
    );
  },
  child: Icon(Icons.refresh),
)
```

### SlideTransition

Purpose: A widget that animates the position of its child from an initial offset to an end offset.

```dart
SlideTransition(
  position: _animation,
  child: Container(color: Colors.blue, height: 100, width: 100),
)
```

### FadeTransition

Purpose: A widget that animates the opacity of its child.

```dart
FadeTransition(
  opacity: _animation,
  child: Container(color: Colors.blue, height: 100, width: 100),
)
```

### RotationTransition

Purpose: A widget that animates the rotation of its child.

```dart
RotationTransition(
  turns: _animation,
  child: Container(color: Colors.blue, height: 100, width: 100),
)
```

## Layout Widgets

### SizedBox

Purpose: A box with a fixed width and height.

```dart
SizedBox(
  width: 200,
  height: 100,
  child: Container(color: Colors.blue),
)
```

### Flexible

Purpose: A widget that takes up available space and shares it with other flexible widgets.

```dart
Row(
  children: [
    Flexible(flex: 1, child

: Container(color: Colors.blue, height: 50)),
    Flexible(flex: 2, child: Container(color: Colors.green, height: 50)),
  ],
)
```

### Wrap

Purpose: A widget that wraps its children to the next line when there is insufficient space.

```dart
Wrap(
  children: [
    Chip(label: Text('Tag 1')),
    Chip(label: Text('Tag 2')),
    Chip(label: Text('Tag 3')),
  ],
)
```

### AspectRatio

Purpose: A widget that attempts to size the child to a specific aspect ratio.

```dart
AspectRatio(
  aspectRatio: 16 / 9,
  child: Image.asset('assets/image.png'),
)
```

### ConstrainedBox

Purpose: A widget that imposes constraints on its child.

```dart
ConstrainedBox(
  constraints: BoxConstraints(minWidth: 100, minHeight: 100),
  child: Container(color: Colors.blue),
)
```

### FittedBox

Purpose: A widget that scales and positions its child within itself.

```dart
FittedBox(
  fit: BoxFit.cover,
  child: Image.asset('assets/image.png'),
)
```

## Image and Icon Handling

### Image

Purpose: A widget that displays an image from the local assets or the internet.

```dart
Image.asset('assets/image.png')
```

### ImageIcon

Purpose: A widget that displays a material design icon as an image.

```dart
ImageIcon(Icons.star)
```

### Image.network

Purpose: A widget that displays an image from the internet.

```dart
Image.network('https://example.com/image.jpg')
```

### Image.asset

Purpose: A widget that displays an image from the local assets.

```dart
Image.asset('assets/image.png')
```

### Icon

Purpose: A widget that displays a material design icon.

```dart
Icon(Icons.star)
```

### IconButton

Purpose: A widget that displays a button with an icon.

```dart
IconButton(
  icon: Icon(Icons.favorite),
  onPressed: () {},
)
```

### CircleAvatar

Purpose: A widget that displays a circular avatar.

```dart
CircleAvatar(
  backgroundImage: AssetImage('assets/avatar.jpg'),
  radius: 50,
)
```

## Material Design Widgets

### Card

Purpose: A widget that displays a material design card.

```dart
Card(
  child: Column(
    children: [
      ListTile(title: Text('Title'), subtitle: Text('Subtitle')),
      Image.asset('assets/image.png'),
      ButtonBar(
        children: [
          FlatButton(child: Text('Button 1'), onPressed: () {}),
          FlatButton(child: Text('Button 2'), onPressed: () {}),
        ],
      ),
    ],
  ),
)
```

### AppBar

Purpose: A widget that displays an app bar with a title and optional actions.

```dart
AppBar(
  title: Text('App Title'),
  actions: [
    IconButton(icon: Icon(Icons.settings), onPressed: () {}),
    IconButton(icon: Icon(Icons.search), onPressed: () {}),
  ],
)
```

### BottomNavigationBar

Purpose: A widget that displays a bottom navigation bar.

```dart
BottomNavigationBar(
  items: [
    BottomNavigationBarItem(icon: Icon(Icons.home), label: 'Home'),
    BottomNavigationBarItem(icon: Icon(Icons.favorite), label: 'Favorites'),
    BottomNavigationBarItem(icon: Icon(Icons.settings), label: 'Settings'),
  ],
)
```

### FloatingActionButton

Purpose: A widget that displays a floating action button.

```dart
FloatingActionButton(
  onPressed: () {},
  child: Icon(Icons.add),
)
```

### Material

Purpose: A widget that sets the material design for its children.

```dart
Material(
  elevation: 4,
  color: Colors.blue,
  borderRadius: BorderRadius.circular(8),
  child: Container(
    width: 100,
    height: 100,
    child: Center(child: Text('Material')),
  ),
)
```

### AlertDialog

Purpose: A widget that displays a material design alert dialog.

```dart
AlertDialog(
  title: Text('Alert Dialog'),
  content: Text('This is an alert dialog.'),
  actions: [
    FlatButton(child: Text('OK'), onPressed: () {}),
    FlatButton(child: Text('Cancel'), onPressed: () {}),
  ],
)
```

## Cupertino Design Widgets

### CupertinoApp

Purpose: A widget that sets up the necessary cupertino design elements for an app.

```dart
CupertinoApp(
  home: HomePage(),
)
```

### CupertinoNavigationBar

Purpose: A widget that displays a cupertino-style navigation bar.

```dart
CupertinoNavigationBar(
  middle: Text('App Title'),
  trailing: IconButton(icon: Icon(Icons.search), onPressed: () {}),
)
```

### CupertinoButton

Purpose: A widget that displays a cupertino-style button.

```dart
CupertinoButton(
  onPressed: () {},
  child: Text('Click Me'),
)
```

### CupertinoAlertDialog

Purpose: A widget that displays a cupertino-style alert dialog.

```dart
CupertinoAlertDialog(
  title: Text('Alert Dialog'),
  content: Text('This is a cupertino-style alert dialog.'),
  actions: [
    CupertinoDialogAction(child: Text('OK'), onPressed: () {}),
    CupertinoDialogAction(child: Text('Cancel'), onPressed: () {}),
  ],
)
```

### CupertinoPicker

Purpose: A widget that displays a cupertino-style picker.

```dart
CupertinoPicker(
  itemExtent: 32,
  onSelectedItemChanged: (index) {},
  children: [
    Text('Option 1'),
    Text('Option 2'),
    Text('Option 3'),
  ],
)
```

## Other Important Widgets

### WebView

Purpose: A widget that displays a web page in the app.

```dart
WebView(
  initialUrl: 'https://example.com',
)
```

### VideoPlayer

Purpose: A widget that displays a video player in the app.

```dart
VideoPlayerController controller = VideoPlayerController.asset('assets/video.mp4');
VideoPlayer(controller),
```

### PageView

Purpose: A widget that displays a scrollable list of pages.

```dart
PageView(
  children: [
    Container(color: Colors.red),
    Container(color: Colors.blue),
    Container(color: Colors.green),
  ],
)
```

### Stepper

Purpose: A widget that displays a material design stepper.

```dart
Stepper(
  steps: [
    Step(title: Text('Step 1'), content: Text('Step 1 content')),
    Step(title: Text('Step 2'), content: Text('Step 2 content')),
    Step(title: Text('Step 3'), content: Text('Step 3 content')),
  ],
)
```

### ShaderMask

Purpose: A widget that applies a shader to its child.

```dart
ShaderMask(
  shaderCallback: (rect) {
    return RadialGradient(
      center: Alignment.topLeft,
      radius: 1.0,
      colors: [Colors.yellow, Colors.transparent],
      tileMode: TileMode.clamp,
    ).createShader(rect);
  },
  child: Image.asset('assets/image.png'),
)
```

### BackdropFilter

Purpose: A widget that applies a filter to its child.

```dart
BackdropFilter(
  filter: ImageFilter.blur(sigmaX: 5, sigmaY: 5),
  child: Container(color: Colors.blue.withOpacity(0.5)),
)


```

This is just a selection of some commonly used Flutter widgets. The Flutter ecosystem is vast, and there are many more widgets and features to explore. Happy coding with Flutter!

For more information and detailed documentation on all widgets, please refer to the official Flutter documentation: [https://flutter.dev/docs](https://flutter.dev/docs)
