Κώδικας κορμός για θέμα με δύο TMP36 και push button
Πατάω button.
Μετράω θερμοκρασία 1 και θερμοκρασία 2.
Υπολογίζω διαφορά.
Αν διαφορά > 15°C → κόκκινο LED για 5 sec.
Αν διαφορά <= 15°C → πράσινο LED για 5 sec.
Μετά όλα σβηστά.

const int tempPin1 = A0;
const int tempPin2 = A1;

const int buttonPin = 2;

const int redLed = 8;
const int greenLed = 9;

const float diffThreshold = 15.0;

void setup() {
  pinMode(buttonPin, INPUT_PULLUP);

  pinMode(redLed, OUTPUT);
  pinMode(greenLed, OUTPUT);

  digitalWrite(redLed, LOW);
  digitalWrite(greenLed, LOW);

  Serial.begin(9600);
}

void loop() {
  int buttonState = digitalRead(buttonPin);

  if (buttonState == LOW) {
    float temp1 = readTemperature(tempPin1);
    float temp2 = readTemperature(tempPin2);

    float difference = abs(temp1 - temp2);

    if (difference > diffThreshold) {
      digitalWrite(redLed, HIGH);
      digitalWrite(greenLed, LOW);
    }
    else {
      digitalWrite(redLed, LOW);
      digitalWrite(greenLed, HIGH);
    }

    delay(5000);

    digitalWrite(redLed, LOW);
    digitalWrite(greenLed, LOW);

    delay(300);
  }
}

float readTemperature(int pin) {
  int value = analogRead(pin);
  float voltage = value * (5.0 / 1023.0);
  float tempC = (voltage - 0.5) * 100.0;
  return tempC;
}
