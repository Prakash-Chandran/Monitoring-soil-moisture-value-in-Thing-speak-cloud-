# Name : PRAKASH C
# Reg.No : 21222340122
# Monitoring-soil-moisture-value-in-Thing-speak-cloud
# Uploading soil moisture sensor data in Thing Speak cloud

# AIM:
To monitor the soil moisture value in the Thing speak cloud using soil moisture sensor and ESP32 controller.
# Apparatus required:
ESP32 Controller<br>
Soil moisture Sensor<br>
Power supply<br>
Connecting wires<br>
Bread board<br>
# PROCEDURE:
## Arduino IDE
Step1:Open the Arduino IDE<br>
Step2: Go to sketch- include library – manage libraries file and install esp32 and thing speak library file<br>
Step3:Go to file and select new file option<br>
Step4:Type the program and update the thing speak channel ID, API key, wifi password and ID<br>
Step5:Go to file and select save option to save the program<br>
Step6:Go to sketch and select verify or compile options<br>
Step7:If no error Hex file will be generated in the temporary folder<br>
Step8: Connect all the components as per the circuit diagram<br>
Step9: Connect the programming cable with esp32 and PC.<br>
Step10: Check the jumper position and connect 4 & 5 of P4.<br>
Step11. Upload the program in the esp32.<br>
Step12 Press the boot button in ESP32 and then press and release the reset button after release the boot button<br>
Step13 Check the output in the cloud<br>
## Thingspeak
Step1 Create a ThingSpeak Account<br>
Step2 Log in to your ThingSpeak account<br>
Step3 Create a new channel by navigating to "Channels" and clicking on "New Channel."<br>
Step4 Configure your channel settings, such as Field labels and Channel name<br>
Step5 Copy the Channel ID and API key in the thingspeak and update in the program<br>
Step6 Execute your program to send the sensor value to ThingSpeak<br>
Step7 Check your ThingSpeak channel to verify that the sensor value has been updated<br>
# THEORY:
## Soil moisture sensor:
The soil moisture sensor is one kind of sensor used to gauge the volumetric content of water within the soil. As the straight gravimetric dimension of soil moisture needs eliminating, drying, as well as sample weighting. These sensors measure the volumetric water content not directly with the help of some other rules of soil like dielectric constant, electrical resistance, otherwise interaction with neutrons, and replacement of the moisture content.
The relation among the calculated property as well as moisture of soil should be adjusted & may change based on ecological factors like temperature, type of soil, otherwise electric conductivity. The microwave emission which is reflected can be influenced by the moisture of soil as well as mainly used in agriculture and remote sensing within hydrology
## Soil Moisture Sensor Pin Configuration
The FC-28 soil moisture sensor includes 4-pins
•	VCC pin is used for power<br>
•	A0 pin is an analog output<br>
•	D0 pin is a digital output<br>
•	GND pin is a Ground<br>
![image](https://github.com/user-attachments/assets/7fd3c618-7d11-4186-afc7-7ab81ec9d326)

This module also includes a potentiometer that will fix the threshold value, & the value can be evaluated by the comparator-LM393. The LED will turn on/off based on the threshold value.
## Working Principle
This sensor mainly utilizes capacitance to gauge the water content of the soil (dielectric permittivity). The working of this sensor can be done by inserting this sensor into the earth and the status of the water content in the soil can be reported in the form of a percent.
This sensor makes it perfect to execute experiments within science courses like environmental science, agricultural science, biology, soil science, botany, and horticulture.
## Specifications
The specification of this sensor includes the following.
•	The required voltage for working is 5V<br>
•	The required current for working is <20mA<br>
•	Type of interface is analog<br>
•	The required working temperature of this sensor is 10°C~30°C<br>
## Soil Moisture Sensor Applications
The applications of moisture sensor include the following.
•	Agriculture<br>
•	Landscape irrigation<br>
•	Research<br>
•	Simple sensors for gardeners<br>
## What is IoT?
Internet of Things (IoT) describes an emerging trend where a large number of embedded devices (things) are connected to the Internet. These connected devices communicate with people and other things and often provide sensor data to cloud storage and cloud computing resources where the data is processed and analyzed to gain important insights. Cheap cloud computing power and increased device connectivity is enabling this trend.IoT solutions are built for many vertical applications such as environmental monitoring and control, health monitoring, vehicle fleet monitoring, industrial monitoring and control, and home automation
![image](https://github.com/user-attachments/assets/9b5b1576-a121-4045-b578-2cf5cc7fa62b)

Sending Data to Cloud with ESP32 and ThingSpeak
ThingSpeak is an Internet of Things (IoT) analytics platform that allows users to collect, analyze, and visualize data from sensors or devices connected to the Internet. It is a cloud-based platform that provides APIs for storing and retrieving data, as well as tools for data analysis and visualization.The Internet of Things ( or IoT) is a network of interconnected computing devices such as digital machines, automobiles with built-in sensors, or humans with unique identifiers and the ability to communicate data over a network without human intervention.Hello readers, I hope you all are doing great. In this tutorial, we will learn how to send sensor readings from ESP32 to the ThingSpeak cloud. Here we will use the ESP32’s internal sensor like hall-effect sensor and temperature sensor to observe the data and then will share that data cloud.
## What is ThingSpeak?
![image](https://github.com/user-attachments/assets/d7ad629b-944c-47c3-a949-303a3eb32bfc)

It is an open data platform for IoT (Internet of Things). ThingSpeak is a web service operated by MathWorks where we can send sensor readings/data to the cloud. We can also visualize and act on the data (calculate the data) posted by the devices to ThingSpeak. The data can be stored in either private or public channels.ThingSpeak is frequently used for internet of things prototyping and proof of concept systems that require analytics.
## Features Of ThingSpeak
ThingSpeak service enables users to share analyzed data through public channels:<br>
ThingSpeak allows professionals to prepare and analyze data for their businesses:<br>
ThingSpeak updates various ThingSpeak channels using MQTT and REST APIs:<br>
Easily configure devices to send data to ThingSpeak using popular IoT protocols.<br>
Visualize your sensor data in real-time.<br>
Aggregate data on-demand from third-party sources.<br>
Use the power of MATLAB to make sense of your IoT data.<br>
Run your IoT analytics automatically based on schedules or events<br>.
Prototype and build IoT systems without setting up servers or developing web software.<br>

![image](https://github.com/user-attachments/assets/5beaf86c-0d5d-4b99-9c22-bb0351f487ab)

# PROGRAM:

```
#include <WiFi.h>
#include <HTTPClient.h>

// ==========================
// Wi-Fi Configuration
// ==========================
const char* ssid = "طوفان";       // Your WiFi name
const char* password = "12345678"; // Your WiFi password

// ==========================
// ThingSpeak Configuration
// ==========================
String apiKey = "1MFB857N0ZEID146";  // Your ThingSpeak Write API Key
const char* server = "http://api.thingspeak.com/update";

// ==========================
// Sensor & LED Pins
// ==========================
const int sensorPin = 34;  // Analog input pin for soil sensor
const int ledPin = 2;      // Onboard LED

int sensorValue = 0;
int sensorPercentage = 0;

// ==========================
// Function: Connect Wi-Fi
// ==========================
void connectWiFi() {
  Serial.print("Connecting to WiFi");
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("\n✅ Connected to WiFi!");
  Serial.print("IP Address: ");
  Serial.println(WiFi.localIP());
}

// ==========================
// Function: Send Data to ThingSpeak
// ==========================
void sendToThingSpeak(int value) {
  if (WiFi.status() == WL_CONNECTED) {
    HTTPClient http;
    String url = String(server) + "?api_key=" + apiKey + "&field1=" + String(value);

    http.begin(url);
    int httpResponseCode = http.GET();

    if (httpResponseCode > 0) {
      Serial.print("📡 ThingSpeak Response Code: ");
      Serial.println(httpResponseCode);
    } else {
      Serial.print("❌ Error sending data: ");
      Serial.println(httpResponseCode);
    }

    http.end();
  } else {
    Serial.println("⚠️ WiFi not connected!");
  }
}

// ==========================
// Setup Function
// ==========================
void setup() {
  Serial.begin(115200);
  pinMode(ledPin, OUTPUT);
  connectWiFi();
  Serial.println("🌿 Soil Moisture Monitoring Started...");
}

// ==========================
// Main Loop
// ==========================
void loop() {
  sensorValue = analogRead(sensorPin);  // Read analog value (0–4095 for ESP32)
  
  // Convert raw value to percentage (0 = dry, 100 = wet)
  sensorPercentage = map(sensorValue, 4095, 0, 0, 100);  
  sensorPercentage = constrain(sensorPercentage, 0, 100);

  Serial.print("Raw Sensor Value: ");
  Serial.print(sensorValue);
  Serial.print("  |  Soil Moisture: ");
  Serial.print(sensorPercentage);
  Serial.println("%");

  // Simple soil moisture indication using LED
  if (sensorPercentage < 30) {
    Serial.println("Soil is Dry 💧");
    digitalWrite(ledPin, HIGH);
  } else if (sensorPercentage >= 30 && sensorPercentage < 70) {
    Serial.println("Soil is Moist 🌿");
    digitalWrite(ledPin, LOW);
  } else {
    Serial.println("Soil is Wet 🌧️");
    digitalWrite(ledPin, LOW);
  }

  // Send data to ThingSpeak (send percentage)
  sendToThingSpeak(sensorPercentage);

  Serial.println("-----------------------------");
  delay(20000); // ⚠️ ThingSpeak requires at least 15 seconds between updates
}

```

# CIRCUIT DIAGRAM:

![Ex_05](https://github.com/user-attachments/assets/8d00de15-c58e-47aa-8362-8ac9a696a230)

# OUTPUT:

## SERIAL MONITOR :

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/bcbf98ab-69bf-4a7e-b964-ab371e3c47e2" />


## THINKSPEAK CLOUD :

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/dfb1ab26-c20b-47c7-90f8-5c7452085aa7" />


# RESULT:
Thus the soil moisture values are updated in the Thing speak cloud using ESP32 controller.



