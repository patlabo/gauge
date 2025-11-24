# 🌊 Live oil pressure PSI Gauge (Web Bluetooth)

This is a single-file, highly responsive web application designed to connect to an ESP32 microcontroller broadcasting pressure sensor readings (PSI) over Bluetooth Low Energy (BLE).

The application uses the Web Bluetooth API to create a live, real-time gauge view accessible on mobile devices.

## 🔗 Live Application Link

The application is hosted securely via HTTPS, which is mandatory for Web Bluetooth functionality.

**[Insert Your Live GitHub Pages URL Here]**

---

## 🛠️ Requirements & Setup

### 1. ESP32 Configuration

This web app is configured to communicate with the ESP32 using the standard **Nordic UART Service (NUS)** UUIDs. Your ESP32 Arduino sketch **must** use these specific UUIDs and transmit the PSI value as a simple UTF-8 encoded string (e.g., `"105.4"`).

| Component | UUID | Description |
| :--- | :--- | :--- |
| **Service** | `6e400001-b5a3-f393-e0a9-e50e24dcca9e` | Nordic UART Service |
| **RX Characteristic** | `6e400003-b5a3-f393-e0a9-e50e24dcca9e` | This characteristic must be configured to send **NOTIFICATIONS**. |

### 2. Mobile Browser Requirement (iOS Critical)

Due to Apple's strict policies on Web Bluetooth implementation, standard browsers like Chrome or Safari on iOS often do not fully support the API, or they block the device selection prompt.

**You MUST use a dedicated Web Bluetooth browser on iOS:**

* ✅ **Recommended:** **Bluefy** (Free on the App Store).
* ✅ **Alternative:** Microsoft Edge (sometimes works better than Chrome on iOS).

On Android or Desktop (Windows/Mac), Chrome and Edge generally work without issue.

---

## 🚀 How to Use

1.  **Power On:** Ensure your ESP32 device is powered on and actively advertising the BLE service.
2.  **Open App:** Open the **Live Application Link** above in your mobile browser (ideally Bluefy).
3.  **Connect:** Tap the **"Connect to ESP32 Gauge"** button. Your browser will prompt you to select the ESP32 device from the list of nearby Bluetooth devices.
4.  **Live Data:** Once connected, the PSI value will update in real-time as your ESP32 transmits new readings (typically at 10-50 Hz).

---

## ✨ Mock Data Mode

The application includes a built-in mock data mode for quick testing and UI responsiveness checks without needing the physical ESP32.

* Tap the **"Start Mock Data"** button to simulate a high-speed data stream ($10$ Hz) with fluctuating PSI values between 0.0 and 150.0.
* Tap the **"Stop Mock Data (Live)"** button to switch back to the connection screen.

---

## 💻 Technical Stack

* **HTML:** Structure
* **Tailwind CSS:** Fully responsive, modern styling for mobile and desktop views.
* **JavaScript:** Core logic for Web Bluetooth API connections, GATT discovery, and real-time data handling.

---
