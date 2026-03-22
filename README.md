<div align="center">
  <img width="201" height="232" alt="Quizazu" src="https://github.com/user-attachments/assets/9fc465b9-da37-4954-9da7-b0cf8b38663c" />
  <h1>Quizazu | College quiz mobile app</h1><div align="center">


![Platform](https://img.shields.io/badge/Platform-Android-3DDC84?logo=android&logoColor=white)
![Min SDK](https://img.shields.io/badge/Min%20SDK-31-3DDC84?logo=android&logoColor=white)
![Java](https://img.shields.io/badge/Java-11-ED8B00?logo=openjdk&logoColor=white)
![JWT](https://img.shields.io/badge/Auth-JWT-000000?logo=jsonwebtokens&logoColor=white)
![Real-Time](https://img.shields.io/badge/Real--Time-SignalR-512BD4?logo=microsoft&logoColor=white)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](https://choosealicense.com/licenses/mit/)
</div>
</div> 

**Quizazu Android app** is the native mobile frontend for the **[College Quiz (Quizazu)](https://github.com/patrick012016/college-quiz-quizazu)** platform. Engineered for interactive, real-time competitive gameplay, the app leverages bi-directional communication to ensure ultra-low latency state synchronization and seamless participation in live quiz sessions.
This repository houses the source code for the Java-based Android application. It serves as a core component of the broader Quizazu cross-platform ecosystem, alongside a React & Razor web client and a centralized ASP.NET WebAPI backend.

## Table of contents

* [Gallery](#gallery)
* [Features](#features)
* [Built with](#built-with)
* [Architecture & app flow](#architecture--app-flow)
* [Prerequisites & setup](#prerequisites--setup)
* [License](#license)

## Gallery

<div align="center">

**Home screen** &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 
**Join screen**<br>
<img width="290" alt="Quizazu home screen" src="https://github.com/user-attachments/assets/ea78d50f-43eb-43ac-a245-6c27244d7da3" />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img width="290" alt="Join game screen with code input and QR scanner" src="https://github.com/user-attachments/assets/d9c050ff-2c5d-4209-a9b9-005aeb07bf88" />

<br> 
&nbsp; 

**Player gameplay screen (4 options)**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Player gameplay screen (6 options)**<br>
<img width="290" alt="Gameplay screen with 4 answer options" src="https://github.com/user-attachments/assets/1351a3bc-d28b-4192-9c58-3271a0b6dcb9" />
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img width="290" alt="Gameplay screen with 6 answer options" src="https://github.com/user-attachments/assets/6226d45c-4227-43fc-b0b2-1c7753a639cc" />

</div>


## Features

* **Secure authentication & auto-login:** Users log in via a REST API, utilizing JWT (JSON Web Tokens) to secure API calls and quiz room entries. The resulting token is securely stored locally to enable auto-login, bypassing the login screen on subsequent app launches.
* **Real-time gameplay sync:** Powered by Microsoft SignalR for low-latency bidirectional communication. The app reacts instantly to server events (hub methods) such as `onGame`, `onQuestionTimer`, `onCorrectAnswer`, and `onQuestionResult`.
* **Dynamic game modes:** The app dynamically renders native UI fragments based on the incoming question type. Supported modes include:
    * `SINGLE_FOUR_ANSWERS` (4 Options)
    * `MULTIPLE_FOUR_ANSWERS` (Multiple Choice)
    * `SINGLE_SIX_ANSWERS` (6 Options)
    * `TRUE_FALSE` (True / False)
    * `RANGE` (Range Slider Questions)

* **Quick join via QR code:** Integrated with the `Quickie` library for fast lobby joining via camera.
* **Live timers & results:** Synchronized countdown timers and immediate visual feedback on correct/incorrect answers directly on the device.

## Built with

### Core Android

* **Java 11:** Main programming language.
* **Android SDK:** Minimum SDK 31, Target SDK 33.
* **AndroidX & Material Design:** For modern UI components (`ConstraintLayout`, `GridLayout`, `RangeSlider`, `CardView`).

### Third-party libraries

* [**SignalR Client** (`com.microsoft.signalr:7.0.5`)](https://learn.microsoft.com/en-us/aspnet/core/signalr/java-client) - Full-duplex WebSocket communication for real-time game state updates.
* [**OkHttp3** (`com.squareup.okhttp3:okhttp:4.10.0`)](https://square.github.io/okhttp/) - HTTP client for initial REST API handshakes and joining rooms.
* [**Gson** (`com.google.code.gson:2.10.1`)](https://github.com/google/gson) - Serialization and deserialization of JSON WebSocket payloads and DTOs.
* [**Quickie** (`io.github.g00fy2.quickie-bundled:1.6.0`)](https://github.com/G00fY2/quickie) - High-performance QR code scanning for joining quizzes.

## Architecture & app flow

The application flow is broken down into modular, core activities:

1. **`StartActivity`**: Displays a splash screen while the app initializes.
2. **`MainActivity`**: Manages user authentication. It utilizes `MotionLayout` for UI state transitions during network calls and handles invalid credentials.
3. **`MenuActivity`**: The main hub post-login. It hosts the `BottomNavigationView` to swap out main application fragments and manages the logout sequence.
4. **`LobbyActivity`**: Connects to the specific quiz room using a JWT-secured OkHttp request, then spins up the SignalR hub connection. It maintains a live countdown until the host starts the game.
5. **`Quiz_Activity`**: The core gameplay engine. It listens to WebSocket events and dynamically injects the appropriate UI Fragments (e.g., `FourAnswersFragment`, `TrueFalseFragment`, `SliderFragment`) into its FrameLayout depending on the `QuizDto` payload.
6. **`ResultActivity`**: Shows scorecards with point gains and streaks between questions or at the end of the quiz, using `ResultDto` data.

## Prerequisites & setup

To build and run this project, you will need:

* **Android Studio** (Electric Eel or newer recommended)
* **JDK 11**
* An active instance of the **Quizazu ASP.NET WebAPI** running locally or deployed (e.g., on MS Azure) to act as the SignalR broker.

1. Clone the repository:
    ```bash
    git clone https://github.com/patrick012016/CollegeQuizMobileApp.git
    ```


2. Open the project in Android Studio.
3. Ensure your backend environment variables in your `Constants` class point to your active ASP.NET server.
4. Sync Gradle and run the app on an emulator or physical device (API 31+).

## License

This project is licensed under the **[MIT License](https://choosealicense.com/licenses/mit/)**.
 
