# Firmware

This folder contains the Arduino Uno firmware source code for the offline smart home automation system.


#include <Wire.h> 
#include <LiquidCrystal_I2C.h> 
LiquidCrystal_I2C lcdsmall(0x23,16,2); // Small lcd (blue color) 
LiquidCrystal_I2C lcd(0x27,20,4); // Large lcd (yellow) 62 

#include <Adafruit_Sensor.h> 
#include <DHT.h> 
#include <DHT_U.h> 
#define DHTPIN 2 // PINMODE = 2 
#define DHTTYPE DHT11 // DHT 22 (AM2302) 
DHT_Unified dht(DHTPIN, DHTTYPE); 
uint32_t delayMS; 
// Variable initiation 
int FLAME = 8; 
int LED = 13; 
int ALARM = 9; 
int smoke = A0; 
Your threshold value 
int sensorThres = 400; 
void setup() { 
pinMode(FLAME, INPUT);//define FLAME input pin 
pinMode(ALARM, OUTPUT);//define ALARM output pin 
lcdsmall.begin(); 63 

lcd.begin(); 
lcd.backlight(); 
lcdsmall.setCursor(0,0); 
lcd.setCursor(0,0); 
dht.begin(); 
Serial.println(F("DHTxx Unified Sensor Example")); 
// Print temperature sensor details. 
sensor_t sensor; 
dht.temperature().getSensor(&sensor); 
// Print humidity sensor details. 
dht.humidity().getSensor(&sensor); 
delayMS = sensor.min_delay / 1000; 
pinMode(LED, OUTPUT); 
pinMode(ALARM, OUTPUT); 
pinMode(smoke, INPUT); 
Serial.begin(9600); 
// lcd.init(); 
lcd.begin(); 
lcd.backlight(); 
// HOME LIGHT CODEDS 
pinMode(3, OUTPUT); // put your setup code here, to run once: 
pinMode(4, OUTPUT); 
pinMode(5, OUTPUT); 64 

pinMode(6, OUTPUT); 
} 
void loop() { 
sensors_event_t event; 
dht.temperature().getEvent(&event); 
if (isnan(event.temperature)) { 
} 
else { 
lcdsmall.setCursor(0,0); 
lcdsmall.print(F("ROOM TEMP ")); 
lcdsmall.print(event.temperature); 
lcdsmall.print(F("C")); 
} 
// Get humidity event and print its value. 
dht.humidity().getEvent(&event); 
if (isnan(event.relative_humidity)) { 
Serial.println(F("Error reading humidity!")); 
} 
else { 
lcdsmall.setCursor(0,1); 
lcdsmall.print(F("ROOM HUMI ")); 
lcdsmall.print(event.relative_humidity); 
lcdsmall.print(F("%")); 65 

} 
int Sensor = analogRead(smoke); 
Serial.print("VALUE "); 
Serial.println(Sensor); 
delay(800); 
// Checks if it has reached the threshold value 
if (Sensor >300) 
{ 
lcd.setCursor(0,1); 
lcd.print("GAS LEAKAGE"); 
digitalWrite(LED, HIGH); 
tone(ALARM, 1000, 200); 
//lcd.clear(); 
} 
else 
{ 
lcd.setCursor(0,1); 
lcd.print("HOME SAVE"); 
digitalWrite(ALARM,LOW); // Set the buzzer OFF 
Serial.println("Peace"); 
digitalWrite(LED,LOW); 
lcd.clear(); 
} 
//delay(100); 
// FLAME CODES BELOW 66 

int fire = digitalRead(FLAME);// read FLAME sensor 
if (fire == HIGH) 
{ 
lcd.setCursor(0,3); 
lcd.print("FIRE OUTBREAK"); 
digitalWrite(ALARM,HIGH);// set the buzzer ON 
tone (10,449,970); 
delay (200); 
Serial.println("FIRE OUTBREAK"); 
digitalWrite(LED,HIGH); 
} else { 
lcd.setCursor(0,3); 
lcd.print("NO FIRE YET"); 
digitalWrite(ALARM,LOW); // Set the buzzer OFF 
Serial.println("Peace"); 
digitalWrite(LED,LOW); 
//lcd.clear(); 
} 
delay (1500); 
if (Serial. Available ()>0) 67 

{ 
char data= Serial. Read (); // reading the data received from the Bluetooth module 
switch(data) 
{ 
case '5': digital Write (3, LOW); break; 
case '6': digital Write (4, LOW); break; 
case '7': digital Write (5, LOW); break; 
case '8': digital Write(6, LOW );break; 
case '1': digital Write (3, HIGH );break; 
case '2': digital Write(4, HIGH );break; 
case '3': digitalWrite(5, HIGH );break; 
case '4': digitalWrite(6, HIGH );break; 
default : break; 
}
Serial.println(data); 
} 
}

