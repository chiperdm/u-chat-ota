# U-Chat OTA

Central OTA metadata repository for **U-Chat**.

The mobile client fetches a small JSON manifest from this repository, compares the remote `build` with its current build, and can offer an APK update.

## Repository layout

```text
u-chat-ota/
├── latest.json                 # Current stable release manifest
├── channels/
│   ├── stable.json             # Stable channel mirror
│   └── beta.json               # Beta channel manifest
├── schema/
│   └── latest.schema.json      # JSON Schema for manifests
├── docs/
│   └── integration.md         # Client integration example
└── .github/workflows/
    └── validate.yml            # Manifest validation
```

## Client endpoint

For the stable manifest, U-Chat can read:

```text
https://raw.githubusercontent.com/chiperdm/u-chat-ota/main/latest.json
```

The repository is intentionally small. **Do not commit APKs directly to the repository.** Publish APKs as GitHub Release assets and put the asset URL into the manifest.

## Release flow

1. Build the new U-Chat APK.
2. Create a GitHub Release in this repository, for example `v1.0.1`.
3. Upload the APK as a release asset, e.g. `u-chat-1.0.1.apk`.
4. Update `latest.json` with the new `version`, `build`, `download_url`, notes and optional SHA-256.
5. Commit and push the manifest.
6. U-Chat sees the larger `build` and offers the update.

## Security notes

- Never put API keys, service tokens, signing keys or private credentials in this repository.
- Keep Android signing keys outside GitHub source control.
- Prefer HTTPS only.
- For production, publish a SHA-256 checksum and verify the downloaded APK before installing it.

## License

MIT — see [LICENSE](LICENSE).
