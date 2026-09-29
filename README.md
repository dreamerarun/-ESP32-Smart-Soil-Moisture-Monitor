# 🌱 ESP32 Smart Soil Moisture Monitor

An interactive **ESP32-based soil moisture monitoring system** that combines real-time sensing, a web-based dashboard, and an OLED display with expressive animations.

The system continuously measures soil moisture and presents the reading in two ways:

* 🌐 **Web dashboard** — displays the current moisture percentage through the ESP32's Wi-Fi web server.
* 👀 **OLED display** — reacts to the soil condition using different eye/face animations and messages.

The project demonstrates how **IoT, embedded systems, web technologies, and interactive displays** can be combined into a simple smart-plant monitoring system.

---

## ✨ Features

* 📡 ESP32 Wi-Fi connectivity
* 🌱 Analog soil moisture sensing
* 📊 Real-time moisture percentage calculation
* 🌐 Built-in ESP32 web server
* 💻 Live browser-based monitoring
* 🖥️ 128×64 OLED graphical display
* 👀 Interactive eye animations
* 💧 Different responses based on soil moisture
* 🔄 Automatic browser updates without manually refreshing the page
* 🔌 Serial Monitor output for debugging and monitoring

---

## 🧠 How It Works

The soil moisture sensor provides an analog reading to the ESP32.

The ESP32 converts the sensor value into an approximate moisture percentage:

```text
Moisture % = 100 - (Analog Reading / 4095 × 100)
```

The calculated value is then:

1. Printed to the Serial Monitor.
2. Made available through the ESP32 web server.
3. Displayed on the OLED through text and bitmap animations.
4. Used to determine the OLED's response to the soil condition.

The web interface periodically sends a request to:

```text
/readMoisture
```

and updates the displayed moisture value automatically.

---

## 🏗️ System Architecture

```text
             ┌─────────────────────┐
             │   Soil Moisture     │
             │      Sensor         │
             └──────────┬──────────┘
                        │ Analog
                        ▼
             ┌─────────────────────┐
             │        ESP32        │
             │                     │
             │  Sensor Processing  │
             │  Wi-Fi              │
             │  Web Server         │
             └───────┬─────┬───────┘
                     │     │
            Wi-Fi    │     │ I²C
                     │     ▼
                     │  ┌──────────┐
                     │  │   OLED   │
                     │  │ Display  │
                     │  └──────────┘
                     │
                     ▼
              ┌───────────────┐
              │ Web Browser   │
              │               │
              │ Moisture: XX% │
              └───────────────┘
```

---

## 🧰 Hardware Requirements

| Component               | Purpose                             |
| ----------------------- | ----------------------------------- |
| ESP32 Development Board | Main microcontroller                |
| Soil Moisture Sensor    | Measures soil moisture              |
| 0.96" 128×64 OLED       | Displays animations and information |
| Jumper Wires            | Connections                         |
| Breadboard              | Prototyping                         |
| USB Cable               | Programming and power               |
| Computer                | Arduino IDE and serial monitoring   |

> The exact sensor and OLED module can be substituted with compatible components, provided the corresponding wiring and libraries are configured correctly.

---

## 💻 Software Requirements

### Required

* Arduino IDE
* ESP32 board package for Arduino
* Wi-Fi support for ESP32
* WebServer library
* OLED display library compatible with the display used
* `html.h` included in the project

The main program uses:

```cpp
#include <WiFi.h>
#include <WebServer.h>
#include "html.h"
```

---

## 📁 Project Structure

```text
esp32-smart-soil-monitor/
│
├── README.md
│
├── src/
│   └── esp32_soil_monitor.ino
│
├── include/
│   └── html.h
│
├── images/
│   └── demo.jpg
│
├── LICENSE
│
└── .gitignore
```

---

## 🔌 Connections

The soil moisture sensor is connected to an ESP32 analog input.

The code currently defines:

```cpp
const int sensor_pin = A0;
```

The OLED uses the display interface configured in the Arduino project.

Before uploading the code, verify the actual GPIO connections on your ESP32 board and adjust the pin definitions if required.

---

## ⚙️ Configuration

Before uploading the program, configure your Wi-Fi credentials.

In the `.ino` file:

```cpp
const char* ssid = "YOUR_WIFI_NAME";
const char* password = "YOUR_WIFI_PASSWORD";
```

### ⚠️ Important

**Do not upload real Wi-Fi credentials to GitHub.**

For a public repository, replace them with placeholders or move the credentials into a separate configuration file that is excluded using `.gitignore`.

---

## 🚀 Installation

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/esp32-smart-soil-monitor.git
cd esp32-smart-soil-monitor
```

### 2. Open the project

Open the Arduino `.ino` file using Arduino IDE.

Make sure:

* ESP32 board support is installed.
* The required OLED library is installed.
* `html.h` is available in the project.
* The correct ESP32 board is selected.

### 3. Configure Wi-Fi

Enter your Wi-Fi SSID and password in the configuration section.

### 4. Connect the hardware

Connect:

```text
Soil Moisture Sensor → ESP32 Analog Input
OLED Display         → ESP32 Display Interface
ESP32                → USB
```

### 5. Upload

Select the appropriate ESP32 board and serial port, then upload the program.

### 6. Open Serial Monitor

Set the baud rate to:

```text
115200
```

After connecting to Wi-Fi, the ESP32 prints its local IP address.

Example:

```text
Connecting to YOUR_WIFI_NAME
Connected to YOUR_WIFI_NAME
Your Local IP address is: 192.168.x.x
```

### 7. Open the Web Dashboard

Enter the displayed IP address into a browser connected to the same network:

```text
http://192.168.x.x
```

The browser will display the live soil moisture value.

---

## 🌐 Web Interface

The ESP32 hosts a lightweight HTML page directly from the microcontroller.

The page displays:

```text
Soil Moisture With ESP32

Moisture Level : XX%
```

The JavaScript periodically requests:

```text
/readMoisture
```

and updates the moisture value on the page without requiring a manual refresh.

The current implementation uses a periodic XMLHttpRequest to retrieve the value from the ESP32 web server.

---

## 👀 Interactive OLED Behaviour

The OLED is used as more than a numerical display.

The project contains multiple bitmap-based eye and expression animations, including states such as:

* Front-facing eyes
* Sleeping eyes
* Tired eyes
* Crossed eyes
* Glare
* Angry eyes
* Crying eyes
* Confused expressions
* Directional eye movements
* Upper/lower eyelids
* Greeting and goodbye animations

The display therefore provides a more interactive representation of the soil condition.

---

## 💧 Moisture-Based Behaviour

The program uses a threshold to determine the OLED response.

```cpp
if (value > 20) {
    // Moisture response
}
else {
    // Dry-soil response
}
```

When the measured value crosses the configured threshold, the OLED runs a different animation sequence and displays corresponding messages.

For example, the dry-soil sequence eventually displays:

```text
WATER ME PLEASE!
```

The system also displays the measured moisture value on the OLED.

---

## 📊 Serial Monitoring

The ESP32 continuously prints the calculated moisture level:

```text
Moisture = XX%
```

This is useful for:

* Debugging
* Sensor calibration
* Observing moisture changes
* Comparing sensor readings with the web dashboard

---

## 🔐 Security Note

Do not commit credentials such as:

```cpp
const char* ssid = "MyWiFi";
const char* password = "MyPassword";
```

to a public repository.

Instead, use a configuration file or environment-specific setup and add sensitive files to `.gitignore`.

If credentials have already been pushed to a public Git repository, change the Wi-Fi password.

---

## ⚠️ Current Limitations

This project is a prototype and has several areas that can be improved.

### 1. Fixed moisture threshold

The current behaviour uses a simple threshold:

```cpp
value > 20
```

Real soil moisture requirements vary depending on:

* Soil type
* Plant type
* Sensor characteristics
* Environmental conditions

### 2. Sensor calibration

The percentage calculation assumes a particular ADC range and sensor behaviour.

The formula should ideally be calibrated using measured:

* Dry-soil value
* Wet-soil value

### 3. Long blocking delays

The OLED animation sequences contain many `delay()` calls.

This can temporarily prevent the ESP32 from responding as efficiently to other tasks.

A future version could use a non-blocking state machine based on `millis()`.

### 4. Wi-Fi credentials

Credentials are currently defined directly in the source code and should be externalized before publishing the project.

### 5. Web interface

The current dashboard is intentionally simple. It could be expanded with:

* Historical moisture graphs
* Status indicators
* Responsive UI
* Sensor calibration controls
* Plant profiles
* Data logging

---

## 🔮 Future Improvements

Possible extensions include:

* 🌱 Automatic plant watering
* 💧 Relay-controlled water pump
* 📈 Historical moisture graphs
* ☁️ Cloud data logging
* 📱 Mobile-friendly dashboard
* 🔔 Low-moisture notifications
* 🌡️ Temperature and humidity monitoring
* 🧠 Adaptive moisture thresholds
* 📊 Sensor calibration interface
* ⚡ Non-blocking animation system
* 🔐 Secure Wi-Fi credential management
* 🌐 MQTT integration
* 📡 Remote monitoring

---

## 🎯 Applications

This concept can be extended to:

* Smart plant monitoring
* Home gardening
* IoT agriculture
* Indoor plants
* Educational IoT projects
* Embedded systems demonstrations
* Smart irrigation prototypes

---

## 📜 License

This project is provided for educational and experimental purposes.

Add a `LICENSE` file to the repository according to the license you choose, such as MIT or Apache-2.0.

---

## 👨‍💻 Author

**Arun M.**

Robotics & Automation Engineer
Interested in Robotics, Physical AI, Embedded Systems and Intelligent Automation.

---

## ⭐ If You Found This Useful

If this project helped you learn something about ESP32, IoT, embedded systems, or robotics, consider giving the repository a ⭐.
