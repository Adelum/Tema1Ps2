// Include librăria pentru comunicare serială
#include <SoftwareSerial.h>

// Definirea pinilor pentru senzorul de temperatură LM35 și LED
const int lm35Pin = A0;
const int ledPin = 13;

// Variabile pentru stocarea temperaturii și stării LED-ului
float temperatura;
bool ledStare = false;

// Inițializare obiect Serial pentru comunicarea cu SerialMonitor
SoftwareSerial SerialMonitor(10, 11); // RX, TX

void setup() {
  // Inițializare pin pentru LED ca ieșire
  pinMode(ledPin, OUTPUT);
  
  // Inițializare comunicare serială cu o rată de baud de 9600
  SerialMonitor.begin(9600);
}

void loop() {
  // Citirea temperaturii de la senzorul LM35
  temperatura = analogRead(lm35Pin) * 0.488; // Conversie la grade Celsius
  
  // Afisarea temperaturii pe SerialMonitor
  SerialMonitor.print("Temperatura curenta: ");
  SerialMonitor.print(temperatura);
  SerialMonitor.println(" °C");

  // Verificarea dacă s-a primit comandă de la SerialMonitor
  if (SerialMonitor.available() > 0) {
    char comanda = SerialMonitor.read();
    // Verificare comandă și controlul LED-ului corespunzător
    if (comanda == 'A') {
      ledStare = true;
      digitalWrite(ledPin, HIGH); // Aprindere LED
    } else if (comanda == 'S') {
      ledStare = false;
      digitalWrite(ledPin, LOW); // Stingere LED
    }
  }
  
  // Așteptare 500 ms între citiri și verificări
  delay(500);
}
