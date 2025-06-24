# mid-course-project
# 🔌 Smart Home Automation System – ESP32 IoT Project

##  Aim
To simulate a smart home automation system that:
- Continuously detects human movement using a PIR sensor
- Monitors ambient light using an LDR sensor
- Automatically controls lights and sends real-time alerts to the cloud when motion is detected in low-light conditions

---

##  Problem Statement

Simulate a smart home automation system that:
- (a) Continuously detects human movement using a PIR sensor and monitors ambient light using an LDR sensor.
- (b) Automatically controls lights and sends real-time alerts to the cloud when motion is detected in low-light conditions, indicating the presence of a person in a dark environment.

---

##  Scope of the Solution

This project demonstrates how a home automation system can:
- Automate lighting based on occupancy and ambient light
- Log environmental and security-related data (motion, temperature, humidity, light) to the cloud in real time
- Enable remote monitoring and future scalability (e.g., alerts, dashboards, automation rules)

---

##  Required Components

###  Hardware:
- ESP32 microcontroller
- PIR Motion Sensor
- LDR (Light Dependent Resistor)
- DHT22 (Temperature & Humidity Sensor)
- LED (representing a smart light)
- Resistors
- Breadboard and jumper wires

###  Software:
- Arduino IDE (for code development)
- Wokwi Simulator (for testing without hardware)

###  Cloud Environment:
- [ThingSpeak](https://thingspeak.com/) account
  - For real-time cloud monitoring of sensor values

---

## Flowchart of the Code

    A[Start] --> B[Initialize WiFi, Sensors, LED]
    B --> C[Read PIR Sensor]
    C --> D[Read LDR Sensor]
    D --> E[If motion detected AND low light?]
    E -- Yes --> F[Turn ON LED]
    E -- No --> G[Turn OFF LED]
    F --> H[Read Temp & Humidity]
    G --> H[Read Temp & Humidity]
    H --> I[Send Data to ThingSpeak]
    I --> J[Delay 10 seconds]
    J --> C

         Start
          ↓
      Read PIR Sensor
         ↓
    Is Motion Detected?
     ↓           ↓
    No          Yes
     ↓            ↓
               Idle    Read LDR Sensor
                   ↓
               Is Light Low?
                 ↓      ↓
               No       Yes
             ↓         ↓
    Lights OFF   → Turn ON Lights
                 → Send Alert to Cloud

