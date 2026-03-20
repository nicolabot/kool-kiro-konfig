---
name: screen-scaffolding
description: Scaffold a new iOS screen with SwiftUI view, ViewModel, model, and preview following AnyCompany MVVM conventions and Human Interface Guidelines.
---

## Steps

1. Ask which screen to create and its purpose
2. Create a directory under the appropriate feature folder
3. Generate:
   - `ScreenNameView.swift` — SwiftUI view with `@State` ViewModel binding, semantic layout, and HIG-compliant styling
   - `ScreenNameViewModel.swift` — `@Observable` class with state properties and async methods
   - `ScreenNameModel.swift` — `Codable` structs for any API data this screen uses
4. Add a `#Preview` block to the view file
5. Wire the screen into the app’s navigation route enum
6. Present the generated files for review
