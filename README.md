#include <LiquidCrystal.h>
#include <Wire.h>
#include "DFRobot_PH.h"  // Include the library for pH sensor
#include "DFRobot_EC.h"  // Include the library for TDS sensor
#include <Servo.h>

Servo myservo;

LiquidCrystal lcd(12, 11, 5, 4, 3, 2); // LCD pins: RS, E, D4, D5, D6, D7

// pH sensor setup
#define pH_PIN A0  // Analog pin for pH sensor
DFRobot_PH ph;

// Turbidity sensor setup
#define TURBIDITY_PIN A1 // Analog pin for turbidity sensor

// Temperature sensor setup
#include <DHT.h>
#define DHT_PIN 7   // Digital pin for DHT sensor
#define DHT_TYPE DHT22 // Change to DHT11 if using that sensor
DHT dht(DHT_PIN, DHT_TYPE);

// TDS sensor setup
#define TDS_PIN A2   // Analog pin for TDS sensor
DFRobot_EC ec;
int x= 0;
int y = 0;
int n = 0;
int z = 0;
void setup() {
  myservo.attach(9);
  lcd.begin(16, 2); // Initialize the LCD with 16x2 characters
  
  ph.begin();
  ec.begin(TDS_PIN, TDS_PIN);
  dht.begin();
  
  lcd.print("Sensor Reading");
  delay(2000);
  lcd.clear();
}

void loop() {
  int angle;
  // Read pH sensor value
  float pHValue = ph.readPH();

  // Read turbidity sensor value
  int turbidityValue = analogRead(TURBIDITY_PIN);

  // Read temperature and humidity values
  float temperature = dht.readTemperature();
  float humidity = dht.readHumidity();
  
  // Read TDS sensor value
  float tdsValue = ec.readEC();
  
  // Display readings on the LCD
  lcd.setCursor(0, 0);
  lcd.print("pH: ");
  lcd.print(pHValue, 2);
  
  lcd.setCursor(0, 1);
  lcd.print("Turbidity: ");
  lcd.print(turbidityValue);
  
  delay(2000); // Display for 2 seconds
  
  lcd.clear();
  
  lcd.setCursor(0, 0);
  lcd.print("Temp: ");
  lcd.print(temperature, 1);
  lcd.print("C");
  
  lcd.setCursor(0, 1);
  lcd.print("TDS: ");
  lcd.print(tdsValue, 2);
  lcd.print(" ppm");
  
  delay(2000); // Display for 2 seconds
  
  lcd.clear();
  if ( pHvalue >7.5 || pHvalue<6.5 || turbidityValue> 4 || temperature > 25 || temperature < 5 || tdsValue< 250 || tdsValue > 350 ) {
 for (angle = 180; angle >= 0; angle -= 1) {
    myservo.write(angle);
    delay(15);
  }

}
