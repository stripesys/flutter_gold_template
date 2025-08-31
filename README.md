# Flutter Gold Template

1. **Overview & Vision**
To create a robust, scalable, and highly configurable Flutter Gole Template designed to be the foundation for multiple commercial-grade applications. The template will enforce best practices, eliminate common initialization pitfalls, and provide a modular architecture for rapid feature development. It will support multiple flavors, extensive Firebase integration, and a full suite of enterprise-ready features out of the box.

2. **Core Technical Requirements**
**Flutter Version**: Stable channel, supporting null-safety and latest Dart SDK.
**Platforms**: Android, iOS, Web (with platform-specific configurations pre-wired).
**Architecture**: Clean Architecture / Feature-First layered architecture.
**State Management**: Riverpod (Provider) as the primary, official solution for its scalability, testability, and flexibility over other options.
**Dependency Injection**: A combination of Riverpod and GetIt for services that are not context-aware.

3. **Detailed Feature Modules**
3.1. **Flavors & Build System**
**Multiple Flavors**: Pre-configured for at least three flavors (e.g., provider, consumer, rider).
**Environment Config**: JSON/Dart class-based configuration per flavor (App Name, Bundle ID, Firebase options, API endpoints, feature flags).
**Build Scripts**: Scripts (build_android.sh, build_ios.sh) to automate building APK/IPA for all flavors and environments (dev, staging, prod).

3.2. **Centralized Initialization & Service Registry (CRITICAL)**
**AppStartupManager**: A single, centralized class to orchestrate the initialization sequence. This solves the race conditions and double-initialization problems identified.
**Initialization Phases**: Breaks startup into clear, ordered phases:
**Core Framework**: WidgetsFlutterBinding, SharedPreferences.
**Platform Config**: SystemChrome, DevicePreview (dev-only).
**Firebase Core**: Initialize all Firebase services using flavor-specific config.
**Data & Storage**: Local DB, Network Clients.
**Business Services**: Session, Connectivity, etc.
**UI & Presentation**: Theme, Localization.
**Service Health Checks**: Each service must report its initialization status. The AppStartupManager can halt boot-up if a critical service fails.
**Singleton Pattern**: All major services must be implemented as true singletons, accessible through a central service locator (GetIt) or a dedicated provider.

3.3. **Firebase Ecosystem (Full Suite)**
**Core**: Initialized with per-flavor GoogleService-Info.plist/json files.
**Authentication**: Support for Email/Password, Phone OTP, Google Sign-In, Apple Sign-In, Facebook Login, and Custom Token authentication. Includes a QR Code Login handler service.
**Firestore & Realtime DB**: With offline persistence enabled and configured.
**Cloud Messaging (FCM)**: Full push notification setup. Handles foreground, background, and terminated states. Includes topic subscription based on user role/flavor.
**Analytics & Crashlytics**: Automatic screen tracking, custom event logging, and uncaught error reporting.
**App Check**: To protect backend resources from abuse. Debug token setup for development.
**Remote Config**: For feature toggling and A/B testing, with offline default values.

3.4. **State Management & Dependency Injection**
**Riverpod Providers**: For all UI state and services that need to be context-aware.
 - ThemeProvider (light, dark, system)
 - LanguageProvider (notify about locale changes)
 - AuthProvider (stream of user auth state)
 - AppConfigProvider (flavor & environment info)
**GetIt Service Locator**: For global, non-reactive services (e.g., LocalDatabaseService, ApiClient).

3.5. **User Interface & Experience (UI/UX)**
**Theming**:
 - Full Material 3 (Material You) theming system.
 - Provider for ThemeMode (light, dark, system).
 - Pre-defined TextTheme, ColorScheme, ButtonStyles.
**Localization & Internationalization (i18n)**:
 - Support for English, Hindi, Spanish, French, Arabic, Chinese, Japanese using flutter_localizations and ARB files.
 - Gender & Pitch-aware Voice Support: Architecture in place for integrating TTS (Text-to-Speech) services. Includes a VoiceSettings model (language, gender: male/female, pitch) that can be passed to a TextToSpeechService.
**Common Widgets Library**: A suite of reusable widgets (AppBars, buttons, loaders, error states, empty states) that adhere to the defined theme.
**Routing**: Uses go_router or auto_route for a type-safe, declarative routing solution with deep linking support.

3.6. **Data Layer & Storage**
**Local Storage**:
 - SharedPreferences: For simple key-value pairs (theme, locale).
 - Hive/Isar: A performant NoSQL database for complex objects and caching.
**Network Layer**:
 - Dio Client with interceptors for:
    - Automatic auth token attachment.
    - Request/Response logging.
    - Error handling and parsing.
    - Refresh token logic.
**Offline Support**: Services to sync local DB (Hive/Isar) with Firestore/Realtime DB when connectivity is restored.

3.7. **Application Services**
**SessionManager**: Handles user login, logout, and provides a stream of authentication state.
**ConnectivityService**: Monitors network connection and triggers offline/online mode.
**LocationService**: Handles fetching and listening to the device's location (using geolocator package).
**BiometricService**: Provides support for local authentication via biometrics (fingerprint/face ID).

3.8. **Profile Management**
**Data Model**: A complete UserProfile data class (name, email, phone, address, avatar URL, location coordinates, etc.).
**CRUD Operations**: Services and UI forms to View, Edit, and Update user profile information, syncing with Firestore.

3.9. **Logging, Analytics & Monitoring**
**AppLogger**: A wrapper class around the logger package for consistent, leveled logging (debug, info, warning, error). Configurable to log to console and/or Crashlytics.
**Firebase Analytics Service**: A dedicated service to track custom events and user properties.
**Performance Monitoring**: Basic setup for tracking screen rendering times.

3.10. **Development & Debugging Tools**
**Device Preview**: Integrated for easy multi-device UI testing during development.
**Debug Banner**: Conditionally shown based on flavor (e.g., shows "DEV - CONSUMER").
**Debug Menu**: A hidden/secret trigger (e.g., tap version number 10 times) to open a debug overlay for:
 - Switching API base URLs.
 - Overriding user roles.
 - Viewing logs.
 - Triggering fake errors.

4. **Proposed Folder Structure (Feature-Based)**
CLEAN Architecture, RiverPod, GetIt

5. **Implementation & Delivery Phases**
Phase 1: Foundation (Week 1-2): Flavors, Build System, Core Framework Init, Service Registry, Routing, Theming.
Phase 2: Firebase & Auth (Week 3): Centralized Firebase init, All Auth Methods (including QR), FCM, Crashlytics.
Phase 3: Data & State (Week 4): Riverpod setup, Network Layer (Dio), Local DB (Hive/Isar), Profile Management.
Phase 4: Advanced Features (Week 5): i18n (with voice settings scaffolding), Location, Deep Linking, Debug Tools.
Phase 5: Polish & Documentation (Week 6): Comprehensive README.md, example screens, bug fixes, and sample build scripts.

6. **Success Metrics**
**Reliability**: No initialization race conditions or crashes on cold start.
**Performance**: App startup time (Time to Interactive) under 2 seconds on a mid-range device.
**Maintainability**: Adding a new feature or flavor requires changes in only one obvious location.
**Completeness**: The boilerplate can be cloned and have a basic auth-flow with a profile screen running in under 15 minutes.

This boilerplate will be an invaluable asset, drastically reducing project setup time from weeks to hours while ensuring a high standard of code quality and architectural consistency across all projects.
