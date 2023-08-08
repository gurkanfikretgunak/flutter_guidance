# Flutter Network Image Cache Usage Guidance

## Description

Flutter is an open-source UI framework that offers various UI components and tools to developers. Network Image Cache is an important concept that involves caching images fetched over the network to improve app performance and reduce network traffic.

## Why Do We Face This Problem?

Fetching and displaying images from the network each time can negatively impact user experience and lead to unnecessary data usage. This can result in slow loading times, increased data consumption, and an inconsistent UI.

## Potential Problems Caused by This Issue

- Poor user experience due to slow image loading times.
- Increased data consumption from repeatedly downloading the same images.
- Degraded app performance due to constant network requests.

## Solution: Memory Management and Download Control

```dart
class ImageCacheProvider extends InheritedWidget {
  final Map<String, Uint8List> imageCache = {};

  ImageCacheProvider({required Widget child}) : super(child: child);

  static ImageCacheProvider? of(BuildContext context) {
    return context.dependOnInheritedWidgetOfExactType<ImageCacheProvider>();
  }

  @override
  bool updateShouldNotify(covariant InheritedWidget oldWidget) {
    return false;
  }
}

class CachedNetworkImageWidget extends StatelessWidget {
  final String imageUrl;

  CachedNetworkImageWidget({required this.imageUrl});

  @override
  Widget build(BuildContext context) {
    final imageCacheProvider = ImageCacheProvider.of(context);

    if (imageCacheProvider != null && imageCacheProvider.imageCache.containsKey(imageUrl)) {
      return Image.memory(
        imageCacheProvider.imageCache[imageUrl]!,
        fit: BoxFit.contain,
      );
    } else {
      _loadImage(context);
      return CircularProgressIndicator();
    }
  }

  Future<void> _loadImage(BuildContext context) async {
    final response = await http.get(Uri.parse(imageUrl));
    if (response.statusCode == 200) {
      final imageBytes = response.bodyBytes;

      final imageCacheProvider = ImageCacheProvider.of(context);
      if (imageCacheProvider != null) {
        imageCacheProvider.imageCache[imageUrl] = imageBytes;
      }

      context.dependOnInheritedWidgetOfExactType<ImageCacheProvider>()?.updateShouldNotify(covariant InheritedWidget oldWidget) => true;
    }
  }
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ImageCacheProvider(
      child: MaterialApp(
        title: 'Cached Network Image Example',
        home: Scaffold(
          appBar: AppBar(
            title: Text('Cached Network Image'),
          ),
          body: Center(
            child: CachedNetworkImageWidget(imageUrl: "https://example.com/sample-image.jpg"),
          ),
        ),
      ),
    );
  }
}

void main() {
  runApp(MyApp());
}
```

This example demonstrates using `InheritedWidget` to create an image cache. It checks the cache to see if an image is already available, otherwise, it fetches and caches the image. The `updateShouldNotify` method ensures that the widget tree is updated when the cache changes.

## Scenario: Health Application

**Description:** In this scenario, we'll address using Network Image Cache to manage a user's profile image in a health application.

**Scenario Description:** To prevent repeatedly downloading the user's profile image and to improve performance, we'll use Network Image Cache. The user's profile image will be cached for reuse across different screens and components.

**Code Snippet:**

```dart
class UserProfileImage extends StatelessWidget {
  final String imageUrl;

  UserProfileImage({required this.imageUrl});

  @override
  Widget build(BuildContext context) {
    final imageCacheProvider = ImageCacheProvider.of(context);

    if (imageCacheProvider != null && imageCacheProvider.imageCache.containsKey(imageUrl)) {
      return Image.memory(
        imageCacheProvider.imageCache[imageUrl]!,
        fit: BoxFit.contain,
      );
    } else {
      _loadImage(context);
      return CircularProgressIndicator();
    }
  }

  Future<void> _loadImage(BuildContext context) async {
    final response = await http.get(Uri.parse(imageUrl));
    if (response.statusCode == 200) {
      final imageBytes = response.bodyBytes;

      final imageCacheProvider = ImageCacheProvider.of(context);
      if (imageCacheProvider != null) {
        imageCacheProvider.imageCache[imageUrl] = imageBytes;
      }

      context.dependOnInheritedWidgetOfExactType<ImageCacheProvider>()?.updateShouldNotify(covariant InheritedWidget oldWidget) => true;
    }
  }
}

// Usage
UserProfileImage(imageUrl: "https://example.com/user-profile.png"),
```

## Summary

Using Network Image Cache in Flutter is an effective way to improve app performance by caching images fetched from the network. In this Guidance, we learned how to use `InheritedWidget` to cache images and manage their retrieval. Additionally, we explored a scenario involving a health application where the user's profile image is cached for reuse.

## References

- [Flutter Official Website](https://flutter.dev/)
- [Dart Official Website](https://dart.dev/)
- [HTTP Package](https://pub.dev/packages/http)
