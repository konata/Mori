# Mori

A lightweight AOSP source browsing project for IDEA / IntelliJ.

Its goal is not to build AOSP. It exists to solve these problems:

- IDEA cannot recognize source roots after importing multiple AOSP repositories
- Platform classes and module dependencies are missing, so navigation and indexing are incomplete
- Some AIDL types are missing, which causes unresolved symbols

Example layout:

```text
workspace/
├── Mori/
├── base/
├── Bluetooth/
├── Wifi/
└── ...
```

`base` is required. Other modules are optional and can be added as needed.

## What It Does

- `link`
  - Creates symlinks from sibling AOSP repositories into `externals/`
  - Lets IDEA treat those sources as part of this project
- `stubify`
  - Scans `.aidl` files under `externals/`
  - Generates minimal Java stubs for missing `interface` and `parcelable` types
  - Writes output to `build/generated/stubs/`

## Usage

1. Prepare `../base`, and add other AOSP module repositories if needed.
2. Run this in the project root:

```bash
./gradlew link stubify
```

3. Open `Mori` in IDEA / IntelliJ as a Gradle project.
4. If you add modules, switch branches, or introduce new AIDL files, run it again:

```bash
./gradlew link stubify
```

## Constraints

- This is a browsing project, not a guaranteed buildable workspace
- Missing modules do not block the project itself, but they do affect navigation and indexing for those sources
- The `link` task prints `git clone` hints for missing modules
