# Swift2App: Direct Swift-to-IPA GitHub Actions Builder

Swift2App is a lightweight, zero-Xcode-installation CI/CD workflow designed to compile raw Swift source files into an ad-hoc signed iOS Application Archive (`.ipa`) directly on GitHub Actions. 

This workflow is optimized for rapid testing, prototype deployment, and running Swift applications on iOS devices without maintaining a local Xcode environment or an active Apple Developer account.

---

## Technical Overview

Swift2App bypasses full Xcode project structures (`.xcodeproj` / `.xcworkspace`) by compiling source files directly through the command-line Swift compiler (`swiftc`) against the native `iphoneos` SDK.

### Compilation & Packaging Pipeline

1. **Source Discovery**: Dynamically locates all non-build `.swift` source files across repository directories.
2. **Binary Compilation**: Uses `swiftc` targeting the `arm64-apple-ios15.0` architecture and arm64e support for newer iOS versions such as iOS 27 with `-parse-as-library` enabled for `@main` entry point resolution.
3. **Asset Processing**: Optional downloading and dynamic scaling of app icons using macOS native `sips` into standard iOS asset dimensions (`@2x`, `@3x`, iPad, and iTunesArtwork).
4. **Configuration Generation**: Programmatic assembly of `Info.plist` and `Entitlements.plist` based on user runtime inputs.
5. **Ad-Hoc Signing**: Embedded Mach-O code signature generation via `codesign -s -` with user-selected entitlements and `get-task-allow`.
6. **Payload Packaging**: Structure assembly into standard `Payload/<App_Name>.app` and compression into an installable `.ipa` artifact.

---

## Installation & Setup

### Option A: Forking this Repository
1. Fork this repository directly to your personal GitHub account.
2. Replace or upload your `.swift` source files into the repository root or subfolders.
3. Navigate to the **Actions** tab in your forked repository and click **I understand my workflows, go ahead and enable them**.

### Option B: Adding to an Existing Repository
1. In your target GitHub repository, create the directory structure:
   ```bash
   .github/workflows/
