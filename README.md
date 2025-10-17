# Humidity Temperature Sensor System
#include <DHT.h>
#include <LiquidCrystal_I2C.h>

#define DHTPIN 2
#define DHTTYPE DHT22

DHT dht(DHTPIN, DHTTYPE);
LiquidCrystal_I2C lcd(0x27, 16, 2); // Change address if needed

void setup() {
  Serial.begin(9600);
  lcd.init();
  lcd.backlight();
  dht.begin();
  
  lcd.setCursor(0, 0);
  lcd.print("Temp: ");
  lcd.setCursor(0, 1);
  lcd.print("Humidity: ");
}

void loop() {
  delay(2000); // Wait between measurements
  
  float h = dht.readHumidity();
  float t = dht.readTemperature();
  
  if (isnan(h) || isnan(t)) {
    lcd.setCursor(6, 0);
    lcd.print("Error");
    return;
  }
  
  // Display temperature
  lcd.setCursor(6, 0);
  lcd.print(t);
  lcd.print(" C");
  
  // Display humidity
  lcd.setCursor(9, 1);
  lcd.print(h);
  lcd.print(" %");
  
  // Optional serial output
  Serial.print("Humidity: ");
  Serial.print(h);
  Serial.print(" %\t");
  Serial.print("Temperature: ");
  Serial.print(t);
  Serial.println(" °C");
}
