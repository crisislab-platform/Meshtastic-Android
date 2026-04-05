<p align="center">
  <img src=".github/meshtastic_logo.png" alt="Meshtastic Logo" width="200"/>
</p>
<h1 align="center">CRISiSLab's Simplified Android Meshtastic Client</h1>

![GitHub all releases](https://img.shields.io/github/downloads/meshtastic/meshtastic-android/total)
[![Android CI](https://github.com/meshtastic/Meshtastic-Android/actions/workflows/pull-request.yml/badge.svg?branch=main)](https://github.com/meshtastic/Meshtastic-Android/actions/workflows/pull-request.yml)
[![codecov](https://codecov.io/gh/meshtastic/Meshtastic-Android/graph/badge.svg)](https://codecov.io/gh/meshtastic/Meshtastic-Android)
[![Crowdin](https://badges.crowdin.net/e/f440f1a5e094a5858dd86deb1adfe83d/localized.svg)](https://crowdin.meshtastic.org/android)
[![CLA assistant](https://cla-assistant.io/readme/badge/meshtastic/Meshtastic-Android)](https://cla-assistant.io/meshtastic/Meshtastic-Android)
[![Fiscal Contributors](https://opencollective.com/meshtastic/tiers/badge.svg?label=Fiscal%20Contributors&color=deeppink)](https://opencollective.com/meshtastic/)
[![Vercel](https://img.shields.io/static/v1?label=Powered%20by&message=Vercel&style=flat&logo=vercel&color=000000)](https://vercel.com?utm_source=meshtastic&utm_campaign=oss)


## Why are we modifying the stock client?

During meetings with local community groups focused on deploying these devices to increase disaster resiliency, they expressed that they found the Meshtastic client too verbose, complicated, and confusing. They wished for a simpler app where you could message, see your conacts, and change some simple settings.

So this project aims to strip out any unnecessary features that may cause confusion.

## Documentation

The project's documentation is generated with [Dokka](https://kotlinlang.org/docs/dokka-introduction.html) and hosted on GitHub Pages. It is automatically updated on every push to the `main` branch.

[**View Documentation**](https://meshtastic.github.io/Meshtastic-Android/)

### Generating Locally

You can generate the documentation locally to preview your changes.

1.  **Run the Dokka task:**
    ```bash
    ./gradlew dokkaGeneratePublicationHtml
    ```
2.  **View the output:**
    The generated HTML files will be located in the `build/dokka/html` directory. You can open the `index.html` file in your browser to view the documentation.

## Architecture

### Modern Android Development (MAD)
The app follows modern Android development practices, built on top of a shared Kotlin Multiplatform (KMP) Core:
- **KMP Modules:** Business logic (`core:domain`), data sources (`core:data`, `core:database`, `core:datastore`), and communications (`core:network`, `core:ble`) are entirely platform-agnostic, targeting Android and Compose Desktop.
- **UI:** JetBrains Compose Multiplatform (Material 3) using Compose Multiplatform resources.
- **State Management:** Unidirectional Data Flow (UDF) with ViewModels, Coroutines, and Flow.
- **Dependency Injection:** Koin with Koin Annotations (K2 Compiler Plugin).
- **Navigation:** JetBrains Navigation 3 (Multiplatform routing with RESTful deep linking).
- **Data Layer:** Repository pattern with Room KMP (local DB), DataStore (prefs), and Protobuf (device comms).

### Bluetooth Low Energy (BLE)
The BLE stack uses a multiplatform interface-driven architecture. Platform-agnostic interfaces live in `commonMain`, utilizing the **Kable** multiplatform BLE library to handle device communication across all supported targets (Android, Desktop). This provides a robust, Coroutine-based architecture for reliable device communication while remaining fully KMP compatible. See [core/ble/README.md](core/ble/README.md) for details.

## API & Integration

Developers can integrate with the Meshtastic Android app using our published API library via **JitPack**. This allows third-party applications (like the ATAK plugin) to communicate with the mesh service via AIDL.

For detailed integration instructions, see [core/api/README.md](core/api/README.md).

Additionally, the app includes a built-in **Local TAK Server** feature that can be enabled in settings. This runs a local TCP server on port 8089 to allow ATAK clients to connect directly and route their traffic over the mesh.

## Building the Android App
> [!WARNING]
> Debug and release builds can be installed concurrently. This is solely to enable smoother development, and you should avoid running both apps simultaneously. To ensure proper function, force quit the app not in use.

https://meshtastic.org/docs/development/android/

Note: when building the `google` flavor locally you will need to supply your own [Google Maps Android SDK api key](https://developers.google.com/maps/documentation/android-sdk/get-api-key) `MAPS_API_KEY` in `local.properties` in order to use Google Maps.
e.g.
```properties
MAPS_API_KEY=your_google_maps_api_key_here
```

## Repository Statistics

![Alt](https://repobeats.axiom.co/api/embed/1d75239069a6d671fe0b8f80b2e1bf590a98f0eb.svg "Repobeats analytics image")

Copyright 2025, Meshtastic LLC. GPL-3.0 license
