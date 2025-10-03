# Copilot Instructions

## About this Project

This is a Flutter application for mobile, web, and desktop. The main application code is written in Dart and is located in the `lib/` directory.

## Getting Started

The project is based on the standard Flutter template. The main entry point is `lib/main.dart`.

- To run the application, use the `flutter run` command.
- To run tests, use `flutter test`. Widget tests are in `test/widget_test.dart`.

## Architecture

The app follows a basic Flutter architecture.

- The root of the application is the `MyApp` widget in `lib/main.dart`, which sets up the `MaterialApp`.
- The main UI is contained within the `MyHomePage` stateful widget.
- State is managed locally within widgets using `StatefulWidget` and `setState`.

## Dependencies

- The project uses the standard Flutter SDK.
- UI components are primarily from the Material Design library (`uses-material-design: true` in `pubspec.yaml`).
- Icons from `cupertino_icons` are available.

## Developer Workflow

- **Hot Reload:** A key feature of Flutter development. After making changes, use "hot reload" (press "r" in the terminal running `flutter run`, or use the feature in your IDE) to see changes instantly without restarting the app.
- **State Management:** For simple changes, using `StatefulWidget` and `setState` is acceptable. For more complex state, consider using a state management library like Provider or BLoC.
- **Linting:** The project uses `flutter_lints` to enforce coding standards. Ensure your code adheres to these lints. Check `analysis_options.yaml` for configured rules.
