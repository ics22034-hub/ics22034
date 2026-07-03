const int tempPin = A0;
const int lightPin = A1;

const int redPin = 9;
const int greenPin = 10;
const int bluePin = 11;

const int lightThreshold = 300;

void setup() {
  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);

  Serial.begin(9600);
}

void loop() {
  float temp = readTemperature(tempPin);
  int lightValue = analogRead(lightPin);

  if (lightValue < lightThreshold) {
    setColor(LOW, LOW, LOW);       // χωρίς φως, LED off
  }
  else {
    if (temp < 10) {
      setColor(LOW, LOW, HIGH);    // μπλε
    }
    else if (temp <= 30) {
      setColor(LOW, HIGH, LOW);    // πράσινο
    }
    else {
      setColor(HIGH, LOW, LOW);    // κόκκινο
    }
  }

  delay(500);
}

float readTemperature(int pin) {
  int value = analogRead(pin);
  float voltage = value * (5.0 / 1023.0);
  float tempC = (voltage - 0.5) * 100.0;
  return tempC;
}

void setColor(int red, int green, int blue) {
  digitalWrite(redPin, red);
  digitalWrite(greenPin, green);
  digitalWrite(bluePin, blue);
}
