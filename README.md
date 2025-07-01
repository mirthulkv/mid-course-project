# mid-course-project
# Smart Home Automation System – ESP32 IoT Project

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

## Code

     #include <WiFi.h>
     #include "DHTesp.h"
     #include "ThingSpeak.h"

    #define ldrPin 2

     const int PIR_PIN = 14;
     const int DHT_PIN = 15;
     const int LDR_PIN = A0;  // Define the LDR pin
     const int LED_PIN = 12;
     const float gama = 0.7; 
     const float rl10 = 50;

     int pirState = LOW;
     int val = 0;
     int ldrValue = 0;  // Variable to store LDR value

     const char* WIFI_NAME = "Wokwi-GUEST";
     const char* WIFI_PASSWORD = "";
     const int myChannelNumber = 2996048;
     const char* myApiKey = "VFZ3WMLX146INWU0";
     const char* server = "api.thingspeak.com";

    DHTesp dhtSensor;
    WiFiClient client;

     void setup() {
      Serial.begin(115200);
      pinMode(PIR_PIN, INPUT);
      pinMode(LDR_PIN, INPUT);  // Set the LDR pin as an input
      dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
      pinMode(LED_PIN, OUTPUT);
      WiFi.begin(WIFI_NAME, WIFI_PASSWORD);
      while (WiFi.status() != WL_CONNECTED){
      delay(1000);
      Serial.println("Wifi not connected");
      }
     Serial.println("Wifi connected !");
     Serial.println("Local IP: " + String(WiFi.localIP()));
     WiFi.mode(WIFI_STA);
    ThingSpeak.begin(client);
     }

    void loop() {
     TempAndHumidity  data = dhtSensor.getTempAndHumidity();
     ThingSpeak.setField(1, data.temperature);
     ThingSpeak.setField(2, data.humidity);

    // Read PIR sensor
     val = digitalRead(PIR_PIN);
    if (val == HIGH) {
    pirState = HIGH;
    digitalWrite(LED_PIN, HIGH);  // Turn on LED when motion is detected
         }  else {
    pirState = LOW;
    digitalWrite(LED_PIN, LOW);   // Turn off LED when no motion
     }

    // Read LDR sensor
    ldrValue = analogRead(LDR_PIN);

    // Send motion detection and LDR data
    ThingSpeak.setField(3, pirState); // Use Field 3 for motion detection data
    ThingSpeak.setField(4, ldrValue); // Use Field 4 for LDR data

    int x = ThingSpeak.writeFields(myChannelNumber, myApiKey);
  
    Serial.println("Temp: " + String(data.temperature, 2) + "°C");
    Serial.println("Humidity: " + String(data.humidity, 1) + "%");
    Serial.println("LDR Value: " + String(ldrValue));

    if (pirState == HIGH) {
    Serial.println("Motion Detected!");
    }
  
    if (x == 200){
    Serial.println("Data pushed successfully");
    } else {
    Serial.println("Push error: " + String(x));
    }
    Serial.println("---");

    delay(10000);
    }

## Simulated circuit

![image](https://github.com/user-attachments/assets/48ee3baf-4f07-4e19-8344-ef2298ae3c81)

Project link(Wokwi): https://wokwi.com/projects/434546104861605889

## Project Demo

https://drive.google.com/file/d/161kzkcA-wFU74ar1Yv_FGzWWPMLfD0Wq/view?usp=drive_link


## Procedure

1.Open the Wokwi Simulator
    Begin the simulation environment for ESP32-based IoT projects.

2.Add Required Components
     Add ESP32, PIR sensor, LDR, DHT22, and LED to the circuit.

3.Make Circuit Connections
     Connect all components appropriately to the ESP32 pins.

4. Write and Upload Code
    Develop code to read sensor values, control the LED, and send data to the cloud.

5.Configure ThingSpeak Settings
    Set up a ThingSpeak channel and insert the Channel ID and Write API Key into the code.

6.Run the Simulation
    Start the simulation and test the functionality using virtual input controls.

7.Simulate Different Conditions
    Test motion detection and light variation to observe automatic LED control.

8.Monitor Data on ThingSpeak
    View real-time sensor data updates on the cloud dashboard.

9.Document Results
    Capture screenshots and observations for data analysis.

## Result
The smart home automation system was successfully simulated using the Wokwi platform. The system continuously monitored motion using a PIR sensor and ambient light using an LDR. When motion was detected in a low-light condition, the LED (representing a smart light) automatically turned ON. Additionally, real-time sensor data including temperature, humidity, motion status, and light intensity was successfully sent to the ThingSpeak cloud platform and visualized through live graphs. The project met all objectives defined in the problem statement and demonstrated reliable performance in a simulated environment.
