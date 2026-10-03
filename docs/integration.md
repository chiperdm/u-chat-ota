# U-Chat Flutter integration

Stable manifest URL:

```dart
const otaManifestUrl =
    'https://raw.githubusercontent.com/chiperdm/u-chat-ota/main/latest.json';
```

Recommended check flow:

```text
fetch manifest
    ↓
validate product/channel
    ↓
compare remote build with current build
    ↓
remote build > current build?
    ├─ no  → no update
    └─ yes → show update UI
               ↓
          download APK
               ↓
        verify SHA-256 (when present)
               ↓
        start Android installer
```

A minimal model can look like:

```dart
class OtaManifest {
  const OtaManifest({
    required this.enabled,
    required this.version,
    required this.build,
    required this.minSupportedBuild,
    required this.downloadUrl,
    required this.sha256,
    required this.releaseNotes,
  });

  final bool enabled;
  final String version;
  final int build;
  final int minSupportedBuild;
  final String downloadUrl;
  final String sha256;
  final List<String> releaseNotes;

  factory OtaManifest.fromJson(Map<String, dynamic> json) {
    return OtaManifest(
      enabled: json['enabled'] as bool,
      version: json['version'] as String,
      build: json['build'] as int,
      minSupportedBuild: json['min_supported_build'] as int,
      downloadUrl: json['download_url'] as String,
      sha256: json['sha256'] as String? ?? '',
      releaseNotes: [
        for (final item in (json['release_notes'] as List<dynamic>? ?? const []))
          item as String,
      ],
    );
  }
}
```

## Important

Do not treat the human-readable version string as the update comparison source. Compare Android's numeric build/version code:

```dart
if (remote.build > currentBuild) {
  // Update available.
}
```

For forced updates, use:

```dart
final forceUpdate = currentBuild < manifest.minSupportedBuild;
```
