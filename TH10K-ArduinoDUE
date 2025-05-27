#define THERMISTOR_PIN A0
#define VCC 3.3
#define R1 10000.0  // 10kΩ series resistor

// Steinhart-Hart coefficients for TH10K
#define A 1.129241e-03
#define B 2.341077e-04
#define C 8.775468e-08

void setup() {
  Serial.begin(9600);
  analogReadResolution(12);  // 12-bit ADC for Arduino Due
}

void loop() {
  int adcValue = analogRead(THERMISTOR_PIN);
  float Vout = (adcValue / 4095.0) * VCC;

  // Avoid divide-by-zero
  if (Vout <= 0) Vout = 0.0001;
  if (Vout >= VCC) Vout = VCC - 0.0001;

  // Calculate thermistor resistance
  float R2 = R1 * (Vout / (VCC - Vout));

  // Apply Steinhart-Hart equation
  float logR = log(R2);
  float invT = A + B * logR + C * pow(logR, 3);
  float temperatureK = 1.0 / invT;
  float temperatureC = temperatureK - 273.15;

  Serial.print("ADC: "); Serial.print(adcValue);
  Serial.print(" | Vout: "); Serial.print(Vout, 3);
  Serial.print(" V | R: "); Serial.print(R2, 1);
  Serial.print(" ohm | T: "); Serial.print(temperatureC, 2);
  Serial.println(" °C");

  delay(1000);
}
