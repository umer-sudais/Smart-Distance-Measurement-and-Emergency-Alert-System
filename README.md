# Smart-Distance-Measurement-and-Emergency-Alert-System
My semester project initially focused on developing a Distance Measurement Device, a tool designed to measure distances with high precision. Recognizing the potential for further enhancement, I integrated an additional safety feature into the device. This upgraded version now includes an Emergency Alert System that activates when an object is detected within 30 centimeters of the sensor. Upon detection, the device triggers a blinking light to signal an immediate alert, providing a clear visual warning in potentially hazardous situations. 
This evolution of the project not only underscores the importance of accuracy in distance measurement but also highlights the critical role of proactive safety mechanisms in modern technology.

# Components
* Ultrasonic Sensor (HC-SR04)
* Aurdino (ESP8266)
* Breadboard
* Jumper Wires
* Power Supply
* Buzzer or LED light (for emergency alarm)

# Circuit Diagram

![diagram](https://github.com/user-attachments/assets/d4488f65-8fc4-441a-a1d8-94b8b526945f)
# Code
#define BLYNK_PRINT Serial

#define BLYNK_TEMPLATE_ID "TMPL6sGC0ootP"
#define BLYNK_TEMPLATE_NAME "Sonar Measurement"
#define BLYNK_AUTH_TOKEN "L5wjwmk8L0h91M_bP6hLxYjKbA9w7s0x"


#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#define TRIGGERPIN D1
#define ECHOPIN    D2

char auth[] = "L5wjwmk8L0h91M_bP6hLxYjKbA9w7s0x";


char ssid[] = "HUAWEI Mate 10 Pro";
char pass[] = "123456789"; 

WidgetLCD lcd(V1);

void setup()
{

  Serial.begin(9600);
  pinMode(TRIGGERPIN, OUTPUT);
  pinMode(ECHOPIN, INPUT);
  pinMode(LED_BUILTIN, OUTPUT);
  pinMode(D3, OUTPUT);
  Blynk.begin(auth, ssid, pass);


  lcd.clear(); 
  lcd.print(0, 0, "Distance in cm"); 
 
}

void loop()
{
  lcd.clear();
  lcd.print(0, 0, "Distance in cm"); 
  long duration, distance;
  digitalWrite(TRIGGERPIN, LOW);  
  delayMicroseconds(3); 
  
  digitalWrite(TRIGGERPIN, HIGH);
  delayMicroseconds(12); 
  
  digitalWrite(TRIGGERPIN, LOW);
  duration = pulseIn(ECHOPIN, HIGH);
  distance = (duration/2) / 29.1;
  if(distance<=30)
  {
     digitalWrite(LED_BUILTIN, LOW);
     digitalWrite(D3, LOW);
  }
  else
  {
     digitalWrite(LED_BUILTIN, HIGH);
     digitalWrite(D3, HIGH);
  }
  Serial.print(distance);
  Serial.println("Cm");
  lcd.print(7, 1, distance);
  Blynk.run();

  delay(1000);

}
