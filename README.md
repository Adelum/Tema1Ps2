#include <SoftwareSerial.h>
const int lm35Pin = A0;
const int ledPin = 13;
float temperatura;
bool ledStare = false;
SoftwareSerial SerialMonitor(10, 11); 
void setup() {
  pinMode(ledPin, OUTPUT);
  SerialMonitor.begin(9600);
}
void loop() {
 temperatura = analogRead(lm35Pin) * 0.488; 
  SerialMonitor.print("Temperatura curenta: ");
  SerialMonitor.print(temperatura);
  SerialMonitor.println(" °C");
if (SerialMonitor.available() > 0) {
    char comanda = SerialMonitor.read();
   if (comanda == 'A') {
      ledStare = true;
      digitalWrite(ledPin, HIGH);
    } else if (comanda == 'S') {
      ledStare = false;
      digitalWrite(ledPin, LOW); 
    }
  }
  delay(500);
}
