Before stepping into this
install this library  (LiquidCrystal_I2C lcd(0x27, 16, 2);)
System must have external 12v or 9v external DC output to make the Filter work od different sensors output.

Upload this on Arduino- 

#include <DHT.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

// -------------------- DHT SETUP --------------------
#define DHTPIN 2
#define DHTTYPE DHT11
DHT dht(DHTPIN, DHTTYPE);

// -------------------- MOTOR DRIVER --------------------
#define ENA 5     // PWM pin
#define IN1 6
#define IN2 7

// -------------------- BUZZER --------------------
#define BUZZER 8

// -------------------- LCD --------------------
LiquidCrystal_I2C lcd(0x27, 16, 2);

// -------------------- VARIABLES --------------------
float temp = 0;
float hum = 0;
int fanSpeed = 0;

void setup() {
  pinMode(ENA, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(BUZZER, OUTPUT);

  digitalWrite(IN1, HIGH);   // fixed direction
  digitalWrite(IN2, LOW);

  Serial.begin(9600);

  dht.begin();

  lcd.init();
  lcd.backlight();

  lcd.setCursor(0, 0);
  lcd.print("AeroSense Init");
  delay(2000);
  lcd.clear();
}

void loop() {

  temp = dht.readTemperature();
  hum  = dht.readHumidity();

  // ----------- CHECK SENSOR ERROR -----------
  if (isnan(temp) || isnan(hum)) {
    lcd.setCursor(0,0);
    lcd.print("Sensor Error   ");
    return;
  }

  // ----------- FAN CONTROL LOGIC -----------
  if (temp < 25) {
    fanSpeed = 0;
    digitalWrite(BUZZER, LOW);
  }
  else if (temp >= 25 && temp < 30) {
    fanSpeed = 100;
    digitalWrite(BUZZER, LOW);
  }
  else if (temp >= 30 && temp < 35) {
    fanSpeed = 180;
    digitalWrite(BUZZER, LOW);
  }
  else {
    fanSpeed = 255;
    digitalWrite(BUZZER, HIGH);
  }

  // ----------- APPLY PWM -----------
  analogWrite(ENA, fanSpeed);

  // ----------- LCD DISPLAY -----------
  lcd.setCursor(0, 0);
  lcd.print("T:");
  lcd.print(temp,1);
  lcd.print("C ");

  lcd.print("H:");
  lcd.print(hum,0);
  lcd.print("%");

  lcd.setCursor(0, 1);
  lcd.print("Fan:");
  lcd.print(map(fanSpeed, 0, 255, 0, 100));
  lcd.print("%   ");

  // ----------- SERIAL MONITOR (DEBUG) -----------
  Serial.print("Temp: ");
  Serial.print(temp);
  Serial.print(" C | Hum: ");
  Serial.print(hum);
  Serial.print(" % | Fan: ");
  Serial.println(map(fanSpeed, 0, 255, 0, 100));

  delay(1000);
}
