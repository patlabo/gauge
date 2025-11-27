# 🛠️ Live Oil Pressure Gauge (ESP32 BLE & Web Client)

This project provides a real-time oil pressure monitoring solution using an ESP32 microcontroller, a 0-150 PSI pressure sensor, an OLED display, and a modern Web Bluetooth (BLE) client.

The ESP32 reads the analog sensor data, applies signal processing (voltage scaling and moving average filtering), and broadcasts the final PSI value over Bluetooth Low Energy. The web application allows any standard smartphone or computer browser to connect and display the data live.

## 🎯 Key Features

* **Real-Time Monitoring:** Live PSI readings transmitted every 100ms.
* **Web Bluetooth Client:** Connect via any modern web browser (Chrome, Edge, Opera) on desktop or mobile.
* **Signal Processing:** Includes voltage divider scaling and a 10-point moving average filter for smooth, stable data.
* **Safety Clamping:** Readings are automatically clamped between 0.0 PSI and 150.0 PSI to handle noise and sensor errors.
* **OLED Display:** Local 128x64 SSD1306 display shows the current PSI and BLE connection status.

## 📱 iOS & Data Logging (Bluefly Integration)

For iOS users who need dedicated data logging and video overlay, the **Bluefly** app is the recommended client.

### Advantages for Track/Enthusiast Use

This solution provides a highly cost-effective alternative to proprietary logging devices:

* **BRZ/GR86 Oil Starvation Monitoring:** By displaying the live oil pressure on an iOS device, you can use a **GoPro** (or similar camera) to capture the screen overlay during high-G cornering. This is crucial for logging and visualizing oil pressure drops (oil starvation events) without needing complex dash integrations.
* **Cost-Effective Datalogging:** This approach eliminates the need for expensive, dedicated hardware like CAN bus dumpers, complex OBD2 dongles, or devices such as the Aim Solo 2 DL, allowing you to log and export custom sensor data directly on your phone.

### Bluefly Setup

1.  **Install Bluefly:** Download the app from the iOS App Store.
2.  **Configure Service:** Add a new BLE peripheral and configure the service and characteristic UUIDs to match those defined below.

## ⚙️ Hardware & Wiring

### Components Required

| Component | Description |
| :--- | :--- |
| **Microcontroller** | ESP32 Dev Module (e.g., ESP32-WROOM-32) |
| **Sensor** | 0-150 PSI Pressure Sensor (0.5V - 4.5V output) |
| **Display** | 128x64 SSD1306 OLED (I2C) |
| **Resistors** | 3.6 kΩ and 4.5 kΩ (for the voltage divider) |

### Voltage Divider Configuration

The 0-4.5V sensor output is reduced to a maximum of **3.3V** (the ESP32's maximum safe ADC input) using a voltage divider.

**Formula for Voltage Divider Output ($V_{\text{measured}}$):**

$$V_{\text{measured}} = V_{\text{sensor}} \times \frac{R_{\text{lower}}}{R_{\text{upper}} + R_{\text{lower}}}$$

* $V_{\text{measured}} = V_{\text{sensor}} \times \frac{4.5\text{ k}\Omega}{3.6\text{ k}\Omega + 4.5\text{ k}\Omega}$
* $V_{\text{measured}} \approx V_{\text{sensor}} \times 0.5555$

| Resistor | Value Used |
| :--- | :--- |
| **$R_{\text{upper}}$** | 3.6 kΩ (Connects sensor V out to ADC) |
| **$R_{\text{lower}}$** | 4.5 kΩ (Connects ADC to GND) |

The scaling factor implemented in the firmware is approximately **0.5555**.

### Wiring Connections

| Component Pin | Connects To | Notes |
| :--- | :--- | :--- |
| **Pressure Sensor V out** | Voltage Divider (R\_upper input) | Must go through the divider! |
| **Voltage Divider V out** | **ESP32 GPIO 36 (VP)** | This is the ADC input pin. |
| **OLED SDA** | **ESP32 GPIO 21** | I2C Data |
| **OLED SCL** | **ESP32 GPIO 22** | I2C Clock |

## 💻 Software Setup (ESP32 Firmware)

The firmware is located in `esp32_oil_gauge.cpp`.

### Required Arduino Libraries

Install these libraries via the Arduino Library Manager:

1.  **ESP32-BLE-Arduino** (or similar library for your specific ESP32 core)
2.  **Adafruit GFX Library**
3.  **Adafruit SSD1306**

### BLE Service UUIDs

The following UUIDs **must match** in the C++ firmware, the HTML client, and the Bluefly app configuration:

| Feature | UUID | Constant in Code |
| :--- | :--- | :--- |
| **Service** | `4fafc201-1fb5-459e-8fcc-c5c9c331914b` | `SERVICE_UUID` |
| **Characteristic** | `beb5483e-36e1-4688-b7f5-ea07361b26a8` | `RX_CHAR_UUID` |

## 🌐 Web Client Usage

The web client is a single HTML file (`index.html`) using JavaScript Web Bluetooth API to connect.

### How to Use the Web Client

1.  **Open the Client:** Open the `index.html` file in a modern browser (Chrome, Edge, Opera) on a device with Bluetooth enabled.
2.  **Click "Connect":** Click the **Connect to ESP32 (BLE)**.
3.  **Select Device:** In the dialog that appears, select the device named **`ESP32_OilGauge`** and pair.
4.  **View Data:** Once connected, the large display will update instantly with live PSI readings transmitted from the ESP32.
5.  *(Optional)* The **Mock Data** button allows you to test the gauge UI and responsiveness without the physical hardware connected.
