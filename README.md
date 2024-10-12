# Interfacing-DHT11-Sensor-with-ESP8266

This project demonstrates how to interface a DHT11 temperature and humidity sensor with an ESP8266. The code reads the temperature and humidity from the sensor and displays the data on the Serial Monitor.
The DHT11 sensor provides readings in both Celsius and Fahrenheit and calculates the heat index.

<h2> Components </h2>
1 x Arduino (e.g., Arduino Uno)
1 x DHT11 Sensor
1 x 10kΩ resistor (optional, for stable data reading)
Breadboard and jumper wires

<h3>Circuit Diagram </h3>

Connect VCC of the DHT11 sensor to the 5V pin of the Arduino.
Connect GND of the DHT11 sensor to the GND pin of the Arduino.
Connect the DATA pin of the DHT11 sensor to digital pin 2 on the Arduino.
(Optional) Place a 10kΩ resistor between the VCC and DATA pin of the DHT11 for better stability.


<h3>Code Explanation</h3>
#include "DHT.h"         // Include the DHT library

#define DHTPIN 2         // Define the pin connected to the DHT11 sensor data pin
#define DHTTYPE DHT11    // Specify the type of DHT sensor used (DHT11)

DHT dht(DHTPIN, DHTTYPE); // Create a DHT object with the specified pin and type

void setup() {
  Serial.begin(115200);  // Start serial communication at 115200 baud rate
  Serial.println(F("DHT11 test!"));

  dht.begin();           // Initialize the DHT sensor
}

void loop() {
  // Wait for 2 seconds between measurements
  delay(2000);

  // Read humidity and temperature data
  float h = dht.readHumidity();
  float t = dht.readTemperature();       // Read temperature in Celsius
  float f = dht.readTemperature(true);   // Read temperature in Fahrenheit

  // Check if the readings are valid
  if (isnan(h) || isnan(t) || isnan(f)) {
    Serial.println(F("Failed to read from DHT sensor!"));
    return;
  }

  // Compute heat index
  float hif = dht.computeHeatIndex(f, h);     // Heat index in Fahrenheit
  float hic = dht.computeHeatIndex(t, h, false); // Heat index in Celsius

  // Print the results to the Serial Monitor
  Serial.print(F("Humidity: "));
  Serial.print(h);
  Serial.print(F("%  Temperature: "));
  Serial.print(t);
  Serial.print(F("C "));
  Serial.print(f);
  Serial.print(F("F  Heat index: "));
  Serial.print(hic);
  Serial.print(F("C "));
  Serial.print(hif);
  Serial.println(F("F"));
}
# How to Use
Copy the code into the Arduino IDE.
Upload the code to your Arduino board.
Open the Serial Monitor (set the baud rate to 115200).
The temperature, humidity, and heat index will be displayed on the Serial Monitor every 2 seconds.
Customization
To change the pin where the DHT11 sensor is connected, modify the #define DHTPIN line.
Adjust the delay() in the loop function to change the interval between readings.
Troubleshooting
If you see the message "Failed to read from DHT sensor!", double-check the wiring and ensure that the DHT11 is properly connected.
Make sure the DHT library is installed in the Arduino IDE. You can install it through the Arduino Library Manager by searching for DHT sensor library.
# License
This project is open-source and available for personal and educational use.
