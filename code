#include <Wire.h>
#include <Adafruit_Sensor.h>
#include <Adafruit_BMP280.h>
#include <DHT.h>

// --- Pin Definitions ---
#define DHTPIN 9          // DHT11 connected to Digital Pin 9
#define DHTTYPE DHT11     // Defining the type of DHT sensor

#define LDR_PIN A1        // LDR connected to Analog Pin A1
#define MQ3_PIN A2        // MQ-3 connected to Analog Pin A2
#define BENZENE_PIN A3    // Benzene sensor connected to Analog Pin A3
#define MQ7_PIN A6        // MQ-7 connected to Analog Pin A6
#define MQ4_PIN A7        // MQ-4 connected to Analog Pin A7

// --- Object Declarations ---
DHT dht(DHTPIN, DHTTYPE);
Adafruit_BMP280 bmp; // I2C for HW-611

void setup() {
  Serial.begin(9600);
  while (!Serial); // Wait for Serial Monitor to open
  
  // Give sensors a moment to stabilize after power-on
  delay(1000);
  
  // Initialize DHT11
  dht.begin();
  
  // Initialize HW-611 (BMP280) at I2C address 0x76
  if (bmp.begin(0x76)) {
    // Configure the sensor with your optimal settings if found
    bmp.setSampling(Adafruit_BMP280::MODE_NORMAL,     /* Operating Mode. */
                    Adafruit_BMP280::SAMPLING_X2,     /* Temp. oversampling */
                    Adafruit_BMP280::SAMPLING_X16,    /* Pressure oversampling */
                    Adafruit_BMP280::FILTER_X16,      /* Filtering. */
                    Adafruit_BMP280::STANDBY_MS_500); /* Standby time. */
  }

  // Print the CSV Header (Make sure to uncheck "Show timestamp" in Serial Monitor)
  Serial.println("DHT_Temp(C),DHT_Hum(%),BMP_Temp(C),BMP_Press(hPa),MQ3_Alc,MQ4_CH4,MQ7_CO,Benzene,LDR_Light");
}

void loop() {
  // 1. Read DHT11 Sensor
  float dht_hum = dht.readHumidity();
  float dht_temp = dht.readTemperature();

  // 2. Read HW-611 (BMP280) Sensor
  float bmp_temp = bmp.readTemperature();
  float bmp_press = bmp.readPressure() / 100.0F; // Convert Pa to hPa

  // 3. Read Analog Sensors
  int mq3_val = analogRead(MQ3_PIN);
  int mq4_val = analogRead(MQ4_PIN);
  int mq7_val = analogRead(MQ7_PIN);
  int benzene_val = analogRead(BENZENE_PIN);
  int ldr_val = analogRead(LDR_PIN);

  // 4. Print Data in CSV Format
  // DHT Data
  Serial.print(dht_temp); Serial.print(",");
  Serial.print(dht_hum); Serial.print(",");
  
  // HW-611 (BMP280) Data
  Serial.print(bmp_temp); Serial.print(",");
  Serial.print(bmp_press); Serial.print(",");
  
  // Analog Gas & Light Data
  Serial.print(mq3_val); Serial.print(",");
  Serial.print(mq4_val); Serial.print(",");
  Serial.print(mq7_val); Serial.print(",");
  Serial.print(benzene_val); Serial.print(",");
  Serial.println(ldr_val); // println adds the newline at the very end

  // Wait 2 seconds before the next reading (DHT11 requires at least 2 seconds)
  delay(2000);
}
