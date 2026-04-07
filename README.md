# Optimizely Flutter SDK — iOS 16 Defensive Fork

This fork adds a defensive wrapper around method channel calls in the Optimizely Flutter SDK to handle an iOS 16 threading issue where `FlutterResult` called from a background thread can silently return `null`, causing a crash.

The only change is the addition of a `_invoke` helper in `lib/src/optimizely_client_wrapper.dart` that catches null responses and platform exceptions gracefully.

## Setup

### 1. Update pubspec.yaml

Replace the official `optimizely_flutter_sdk` dependency in your app's `pubspec.yaml`:

```yaml
# Remove this:
# optimizely_flutter_sdk: 3.4.1

# Add this:
dependency_overrides:
  optimizely_flutter_sdk:
    git:
      url: https://github.com/thisolivier/optimizely-flutter-sdk.git
      ref: master
```

### 2. Clear cached dependencies

Flutter and CocoaPods cache the git dependency. To ensure the fork is picked up cleanly:

```bash
# Clear Flutter's git package cache
rm -rf ~/.pub-cache/git/optimizely-flutter-sdk-*
rm -rf ~/.pub-cache/git/cache/optimizely-flutter-sdk-*

# Resolve dependencies
flutter pub get

# Delete the iOS Podfile.lock so CocoaPods re-resolves native dependencies
rm ios/Podfile.lock
```

### 3. Build

```bash
flutter build ios --no-codesign
```

No other code changes are required — the fork is API-compatible with the official v3.4.1 release.
