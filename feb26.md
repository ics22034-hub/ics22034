Κατασκευάστε το πρωτότυπο και την τελική πλακέτα του ακόλουθου προϊόντος. Αποθηκεύστε σε ένα σημείο text τον κώδικα C του Tinkercad και πάρτε ένα screenshot του κυκλώματος από το Tinkercad. Έπειτα συμπιέστε σε ένα αρχείο zip τον φάκελο του KiCad project, συμπεριλαμβάνοντας και τα δύο προαναφερθέντα αρχεία και “ανεβάστε” τα στο OpenEclass.
Μία εταιρία μάς ζήτησε την κατασκευή μιας πλακέτας αυτόματης μέτρησης θερμοκρασίας ενός χώρου. Συγκεκριμένα, η πλακέτα θα ανιχνεύει μεταβολές φωτός. Όταν “αισθανθεί” ότι υπάρχει φως στον χώρο, θα αρχίζει μετρήσεις θερμοκρασίας και θα υπάρχουν 3 κλίμακες: Κρύο: κάτω από 10°C, Κανονική: από 10°C έως και 30°C, Ζέστη: πάνω από 30°C. Ανάλογα με τη μετρηθείσα θερμοκρασία, θα δίνεται χρώμα σε ένα RGB LED κοινής καθόδου: Κρύο: Μπλε, Κανονική: Πράσινο, Ζέστη: Κόκκινο. Όταν δεν υπάρχει φως στον χώρο, το RGB LED θα είναι απενεργοποιημένο.
Μπορείτε να επιλέξετε αυθαίρετα το αρχικό “κατώφλι” υπέρβασης φωτός στο Tinkercad, π.χ. κάπου στη μέση του χειριστηρίου.
Η εταιρία μάς έδωσε τις ακόλουθες προδιαγραφές:
• Πλακέτα ορθογώνιου σχήματος με 4 οπές στερέωσης, καμία διάσταση της οποίας να μην υπερβαίνει τα 8cm.

• Τροφοδοσία από μία μπαταρία 9V μέσω βύσματος Barrel Jack.

• Χρήση Through Hole RGB LED διαμέτρου 5mm. Tip: χρησιμοποιήστε το σύμβολο LED_RKBG και το footprint LED_THT:LED_D5.0mm-4_RGB.

• Ο χρησιμοποιούμενος μικροελεγκτής θα προγραμματίζεται επάνω στην πλακέτα μέσω ενός 6-pin καλωδίου ICSP.

Τι υλικά να βάλεις στο Tinkercad

Για να το υλοποιήσεις απλά:

Arduino Uno ή μικροελεγκτή τύπου ATmega328P
Αισθητήρα θερμοκρασίας, π.χ. TMP36
Φωτοαντίσταση LDR
Αντίσταση 10kΩ για το LDR
RGB LED common cathode
3 αντιστάσεις 220Ω για το RGB LED
Μπαταρία 9V μέσω Barrel Jack
ICSP header 2x3 pins
4 mounting holes στην πλακέτα
PCB με καμία πλευρά πάνω από 8 cm

int ldrPin = A0;
int tempPin = A1;

int redPin = 9;
int greenPin = 10;
int bluePin = 11;

int lightThreshold = 500; // μπορείς να το αλλάξεις στο Tinkercad

void setup() {
  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);

  Serial.begin(9600);
}

void loop() {
  int lightValue = analogRead(ldrPin);
  int tempValue = analogRead(tempPin);

  float voltage = tempValue * (5.0 / 1023.0);

  // Για TMP36:
  float temperatureC = (voltage - 0.5) * 100.0;

  Serial.print("Light: ");
  Serial.print(lightValue);
  Serial.print("  Temp: ");
  Serial.println(temperatureC);

  if (lightValue < lightThreshold) {
    // Δεν υπάρχει φως → LED σβηστό
    setColor(0, 0, 0);
  } 
  else {
    // Υπάρχει φως → ελέγχουμε θερμοκρασία
    if (temperatureC < 10) {
      setColor(0, 0, 255); // Μπλε
    } 
    else if (temperatureC < 30) {
      setColor(0, 255, 0); // Πράσινο
    } 
    else {
      setColor(255, 0, 0); // Κόκκινο
    }
  }

  delay(500);
}

void setColor(int red, int green, int blue) {
  analogWrite(redPin, red);
  analogWrite(greenPin, green);
  analogWrite(bluePin, blue);
}
