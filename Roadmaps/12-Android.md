
---

Since Android is your **side skill** and your goal shouldn't be "learn every Android topic." It should be **learn just enough to build increasingly better apps**.

The biggest mistake beginners make is spending months watching tutorials without building anything.

Your approach—**learn → build → learn → build**—is exactly how I'd recommend doing it.

---

# Phase 1: Kotlin Essentials (1–2 weeks)

## Learn

- Variables
    
- Functions
    
- OOP
    
- Null safety
    
- Collections
    
- Generics (basic)
    
- Lambdas
    
- Data classes
    

Don't spend weeks on advanced Kotlin.

## Build

Build 3 console projects:

- Expense Tracker
    
- Student Management System
    
- Library Management System
    

No Android yet.

---

# Phase 2: Android Basics (1 week)

## Learn

Just enough to understand Android:

- Project structure
    
- Activity
    
- Lifecycle (basic)
    
- Intents
    
- Resources
    
- Manifest
    
- Gradle (don't go deep)
    

Ignore Fragments for now unless you encounter them later.

## Build

A simple app with:

- Multiple screens
    
- Passing data
    
- Buttons
    
- Images
    

Example:

- Birthday Wish App
    
- Quiz App
    
- Profile App
    

---

# Phase 3: Jetpack Compose (2 weeks)

This is where modern Android starts.

## Learn

- Composable functions
    
- Text
    
- Button
    
- Image
    
- Column
    
- Row
    
- Box
    
- LazyColumn
    
- TextField
    
- State
    
- Navigation
    

Don't learn animations yet.

## Build

Examples:

- Calculator
    
- Notes UI
    
- Recipe App UI
    
- Weather UI
    
- Spotify Clone UI
    

No backend.

---

# Phase 4: State + Architecture (1 week)

## Learn

- State
    
- State hoisting
    
- ViewModel
    
- MVVM basics
    

Don't dive into Clean Architecture yet.

## Build

A To-Do App with:

- Add tasks
    
- Delete tasks
    
- Mark complete
    
- Search
    

---

# Phase 5: Networking (1 week)

## Learn

- REST API
    
- Retrofit
    
- Coroutines
    
- JSON
    
- Loading
    
- Error handling
    

## Build

Choose one:

- Weather App
    
- Movie App
    
- News App
    
- Pokémon App
    

Real API only.

---

# Phase 6: Local Storage (1 week)

## Learn

- Room Database
    

Ignore SQLite directly.

## Build

Expense Tracker

or

Offline Notes App

---

# Phase 7: Authentication (1 week)

## Learn

Firebase Authentication

or

Your own backend if you want to connect it with your MERN skills.

## Build

Login/Register App

---

# Phase 8: Advanced Compose

Now learn only what's useful:

- Material 3
    
- Dark Mode
    
- Bottom Navigation
    
- Top App Bar
    
- Dialogs
    
- Snackbars
    
- Floating Action Button
    

## Build

Shopping App UI

---

# Phase 9: Dependency Injection

## Learn

- Hilt
    

That's enough.

## Build

Refactor one previous app using Hilt.

---

# Phase 10: Polish

Learn only what you actually need while building:

- Permissions
    
- Camera
    
- Location
    
- File Picker
    
- Notifications
    
- WorkManager (if your app needs background work)
    

Don't study these in isolation.

---

# Topics you can skip initially

These are useful in some projects but not worth learning early:

- ❌ XML (just know the basics if you encounter an older project)
    
- ❌ Fragments (Compose navigation is the modern approach)
    
- ❌ Data Binding
    
- ❌ LiveData (prefer `StateFlow` in new projects)
    
- ❌ AsyncTask (obsolete)
    
- ❌ MVP/MVC
    
- ❌ Java Android development
    

---

# Projects to build (in order)

1. Expense Tracker (console)
    
2. Student Management (console)
    
3. Quiz App
    
4. Calculator
    
5. To-Do App
    
6. Weather App (API)
    
7. Movie App (API)
    
8. Notes App (Room)
    
9. Expense Tracker (Room)
    
10. Chat App (Firebase or your own backend)
    
11. E-commerce App
    
12. Spotify Clone
    
13. Food Delivery App
    

Each project should introduce one or two new concepts rather than everything at once.

---

# Final roadmap

```text
Kotlin
        ↓
Android Basics
        ↓
Compose
        ↓
Build UI Apps
        ↓
ViewModel + MVVM
        ↓
Build To-Do App
        ↓
Retrofit + APIs
        ↓
Build Weather/Movie App
        ↓
Room Database
        ↓
Build Notes App
        ↓
Authentication
        ↓
Build Login App
        ↓
Hilt
        ↓
Build Production-style App
```

This roadmap focuses on the technologies that are common in modern Android development while avoiding older approaches unless you need them for maintaining existing code. It also fits well with your MERN background because you'll be able to connect Android apps to your own backend APIs instead of relying only on Firebase.