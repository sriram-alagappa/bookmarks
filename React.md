# React Native Execution Flow (Hermes Engine)

This document outlines the lifecycle of a React Native application from the moment the user taps the icon to the rendering of native UI components on the screen.

## Architecture Overview

React Native allows you to write mobile applications using JavaScript, but unlike a web view-based app, it renders **actual native components**. This is achieved through a bridge or a direct interface (JSI) between the JavaScript environment and the Native platform (Android/iOS).

---

## The Execution Pipeline

### 1. App Launches
The Android OS creates a new process for the application. The main thread (often called the **UI Thread**) is initialized, and the APK starts loading its resources.

### 2. Hermes JS Engine Starts
Instead of using a generic web engine, the app initializes the **Hermes engine**. 
* **Context:** Hermes is a JavaScript engine optimized for React Native.
* **Location:** It is bundled directly inside your APK.
* **Optimization:** It focuses on reducing app size and memory consumption while decreasing the time it takes for the app to become interactive (TTI).

### 3. Hermes Executes JS Bundle
Hermes loads the bundled JavaScript file (e.g., your `index.bundle` which might be ~2.6MB).
* **Pre-compilation:** One of Hermes' strengths is that it uses **Bytecode pre-compilation**. Instead of parsing the full 2.6MB of JavaScript at runtime, it executes pre-compiled bytecode, which is much faster.

### 4. JavaScript Calls the Bridge (or JSI)
As your React code runs, it calculates what the UI should look like. However, JavaScript cannot directly "draw" a button on an Android screen. It sends a message or a set of instructions.
* **The Command:** "Create a View with these styles and this text."

### 5. Bridge Calls Native Android APIs
The message from the JS environment is passed to the **Native Side**.
* In older versions of React Native, this happens over the **Bridge** (serialized JSON).
* In newer versions (Architecture with Fabric/TurboModules), this happens via **JSI (JavaScript Interface)**, allowing for faster, synchronous communication.

### 6. Real Native UI Renders
The Android system receives the commands from the bridge and invokes the standard `android.view` APIs.
* **Result:** The user sees actual Android `ViewGroup`, `TextView`, and `ImageView` components, not a browser-based simulation.

---

## Summary Diagram

```mermaid
graph TD
    A[App Launches] --> B[Hermes JS Engine Starts]
    B --> C[Hermes Executes 2.6MB JS Bundle]
    C --> D[JS calls React Native Bridge/JSI]
    D --> E[Bridge calls Native Android APIs]
    E --> F[Real Native UI Renders]
