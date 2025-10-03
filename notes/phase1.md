### **Step 0: Project Setup & Foundation (The "Blueprint")**

This is the groundwork. Don't skip these steps, as they ensure your project is clean, scalable, and maintainable.

1.  **Initialize Flutter Project:**
      * Create your project: `flutter create golf_practice_app`
2.  **Version Control:**
      * Initialize Git: `git init`
      * Create a repository on GitHub or GitLab. This is non-negotiable for tracking changes and backing up your code.
3.  **Choose Your Architecture & State Management:**
      * **Folder Structure:** Organize your `lib` folder for clarity. A feature-first approach is excellent:
        ```
        lib/
        ├── src/
        │   ├── features/
        │   │   ├── auth/
        │   │   ├── drills/
        │   │   ├── history/
        │   │   └── dashboard/
        │   ├── common_widgets/
        │   ├── models/
        │   └── services/
        └── main.dart
        ```
      * **State Management:** For an app of this size, `Provider` or `Riverpod` are fantastic choices. They are simpler to grasp than BLoC and provide all the power you need. **Decision: Start with Riverpod.**
4.  **Setup App Navigation:**
      * Use a routing package like `go_router` to handle navigation declaratively.
      * Define your basic routes: `/login`, `/home` (dashboard), `/drills`, `/history`. This creates a skeleton of your app you can fill in.

-----

### **Step 1: Backend Setup & Data Modeling (The "Database")**

You need a place to store user data. **Firebase** is the perfect choice for an MVP due to its ease of use, generous free tier, and seamless integration with Flutter.

1.  **Create a Firebase Project:**
      * Go to the [Firebase Console](https://console.firebase.google.com/), create a new project.
      * Register your app for both iOS and Android within the Firebase project settings.
      * Follow the instructions to add the `google-services.json` (Android) and `GoogleService-Info.plist` (iOS) files to your Flutter project.
2.  **Add Firebase Dependencies:**
      * In your `pubspec.yaml`, add the core Firebase packages:
        ```yaml
        dependencies:
          firebase_core: ^...
          firebase_auth: ^...
          cloud_firestore: ^...
        ```
3.  **Define Your Data Models in Dart:**
      * Create Dart classes in your `lib/src/models/` directory. These will represent the data you store in Firestore.
      * **`user_model.dart`**: `uid`, `email`
      * **`drill_model.dart`**: `id`, `name`, `description`, `scoringMethod` (e.g., 'count', 'made\_out\_of\_total')
      * **`practice_session_model.dart`**: `id`, `userId`, `drillId`, `timestamp`, `score` (this could be a map like `{'made': 8, 'total': 10}`)
4.  **Populate Initial Data:**
      * In the Firebase console, manually create a `drills` collection in Firestore.
      * Add 5-10 documents to this collection, one for each of your pre-defined MVP drills. This is the "Drill Library" your app will read from.

-----

### **Step 2: User Authentication (The "Front Door")**

Users need a way to sign in and have their data saved to their account.

1.  **Enable Authentication Provider:** In the Firebase console, go to Authentication -\> Sign-in method and enable "Email/Password" and "Google Sign-In".
2.  **Build the UI:**
      * Create a `login_screen.dart`. It should have fields for email/password and buttons for "Login", "Sign Up", and "Sign in with Google".
3.  **Implement Auth Logic:**
      * Use the `firebase_auth` package to call methods like `createUserWithEmailAndPassword`, `signInWithEmailAndPassword`, and handle the Google Sign-In flow.
4.  **Manage Auth State:**
      * In your main app widget, use a `StreamProvider` from Riverpod to listen to `FirebaseAuth.instance.authStateChanges()`.
      * This stream will determine whether to show the `LoginScreen` or the `DashboardScreen`. This is the core logic that directs logged-in vs. logged-out users.

-----

### **Step 3: Core Feature Flow - Logging a Drill (The "Main Event")**

This is the most critical part of the user experience. It must be fast and intuitive.

1.  **Drill List Screen:**
      * Create a `drills_screen.dart`.
      * Fetch the `drills` collection from Firestore and display the documents in a `ListView.builder`.
      * Each list item should be tappable.
2.  **Score Entry Screen:**
      * When a user taps a drill, navigate them to a `score_entry_screen.dart`, passing the selected `Drill` object.
      * Display the drill's name and description.
      * **Design the input UI for simplicity:** Use large `+` and `-` buttons, a numeric keypad, or sliders. Avoid tiny text fields. For a "Made out of 10" drill, you might just have a row of 10 tappable circles.
      * Have a prominent "Save Practice" button.
3.  **Save the Data:**
      * When the user taps "Save Practice", create a `PracticeSession` object.
      * Populate it with the `userId` (from `FirebaseAuth.instance.currentUser`), the `drillId`, the current `timestamp`, and the score data from the UI.
      * Save this object as a new document in a `practice_sessions` collection in Firestore.
      * After a successful save, automatically navigate the user to the history screen to see their new entry. This provides immediate positive reinforcement.

-----

### **Step 4: Displaying Progress (The "Payoff")**

This is where users see the value of their efforts.

1.  **Practice History Screen:**
      * Create a `history_screen.dart`.
      * Query the `practice_sessions` collection, using `.where('userId', isEqualTo: currentUser.uid)` to fetch only the logged-in user's sessions. Order them by timestamp.
      * Use a `StreamBuilder` or `FutureBuilder` to display the sessions in a list, showing the drill name, score, and date for each.
2.  **Basic Dashboard Screen:**
      * This will be the `home_screen.dart`.
      * Fetch all of the user's practice sessions.
      * Pick one or two key drills and create a simple visual.
      * Add the `fl_chart` package to your `pubspec.yaml`.
      * Create a `LineChart` widget that plots the score of a specific drill over time (Score on the Y-axis, Date on the X-axis). This is the MVP's "killer feature" as it visually validates the user's hard work.

By following these four steps, you will have built a complete, functional MVP that solves the core problem for your target audience. You can then gather feedback and confidently move on to Phase 2.