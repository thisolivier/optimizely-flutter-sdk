# Optimizely Flutter SDK — iOS 16 Defensive Fork

This fork adds a defensive wrapper around method channel calls in the Optimizely Flutter SDK to handle an iOS 16 threading issue where `FlutterResult` called from a background thread can silently return `null`, causing a crash.

The only change is the addition of a `_invoke` helper in `lib/src/optimizely_client_wrapper.dart` that catches null responses and platform exceptions gracefully.

## Setup

Replace the official `optimizely_flutter_sdk` dependency in your app's `pubspec.yaml`:

```yaml
# Remove this:
# optimizely_flutter_sdk: 3.4.1

# Add this:
dependency_overrides:
  optimizely_flutter_sdk:
    git:
      url: https://github.com/thisolivier/optimizely-flutter-sdk.git
      ref: fix/ios16-defensive-invoke
```

Then run:

```bash
flutter pub get
```

No other code changes are required — the fork is API-compatible with the official 3.4.1 release.
