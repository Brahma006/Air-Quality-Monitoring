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
int R0 = 176; 
int R2 = 1000; 
float RS; 
float PPM_acetone; 



#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>

// Your WiFi credentials.
// Set password to "" for open networks.
char ssid[] = "Brahma";
char pass[] = "dgof1655";

void setup()
{
  // Debug console
  Serial.begin(9600);

  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
}

void loop()
{
  Blynk.run();
    int sensorValue = analogRead(A0); // read the input on analog pin 0: 
	   Serial.println("The amount of CO2 (in PPM): "); 
	   Serial.println(sensorValue); 
	   delay(2000); 

     Blynk.virtualWrite(V0,sensorValue );

}
