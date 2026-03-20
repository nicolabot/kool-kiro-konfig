# Mobile Developer (iOS) Guidelines

## Architecture — MVVM

- Use MVVM (Model-View-ViewModel) for all screens
- **View**: SwiftUI `View` structs — declarative UI only, no business logic
- **ViewModel**: `@Observable` class (or `ObservableObject` with `@Published`) — owns state and business logic
- **Model**: Plain Swift structs conforming to `Codable` for API data

## SwiftUI Conventions

- Prefer SwiftUI-native components over UIKit wrappers
- Use `@State` for view-local state, `@Environment` for dependency injection
- Extract reusable views into separate files when they exceed ~50 lines
- Use `ViewModifier` for shared styling logic
- Always provide `Preview` blocks for every view

## Human Interface Guidelines (HIG)

- Follow Apple’s HIG for layout, typography, and iconography
- Use SF Symbols for icons
- Support Dynamic Type for all text
- Respect safe areas and device-specific layouts
- Support both light and dark mode using semantic colors

## Navigation

- Use `NavigationStack` with typed navigation paths
- Define routes as an enum conforming to `Hashable`
- Keep navigation logic in the ViewModel, not the View
- Use `.sheet()` for modal presentations, `.navigationDestination()` for push navigation

## API Integration

- Use `URLSession` with `async/await` for network calls
- Define a centralized `APIClient` with base URL and auth token handling
- All request/response types conform to `Codable`
- Handle errors with a typed `APIError` enum (network, decoding, server, unauthorized)
- Show loading indicators during requests and meaningful error messages on failure
- Integrate with the AnyCompany backend REST API for transaction data and categorization results

## Local Storage

- Use `UserDefaults` for small preferences (settings, feature flags)
- Use `SwiftData` or Core Data for structured local data
- Never store sensitive data (tokens, PII) in `UserDefaults` — use Keychain

## Testing

- Unit test ViewModels with mock dependencies
- Test `Codable` conformance for all API models
- Use `Preview` blocks as visual smoke tests
- Integration tests for critical user flows
