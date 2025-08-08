# Battery-Saving-Automation-Circuit

This project implements a battery-saving system with Arduino Uno. The system turns ON when triggered by:

pressing a button

or detecting a nearby object with an HC-SR04 ultrasonic sensor

It stays ON for a configurable period, then turns OFF automatically if no further activity is detected — mimicking a software latch with timeout.

📋 Features ✅ Turns ON by button press or proximity detection. ✅ Stays latched ON for 10 seconds (configurable). ✅ Turns OFF automatically after timeout if no trigger. ✅ Built in Tinkercad and tested on real hardware.

🔷 Components Component Quantity Arduino Uno R3 1 Pushbutton 1 HC-SR04 Ultrasonic Sensor 1 LED (simulates load) 1 Resistor 220Ω 1 Resistor 10kΩ 1 Breadboard & jumpers —

🔷 Circuit Connections Button One pin → GND

Other pin → Arduino pin 2

10kΩ resistor between button and GND

Or use Arduino’s internal pull-up resistor (INPUT_PULLUP)

HC-SR04 Sensor VCC → 5V

GND → GND

TRIG → Arduino pin 5

ECHO → Arduino pin 6

LED Long leg → Arduino pin 4

Short leg → GND

220Ω resistor in series

📝 Code Below is the full Arduino code used in the project:

cpp

// battery_saver.ino // Turns ON when button pressed or sensor triggered // Stays ON for timeout period, then turns OFF

const int buttonPin = 2; // button pin const int trigPin = 5; // HC-SR04 trig pin const int echoPin = 6; // HC-SR04 echo pin const int ledPin = 4; // output LED or relay pin

unsigned long timeout = 10000; // 10 seconds timeout unsigned long turnOffTime = 0;

bool isOn = false;

void setup() { pinMode(buttonPin, INPUT_PULLUP); // button with internal pull-up pinMode(trigPin, OUTPUT); pinMode(echoPin, INPUT); pinMode(ledPin, OUTPUT);

digitalWrite(ledPin, LOW); // start OFF }

void loop() { unsigned long now = millis();

bool buttonPressed = (digitalRead(buttonPin) == LOW); bool sensorTriggered = isSensorTriggered();

if (buttonPressed || sensorTriggered) { digitalWrite(ledPin, HIGH); turnOffTime = now + timeout; isOn = true; }

if (isOn && now >= turnOffTime) { digitalWrite(ledPin, LOW); isOn = false; } }

// Function to check if something is closer than 20 cm bool isSensorTriggered() { digitalWrite(trigPin, LOW); delayMicroseconds(2); digitalWrite(trigPin, HIGH); delayMicroseconds(10); digitalWrite(trigPin, LOW);

long duration = pulseIn(echoPin, HIGH, 30000); // 30ms timeout if (duration == 0) return false; // no echo

long distance = duration * 0.034 / 2;

if (distance > 0 && distance <= 20) { return true; } return false; } 🧪 How to Run 1️⃣ Upload the code to your Arduino Uno. 2️⃣ Connect components as described above. 3️⃣ Power the Arduino. 4️⃣ Press the button or bring your hand close to the sensor. 5️⃣ The LED turns on & stays on for the timeout period, then turns off if no trigger happens.
