
# MVVM with Repository - E-Commerce Use Cases

## 1. View Product List from API

In this use case, we will use MVVM with a repository to fetch a list of products from an API and display them in the e-commerce application.

**Model:**

```dart
class Product {
  final String name;
  final double price;
  final String imageUrl;

  Product({required this.name, required this.price, required this.imageUrl});
}
```

**Repository:**

```dart
class ProductRepository {
  Future<List<Product>> getProducts() async {
    // Fetch products from the API
    // Example API call using http package:
    // final response = await http.get('https://api.example.com/products');
    // Parse the response and return the list of products
    // return List<Product>.from(response.data.map((productData) => Product(
    //   name: productData['name'],
    //   price: productData['price'],
    //   imageUrl: productData['imageUrl'],
    // )));
    // For this example, we will use a hardcoded list of products.
    return [
      Product(name: 'Smartphone', price: 499.99, imageUrl: 'https://example.com/smartphone.jpg'),
      Product(name: 'Laptop', price: 899.99, imageUrl: 'https://example.com/laptop.jpg'),
      Product(name: 'Headphones', price: 99.99, imageUrl: 'https://example.com/headphones.jpg'),
    ];
  }
}
```

**ViewModel:**

```dart
import 'package:flutter/material.dart';

class ProductListViewModel extends ChangeNotifier {
  final ProductRepository _repository;

  ProductListViewModel(this._repository);

  List<Product> _products = [];

  List<Product> get products => _products;

  Future<void> fetchProducts() async {
    _products = await _repository.getProducts();
    notifyListeners();
  }
}
```

**View:**

```dart
class ProductListView extends StatelessWidget {
  final ProductListViewModel viewModel;

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

## 2. Search Products from API

In this use case, we will use MVVM with a repository to search for products based on user input by making API calls.

**Repository:**

```dart
class ProductRepository {
  // ... existing code ...

  Future<List<Product>> searchProducts(String query) async {
    // Fetch products from the API based on the query
    // Example API call using http package:
    // final response = await http.get('https://api.example.com/products?q=$query');
    // Parse the response and return the list of products
    // return List<Product>.from(response.data.map((productData) => Product(
    //   name: productData['name'],
    //   price: productData['price'],
    //   imageUrl: productData['imageUrl'],
    // )));
    // For this example, we will use a hardcoded list of products for simplicity.
    return _products.where((product) => product.name.toLowerCase().contains(query.toLowerCase())).toList();
  }
}
```

**ViewModel:**

```dart
class ProductSearchViewModel extends ChangeNotifier {
  // ... existing code ...

  Future<void> searchProducts(String query) async {
    _products = await _repository.searchProducts(query);
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
          // ... existing code ...
        ],
      ),
    );
  }
}
```

## 3. View Product Details from API

In this use case, we will use MVVM with a repository to fetch and display detailed information about a selected product from the API.

**Repository:**

```dart
class ProductRepository {
  // ... existing code ...

  Future<Product> getProductDetails(String productId) async {
    // Fetch product details from the API based on the productId
    // Example API call using http package:
    // final response = await http.get('https://api.example.com/products/$productId');
    // Parse the response and return the product details
    // return Product(
    //   name: response.data['name'],
    //   price: response.data['price'],
    //   imageUrl: response.data['imageUrl'],
    // );
    // For this example, we will use a hardcoded product details.
    return Product(name: 'Smartphone', price: 499.99, imageUrl: 'https://example.com/smartphone.jpg');
  }
}
```

**ViewModel:**

```dart
class ProductDetailsViewModel extends ChangeNotifier {
  // ... existing code ...

  Product? _selectedProduct;

  Product? get selectedProduct => _selectedProduct;

  Future<void> fetchProductDetails(String productId) async {
    _selectedProduct = await _repository.getProductDetails(productId);
    notifyListeners();
  }
}
```

**View:**

```dart
class ProductDetailsView extends StatelessWidget {
  final ProductDetailsViewModel viewModel;

  ProductDetailsView({required this.viewModel});

  @override
  Widget build(BuildContext context) {
    // ... existing code ...
  }
}
```

## 4. Add to Cart with Local Storage

In this use case, we will use MVVM with a repository to add products to the shopping cart and store the cart locally on the device.

**Model:**

```dart
class CartItem {
  final Product product;
  int quantity;

  CartItem({required this.product, this.quantity = 1});
}
```

**Repository:**

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';

class CartRepository {
  static const String _cartKey = 'cart';

  Future<List<CartItem>> getCartItems() async {
    final prefs = await SharedPreferences.getInstance();
    final cartData = prefs.getStringList(_cartKey);
    if (cartData == null) {
      return [];
    }
    return cartData.map((itemData) {
      final productData = itemData.split(',');
      final product = Product(name: productData[0], price: double.parse(productData[1]), imageUrl: productData[2]);
      return CartItem(product: product, quantity: int.parse(productData[3]));
    }).toList();
  }

  Future<void> addToCart(CartItem item) async {
    final prefs = await SharedPreferences.getInstance();
   

 final cartData = prefs.getStringList(_cartKey) ?? [];
    final itemData = '${item.product.name},${item.product.price},${item.product.imageUrl},${item.quantity}';
    cartData.add(itemData);
    await prefs.setStringList(_cartKey, cartData);
  }

  Future<void> clearCart() async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.remove(_cartKey);
  }
}
```

**ViewModel:**

```dart
class ShoppingCartViewModel extends ChangeNotifier {
  final CartRepository _repository;

  ShoppingCartViewModel(this._repository);

  List<CartItem> _cartItems = [];

  List<CartItem> get cartItems => _cartItems;

  Future<void> fetchCartItems() async {
    _cartItems = await _repository.getCartItems();
    notifyListeners();
  }

  Future<void> addToCart(Product product) async {
    var existingItem = _cartItems.firstWhere((item) => item.product == product, orElse: () => null);
    if (existingItem != null) {
      existingItem.quantity++;
    } else {
      _cartItems.add(CartItem(product: product));
    }
    notifyListeners();
    await _repository.addToCart(_cartItems.last);
  }

  Future<void> clearCart() async {
    _cartItems.clear();
    notifyListeners();
    await _repository.clearCart();
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
          // ... existing code ...
        },
      ),
    );
  }
}
```

## 5. Checkout with API Integration

In this use case, we will use MVVM with a repository to handle the checkout process and place an order by making API calls.

**Repository:**

```dart
class OrderRepository {
  Future<void> placeOrder(List<CartItem> items) async {
    // Perform order placement logic using API
    // Example API call using http package:
    // await http.post('https://api.example.com/orders', body: {'items': items});
    // For this example, we will simply print the order details.
    print('Order Placed:');
    for (var item in items) {
      print(' - ${item.product.name} x ${item.quantity}');
    }
  }
}
```

**ViewModel:**

```dart
class CheckoutViewModel extends ChangeNotifier {
  // ... existing code ...

  final OrderRepository _orderRepository;

  CheckoutViewModel(this._orderRepository);

  Future<void> checkout() async {
    await _orderRepository.placeOrder(_cartItems);
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
          // ... existing code ...
        ],
      ),
    );
  }
}
```

## 6. View Categories and Subcategories from API

In this use case, we will use MVVM with a repository to fetch and display product categories and their subcategories from an API.

**Model:**

```dart
class Category {
  final String name;
  final List<String> subcategories;

  Category({required this.name, required this.subcategories});
}
```

**Repository:**

```dart
class CategoryRepository {
  Future<List<Category>> getCategories() async {
    // Fetch categories from the API
    // Example API call using http package:
    // final response = await http.get('https://api.example.com/categories');
    // Parse the response and return the list of categories
    // return List<Category>.from(response.data.map((categoryData) => Category(
    //   name: categoryData['name'],
    //   subcategories: List<String>.from(categoryData['subcategories']),
    // )));
    // For this example, we will use a hardcoded list of categories.
    return [
      Category(name: 'Electronics', subcategories: ['Smartphones', 'Laptops', 'Headphones']),
      Category(name: 'Clothing', subcategories: ['T-Shirts', 'Jeans', 'Dresses']),
      Category(name: 'Home & Living', subcategories: ['Furniture', 'Bedding', 'Decor']),
    ];
  }
}
```

**ViewModel:**

```dart
class CategoryViewModel extends ChangeNotifier {
  // ... existing code ...

  List<Category> _categories = [];

  List<Category> get categories => _categories;

  Future<void> fetchCategories() async {
    _categories = await _repository.getCategories();
    notifyListeners();
  }
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

## 7. View Featured Products from API

In this use case, we will use MVVM with a repository to fetch and display a list of featured products from an API.

**Repository:**

```dart
class FeaturedProductRepository {
  Future<List<Product>> getFeaturedProducts() async {
    // Fetch featured products from the API
    // Example API call using http package:
    // final response = await http.get('https://api.example.com/featured');
    // Parse the response and return the list of featured products
    // return List<Product>.from(response.data.map((productData) => Product(
    //   name: productData['name'],
    //   price: productData['price'],
    //   imageUrl: productData['imageUrl'],
    // )));
    // For this example, we will use a hardcoded list of featured products.
    return [
      Product(name: 'Smartphone', price: 499.99, imageUrl: 'https://example.com/smartphone.jpg'),
      Product(name: 'Laptop', price: 899.99, imageUrl: 'https://example.com/laptop.jpg'),
      Product(name: 'Headphones', price: 99.99, imageUrl: 'https://example.com/headphones.jpg'),
    ];
  }
}
```

**ViewModel:**

```dart
class FeaturedProductViewModel extends ChangeNotifier {
  // ... existing code ...

  List<Product> _featuredProducts = [];

  List<Product> get featuredProducts => _featuredProducts;

  Future<void> fetchFeaturedProducts() async {
    _featuredProducts = await _repository.getFeaturedProducts();
    notifyListeners();
  }
}
```

**View:**

```dart
class FeaturedProductView extends StatelessWidget {
  final FeaturedProductViewModel viewModel;

  FeaturedProductView({required this.viewModel});

  @override
  Widget build(BuildContext context

) {
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

## 8. View User Profile from API

In this use case, we will use MVVM with a repository to fetch and display the user's profile information from an API.

**Model:**

```dart
class UserProfile {
  final String username;
  final String email;
  final String imageUrl;

  UserProfile({required this.username, required this.email, required this.imageUrl});
}
```

**Repository:**

```dart
class UserRepository {
  Future<UserProfile> getUserProfile() async {
    // Fetch user profile from the API
    // Example API call using http package:
    // final response = await http.get('https://api.example.com/user');
    // Parse the response and return the user profile
    // return UserProfile(
    //   username: response.data['username'],
    //   email: response.data['email'],
    //   imageUrl: response.data['imageUrl'],
    // );
    // For this example, we will use a hardcoded user profile.
    return UserProfile(
      username: 'john_doe',
      email: 'john.doe@example.com',
      imageUrl: 'https://example.com/john_doe.jpg',
    );
  }
}
```

**ViewModel:**

```dart
class UserProfileViewModel extends ChangeNotifier {
  // ... existing code ...

  UserProfile? _userProfile;

  UserProfile? get userProfile => _userProfile;

  Future<void> fetchUserProfile() async {
    _userProfile = await _repository.getUserProfile();
    notifyListeners();
  }
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
          CircleAvatar(radius: 50, backgroundImage: NetworkImage(viewModel.userProfile!.imageUrl)),
          SizedBox(height: 20),
          Text('Username: ${viewModel.userProfile!.username}', style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold)),
          SizedBox(height: 10),
          Text('Email: ${viewModel.userProfile!.email}', style: TextStyle(fontSize: 18)),
        ],
      ),
    );
  }
}
```

## 9. View Order History from API

In this use case, we will use MVVM with a repository to fetch and display the user's order history from an API.

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

**Repository:**

```dart
class OrderRepository {
  Future<List<Order>> getOrderHistory() async {
    // Fetch order history from the API
    // Example API call using http package:
    // final response = await http.get('https://api.example.com/orders');
    // Parse the response and return the list of orders
    // return List<Order>.from(response.data.map((orderData) => Order(
    //   orderId: orderData['orderId'],
    //   items: List<CartItem>.from(orderData['items'].map((itemData) => CartItem(
    //     product: Product(name: itemData['name'], price: itemData['price'], imageUrl: itemData['imageUrl']),
    //     quantity: itemData['quantity'],
    //   ))),
    //   totalAmount: orderData['totalAmount'],
    //   orderDate: DateTime.parse(orderData['orderDate']),
    // )));
    // For this example, we will use a hardcoded list of orders.
    return [
      Order(
        orderId: '12345',
        items: [CartItem(product: Product(name: 'Smartphone', price: 499.99, imageUrl: 'https://example.com/smartphone.jpg')), CartItem(product: Product(name: 'Headphones', price: 99.99, imageUrl: 'https://example.com/headphones.jpg'))],
        totalAmount: 599.98,
        orderDate: DateTime.now().subtract(Duration(days: 5)),
      ),
      Order(
        orderId: '67890',
        items: [CartItem(product: Product(name: 'Laptop', price: 899.99, imageUrl: 'https://example.com/laptop.jpg')), CartItem(product: Product(name: 'Headphones', price: 99.99, imageUrl: 'https://example.com/headphones.jpg'))],
        totalAmount: 999.98,
        orderDate: DateTime.now().subtract(Duration(days: 10)),
      ),
    ];
  }
}
```

**ViewModel:**

```dart
class OrderHistoryViewModel extends ChangeNotifier {
  // ... existing code ...

  List<Order> _orderHistory = [];

  List<Order> get orderHistory => _orderHistory;

  Future<void> fetchOrderHistory() async {
    _orderHistory = await _repository.getOrderHistory();
    notifyListeners();
  }
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

## 10. View Deals and Discounts from API

In this use case, we will use MVVM with a repository to fetch and display a list of deals and discounts from an API.

**Model:**

```dart
class Deal {
  final String title;
  final String description;

  Deal({required this.title, required this.description});
}
```

**Repository:**

```dart
class DealRepository {
  Future<List<Deal>> getDeals() async {
    // Fetch deals from the API
    // Example API call using http package:
    // final response = await http.get('https://api.example.com/deals');
    // Parse the response and return the list of deals
    // return List<Deal>.from(response.data.map((dealData) => Deal(
    //   title: dealData['title'],
    //   description: dealData['description'],
    // )));
    // For this example

, we will use a hardcoded list of deals.
    return [
      Deal(title: '50% Off Smartphones', description: 'Get 50% off on all smartphones. Limited time offer.'),
      Deal(title: 'Flash Sale on Laptops', description: 'Flash sale on laptops. Grab them before they sell out.'),
      Deal(title: 'Buy 1 Get 1 Free Headphones', description: 'Buy one pair of headphones and get another one for free.'),
    ];
  }
}
```

**ViewModel:**

```dart
class DealsViewModel extends ChangeNotifier {
  // ... existing code ...

  List<Deal> _deals = [];

  List<Deal> get deals => _deals;

  Future<void> fetchDeals() async {
    _deals = await _repository.getDeals();
    notifyListeners();
  }
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
