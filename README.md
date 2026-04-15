# Air-Quality Monitoring


An IoT-based Air Quality Monitoring built using **ESP8266 NodeMCU**, **MQ-135 sensor**.  
It monitors Air Quality  providing alerts when thresholds are exceeded.  
The system also uploads real-time data to **Blynk** for remote IoT monitoring.

---

## 🧠 Description
This project continuously measures Air Quality  
At the same time, data is uploaded to the **Blynk Cloud**, enabling remote monitoring via IoT.

---

## ⚙️ Components Used
- **ESP8266 NodeMCU**
- **MQ-135 Sensor**

---

## 🔌 Wiring Connections


| MQ-135 Sensor Pin | ESP8266 NodeMCU Pin | Description |
| :--- | :--- | :--- |
| **VCC** | **3V** | Power Supply (3.3V) |
| **GND** | **GND** | Ground Connection |
| **A0** | **A0** | Analog Signal Output |
| **D0** | **NC** | Digital Output (Not Connected) |


---
## 💻 Arduino Code

```cpp
#define BLYNK_PRINT Serial

#define BLYNK_TEMPLATE_ID "TMPL3_fXXzmuI"
#define BLYNK_TEMPLATE_NAME "Air Quality Monitoring"
#define BLYNK_AUTH_TOKEN "NWE0vo8_w68PIlvDjLT7qisLdOwS_P5P"

#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>


char ssid[] = "Brahma";
char pass[] = "dgof1655";

BlynkTimer timer;

float RL = 10.0;
float R0 = 176.0;

void sendSensorData()
{
  int adcValue = analogRead(A0);

  if (adcValue == 0) return;

  float RS = RL * ((1023.0 / adcValue) - 1.0);
  float ratio = RS / R0;
  float CO2 = 116.6020682 * pow(ratio, -2.769);

  Serial.print("CO2 (PPM): ");
  Serial.println(CO2);

  Blynk.virtualWrite(V1, CO2);
}

void setup()
{
  Serial.begin(9600);
  delay(1000);

  Serial.println("Starting...");

  WiFi.begin(ssid, pass);

  Serial.print("Connecting to WiFi");

  while (WiFi.status() != WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
  }

  Serial.println("\nWiFi Connected!");

  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass, "blynk.cloud", 80);

  Serial.println("Connected to Blynk!");

  timer.setInterval(2000L, sendSensorData);
}

void loop()
{
  Blynk.run();
  timer.run();
}
