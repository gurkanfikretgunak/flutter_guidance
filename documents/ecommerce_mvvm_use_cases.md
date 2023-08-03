
# Flutter MVVM Use Cases - E-Commerce Applications

## 1. View Product List

In this use case, we will use MVVM to display a list of products available in the e-commerce application.

**Model:**

```dart
class Product {
  final String name;
  final double price;
  final String imageUrl;

  Product({required this.name, required this.price, required this.imageUrl});
}
```

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class ProductViewModel extends ChangeNotifier {
  List<Product> _products = [
    Product(name: 'Smartphone', price: 499.99, imageUrl: 'https://example.com/smartphone.jpg'),
    Product(name: 'Laptop', price: 899.99, imageUrl: 'https://example.com/laptop.jpg'),
    Product(name: 'Headphones', price: 99.99, imageUrl: 'https://example.com/headphones.jpg'),
  ];

  List<Product> get products => _products;
}
```

**View:**

```dart
class ProductListView extends StatelessWidget {
  final ProductViewModel viewModel;

  ProductListView({required this.viewModel});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Product List')),
      body: ListView.builder(
        itemCount: viewModel.products.length,
        itemBuilder: (context, index) {
          final product = viewModel.products[index];
          return ListTile(
            leading: Image.network(product.imageUrl),
            title: Text(product.name),
            subtitle: Text('\$${product.price.toStringAsFixed(2)}'),
          );
        },
      ),
    );
  }
}
```

## 2. Search Products

In this use case, we will use MVVM to search for products based on user input.

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class ProductSearchViewModel extends ChangeNotifier {
  List<Product> _allProducts = [
    Product(name: 'Smartphone', price: 499.99, imageUrl: 'https://example.com/smartphone.jpg'),
    Product(name: 'Laptop', price: 899.99, imageUrl: 'https://example.com/laptop.jpg'),
    Product(name: 'Headphones', price: 99.99, imageUrl: 'https://example.com/headphones.jpg'),
  ];

  List<Product> _searchResults = [];

  List<Product> get searchResults => _searchResults;

  void searchProducts(String query) {
    _searchResults = _allProducts.where((product) => product.name.toLowerCase().contains(query.toLowerCase())).toList();
    notifyListeners();
  }
}
```

**View:**

```dart
class ProductSearchView extends StatelessWidget {
  final ProductSearchViewModel viewModel;

  ProductSearchView({required this.viewModel});

  final TextEditingController _searchController = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Search Products')),
      body: Column(
        children: [
          TextField(
            controller: _searchController,
            decoration: InputDecoration(labelText: 'Search'),
            onChanged: (query) => viewModel.searchProducts(query),
          ),
          Expanded(
            child: ListView.builder(
              itemCount: viewModel.searchResults.length,
              itemBuilder: (context, index) {
                final product = viewModel.searchResults[index];
                return ListTile(
                  leading: Image.network(product.imageUrl),
                  title: Text(product.name),
                  subtitle: Text('\$${product.price.toStringAsFixed(2)}'),
                );
              },
            ),
          ),
        ],
      ),
    );
  }
}
```

## 3. View Product Details

In this use case, we will use MVVM to display detailed information about a selected product.

**Model:**

```dart
class Product {
  final String name;
  final double price;
  final String description;
  final String imageUrl;

  Product({required this.name, required this.price, required this.description, required this.imageUrl});
}
```

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class ProductDetailsViewModel extends ChangeNotifier {
  Product _selectedProduct = Product(
    name: 'Smartphone',
    price: 499.99,
    description: 'A high-end smartphone with advanced features.',
    imageUrl: 'https://example.com/smartphone.jpg',
  );

  Product get selectedProduct => _selectedProduct;
}
```

**View:**

```dart
class ProductDetailsView extends StatelessWidget {
  final ProductDetailsViewModel viewModel;

  ProductDetailsView({required this.viewModel});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Product Details')),
      body: Column(
        crossAxisAlignment: CrossAxisAlignment.stretch,
        children: [
          Image.network(viewModel.selectedProduct.imageUrl),
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(viewModel.selectedProduct.name, style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold)),
                Text('\$${viewModel.selectedProduct.price.toStringAsFixed(2)}', style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),
                Text(viewModel.selectedProduct.description),
              ],
            ),
          ),
        ],
      ),
    );
  }
}
```

## 4. Add to Cart

In this use case, we will use MVVM to add products to the shopping cart.

**Model:**

```dart
class CartItem {
  final

 Product product;
  int quantity;

  CartItem({required this.product, this.quantity = 1});
}
```

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class ShoppingCartViewModel extends ChangeNotifier {
  List<CartItem> _cartItems = [];

  List<CartItem> get cartItems => _cartItems;

  void addToCart(Product product) {
    var existingItem = _cartItems.firstWhere((item) => item.product == product, orElse: () => null);
    if (existingItem != null) {
      existingItem.quantity++;
    } else {
      _cartItems.add(CartItem(product: product));
    }
    notifyListeners();
  }
}
```

**View:**

```dart
class ShoppingCartView extends StatelessWidget {
  final ShoppingCartViewModel viewModel;

  ShoppingCartView({required this.viewModel});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Shopping Cart')),
      body: ListView.builder(
        itemCount: viewModel.cartItems.length,
        itemBuilder: (context, index) {
          final cartItem = viewModel.cartItems[index];
          return ListTile(
            leading: Image.network(cartItem.product.imageUrl),
            title: Text(cartItem.product.name),
            subtitle: Text('\$${cartItem.product.price.toStringAsFixed(2)}'),
            trailing: Text('Quantity: ${cartItem.quantity}'),
          );
        },
      ),
    );
  }
}
```

## 5. Checkout

In this use case, we will use MVVM to handle the checkout process and display the order summary.

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class CheckoutViewModel extends ChangeNotifier {
  List<CartItem> _cartItems = [];

  List<CartItem> get cartItems => _cartItems;

  void setCartItems(List<CartItem> items) {
    _cartItems = items;
  }

  double getTotalAmount() {
    return _cartItems.fold(0, (total, item) => total + (item.product.price * item.quantity));
  }

  void checkout() {
    // Perform checkout logic here
    // e.g., clear cart, update inventory, process payment, etc.
    _cartItems.clear();
    notifyListeners();
  }
}
```

**View:**

```dart
class CheckoutView extends StatelessWidget {
  final CheckoutViewModel viewModel;

  CheckoutView({required this.viewModel});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Checkout')),
      body: Column(
        crossAxisAlignment: CrossAxisAlignment.stretch,
        children: [
          Expanded(
            child: ListView.builder(
              itemCount: viewModel.cartItems.length,
              itemBuilder: (context, index) {
                final cartItem = viewModel.cartItems[index];
                return ListTile(
                  leading: Image.network(cartItem.product.imageUrl),
                  title: Text(cartItem.product.name),
                  subtitle: Text('\$${cartItem.product.price.toStringAsFixed(2)}'),
                  trailing: Text('Quantity: ${cartItem.quantity}'),
                );
              },
            ),
          ),
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: Text('Total: \$${viewModel.getTotalAmount().toStringAsFixed(2)}', style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold)),
          ),
          ElevatedButton(
            onPressed: () {
              viewModel.checkout();
              showDialog(
                context: context,
                builder: (context) => AlertDialog(
                  title: Text('Order Placed'),
                  content: Text('Your order has been placed successfully.'),
                  actions: [
                    TextButton(
                      onPressed: () {
                        Navigator.pop(context);
                      },
                      child: Text('Close'),
                    ),
                  ],
                ),
              );
            },
            child: Text('Place Order'),
          ),
        ],
      ),
    );
  }
}
```

## 6. View Categories and Subcategories

In this use case, we will use MVVM to display product categories and their subcategories.

**Model:**

```dart
class Category {
  final String name;
  final List<String> subcategories;

  Category({required this.name, required this.subcategories});
}
```

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class CategoryViewModel extends ChangeNotifier {
  List<Category> _categories = [
    Category(name: 'Electronics', subcategories: ['Smartphones', 'Laptops', 'Headphones']),
    Category(name: 'Clothing', subcategories: ['T-Shirts', 'Jeans', 'Dresses']),
    Category(name: 'Home & Living', subcategories: ['Furniture', 'Bedding', 'Decor']),
  ];

  List<Category> get categories => _categories;
}
```

**View:**

```dart
class CategoryView extends StatelessWidget {
  final CategoryViewModel viewModel;

  CategoryView({required this.viewModel});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Categories')),
      body: ListView.builder(
        itemCount: viewModel.categories.length,
        itemBuilder: (context, index) {
          final category = viewModel.categories[index];
          return ExpansionTile(
            title: Text(category.name),
            children: [
              for (var subcategory in category.subcategories) ListTile(title: Text(subcategory)),
            ],
          );
        },
      ),
    );
  }
}
```

## 7. View Featured Products

In this use case, we will use MVVM to display a list of featured products in the e-commerce application.

**Model:**

```dart
class

 FeaturedProduct {
  final String name;
  final double price;
  final String imageUrl;

  FeaturedProduct({required this.name, required this.price, required this.imageUrl});
}
```

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class FeaturedProductViewModel extends ChangeNotifier {
  List<FeaturedProduct> _featuredProducts = [
    FeaturedProduct(name: 'Smartphone', price: 499.99, imageUrl: 'https://example.com/smartphone.jpg'),
    FeaturedProduct(name: 'Laptop', price: 899.99, imageUrl: 'https://example.com/laptop.jpg'),
    FeaturedProduct(name: 'Headphones', price: 99.99, imageUrl: 'https://example.com/headphones.jpg'),
  ];

  List<FeaturedProduct> get featuredProducts => _featuredProducts;
}
```

**View:**

```dart
class FeaturedProductView extends StatelessWidget {
  final FeaturedProductViewModel viewModel;

  FeaturedProductView({required this.viewModel});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Featured Products')),
      body: ListView.builder(
        itemCount: viewModel.featuredProducts.length,
        itemBuilder: (context, index) {
          final product = viewModel.featuredProducts[index];
          return ListTile(
            leading: Image.network(product.imageUrl),
            title: Text(product.name),
            subtitle: Text('\$${product.price.toStringAsFixed(2)}'),
          );
        },
      ),
    );
  }
}
```

## 8. View User Profile

In this use case, we will use MVVM to display the user's profile information.

**Model:**

```dart
class UserProfile {
  final String username;
  final String email;
  final String imageUrl;

  UserProfile({required this.username, required this.email, required this.imageUrl});
}
```

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class UserProfileViewModel extends ChangeNotifier {
  UserProfile _userProfile = UserProfile(
    username: 'john_doe',
    email: 'john.doe@example.com',
    imageUrl: 'https://example.com/john_doe.jpg',
  );

  UserProfile get userProfile => _userProfile;
}
```

**View:**

```dart
class UserProfileView extends StatelessWidget {
  final UserProfileViewModel viewModel;

  UserProfileView({required this.viewModel});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('User Profile')),
      body: Column(
        crossAxisAlignment: CrossAxisAlignment.center,
        children: [
          SizedBox(height: 20),
          CircleAvatar(radius: 50, backgroundImage: NetworkImage(viewModel.userProfile.imageUrl)),
          SizedBox(height: 20),
          Text('Username: ${viewModel.userProfile.username}', style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold)),
          SizedBox(height: 10),
          Text('Email: ${viewModel.userProfile.email}', style: TextStyle(fontSize: 18)),
        ],
      ),
    );
  }
}
```

## 9. View Order History

In this use case, we will use MVVM to display the user's order history.

**Model:**

```dart
class Order {
  final String orderId;
  final List<CartItem> items;
  final double totalAmount;
  final DateTime orderDate;

  Order({required this.orderId, required this.items, required this.totalAmount, required this.orderDate});
}
```

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class OrderHistoryViewModel extends ChangeNotifier {
  List<Order> _orderHistory = [
    Order(orderId: '12345', items: [CartItem(product: Product(name: 'Smartphone', price: 499.99, imageUrl: 'https://example.com/smartphone.jpg')), CartItem(product: Product(name: 'Headphones', price: 99.99, imageUrl: 'https://example.com/headphones.jpg'))], totalAmount: 599.98, orderDate: DateTime.now().subtract(Duration(days: 5))),
    Order(orderId: '67890', items: [CartItem(product: Product(name: 'Laptop', price: 899.99, imageUrl: 'https://example.com/laptop.jpg')), CartItem(product: Product(name: 'Headphones', price: 99.99, imageUrl: 'https://example.com/headphones.jpg'))], totalAmount: 999.98, orderDate: DateTime.now().subtract(Duration(days: 10))),
  ];

  List<Order> get orderHistory => _orderHistory;
}
```

**View:**

```dart
class OrderHistoryView extends StatelessWidget {
  final OrderHistoryViewModel viewModel;

  OrderHistoryView({required this.viewModel});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Order History')),
      body: ListView.builder(
        itemCount: viewModel.orderHistory.length,
        itemBuilder: (context, index) {
          final order = viewModel.orderHistory[index];
          return ExpansionTile(
            title: Text('Order #${order.orderId}'),
            children: [
              for (var item in order.items) ListTile(title: Text('${item.product.name} - \$${item.product.price.toStringAsFixed(2)}')),
              ListTile(title: Text('Total Amount: \$${order.totalAmount.toStringAsFixed(2)}')),
              ListTile(title: Text('Order Date: ${order.orderDate.day}/${order.orderDate.month}/${order.orderDate.year}')),
            ],
          );
        },
      ),
    );
  }
}
```

## 10. View Deals and Discounts

In this use case, we will use MVVM to display a list of deals and discounts available in the e-commerce application.

**Model:**

```dart
class Deal {
  final String title;
  final String description;

  Deal({required this.title, required this.description});
}
```

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class DealsViewModel extends ChangeNotifier {
  List<Deal> _deals = [
    Deal(title: '50% Off Smartphones', description: 'Get 50% off on all smartphones. Limited time offer.'),
    Deal(title: 'Flash Sale on Laptops', description: 'Flash sale on laptops. Grab them before they sell out.'),
    Deal(title: 'Buy 1 Get 1 Free Headphones', description: 'Buy one pair of headphones and get another one for free.'),
  ];

  List<Deal> get deals => _deals;
}
```

**View:**

```dart
class DealsView extends StatelessWidget {
  final DealsViewModel viewModel;

  DealsView({required this.viewModel});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Deals and Discounts')),
      body: ListView.builder(
        itemCount: viewModel.deals.length,
        itemBuilder: (context, index) {
          final deal = viewModel.deals[index];
          return ListTile(
            title: Text(deal.title),
            subtitle: Text(deal.description),
          );
        },
      ),
    );
  }
}
```
