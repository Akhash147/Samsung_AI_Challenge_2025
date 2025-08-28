# Technical Documentation

## Project: BoostAI – Smart Battery and GPS Optimization App

### 1. Introduction
BoostAI is an Android application designed to intelligently manage battery performance, GPS usage, and display adjustments by monitoring environmental factors and system conditions. Its primary goal is to automatically optimize device resources when Google Maps is running, without requiring manual intervention.

### 2. Features Overview

1. **Automatic Google Maps Monitoring:**
   * Detects when Google Maps is running (foreground or background).
   * Once detected, automatically triggers all optimizations.
   * Does not display status to the user; runs silently.

2. **AI Optimization Toggle:**
   * Manual ON/OFF button starts or stops all monitoring.
   * ON → Requests location permission first, then monitors Maps.
   * OFF → Stops all background processes immediately.

3. **Battery Information & Optimization:**
   * Monitors battery level in real-time.
   * Applies energy-saving adjustments automatically when Maps is running.

4. **Dynamic Light Adjustment:**
   * Uses ambient light sensor to adjust screen brightness.
   * Brighter screen in high lux, dimmer in low lux.

5. **GPS Accuracy Mode Switching:**
   * Monitors device speed via GPS.
   * Speed > 65 km/h → Low-accuracy mode to save power.
   * Stationary → High-accuracy mode for precision.

### 3. Technical Stack

| Technology                                        | Purpose                         | Link                                                                                |
| ------------------------------------------------- | ------------------------------- | ----------------------------------------------------------------------------------- |
| Android (Java/Kotlin)                             | Core application development    | [Android Developers](https://developer.android.com/)                                |
| ActivityManager                                   | Detect running processes        | [Docs](https://developer.android.com/reference/android/app/ActivityManager)         |
| UsageStatsManager                                 | Detect Maps usage               | [Docs](https://developer.android.com/reference/android/app/usage/UsageStatsManager) |
| SensorManager                                     | Ambient light sensor access     | [Docs](https://developer.android.com/reference/android/hardware/SensorManager)      |
| LocationManager / FusedLocationProviderClient    | GPS location & speed tracking   | [Docs](https://developers.google.com/location-context/fused-location-provider)      |
| Handler & Looper                                  | Background task scheduling      | [Docs](https://developer.android.com/reference/android/os/Handler)                  |

### 4. Architecture

```
+------------------------------------------------------+
|                      BoostAI App                     |
+------------------------------------------------------+
| MainActivity                                         |
|  - ON/OFF AI Optimization Button                     |
|  - Requests Location Permission                      |
|  - Automatically monitors Maps                       |
|  - Triggers Dynamic Light & GPS optimizations        |
+------------------------------------------------------+
| Android Services                                     |
|  - SensorManager (Light Sensor)                      |
|  - LocationManager / FusedLocationProvider           |
|  - AppOpsManager (Permissions Check)                 |
+------------------------------------------------------+
```

### 5. Implementation Details

#### 5.1 Automatic Google Maps Detection
* Hybrid approach using:
  1. `ActivityManager.getRunningAppProcesses()`
  2. `UsageStatsManager.queryUsageStats()`
* Once Maps is detected, all optimizations start automatically.

#### 5.2 Dynamic Light Adjustment
* Registers `SensorEventListener` for TYPE_LIGHT.
* Adjusts brightness dynamically:
```kotlin
val brightness = when {
    lux > 1000 -> 1.0f
    lux > 500 -> 0.7f
    else -> 0.4f
}
```

#### 5.3 GPS Accuracy Switching
* Measures speed via LocationManager or FusedLocationProviderClient.
* Speed > 65 km/h → Low-power mode (NETWORK_PROVIDER).
* Stationary → High-accuracy GPS mode.

#### 5.4 AI Optimization Toggle
* ON → Requests location permission first.
* Once granted → Starts automatic Maps monitoring and all optimizations.
* OFF → Stops all services and handlers immediately.

### 6. Installation Instructions

1. Clone or download the project.
2. Open in Android Studio.
3. Add the following permissions in `AndroidManifest.xml`:
```xml
<uses-permission android:name="android.permission.PACKAGE_USAGE_STATS"/>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
<uses-permission android:name="android.permission.BATTERY_STATS"/>
<uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>
```
4. Enable Usage Access Permission manually in phone settings for BoostAI.

### 7. User Guide

1. Press Turn ON AI Optimization.
2. Grant Location Permission when prompted.
3. Once permission is granted:
   * App silently detects Google Maps.
   * Automatically applies battery optimization, GPS switching, and dynamic light adjustment.
4. Press Turn OFF to stop all optimizations.

### 8. Salient Features

* Fully automatic optimization triggered by Google Maps.
* Dynamic screen brightness adjustment.
* GPS power-saving mode based on speed.
* Real-time battery monitoring and optimization.
* User-friendly interface requiring minimal interaction.

