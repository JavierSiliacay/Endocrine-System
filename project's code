/*
  ------------------------------------------------------------------------------
  © 2025 Javier Siliacay | USTP Autotronics - All Rights Reserved.

  This source code is the intellectual property of the author and is provided 
  for educational and non-commercial use only. Unauthorized copying, modification, 
  distribution, or use of this code for commercial purposes is strictly prohibited 
  without prior written consent from the author.

  Violation of these terms may result in legal action under applicable intellectual 
  property laws.

  For permissions, contact: github.com/javiersiliacay
  ------------------------------------------------------------------------------
*/

// Pin Definitions
const int greenLed   = 9;  // PWM
const int redLed     = 4;
const int buzzer     = 10; // PWM 
const int yellowLed1 = 3;  // PWM
const int yellowLed2 = 5;  // PWM
const int yellowLed3 = 6;  // PWM
const int buttonPin  = 2;
const int potPin     = A0;

// Arrays of Yellow LEDs
const int yellowLeds[] = {yellowLed1, yellowLed2, yellowLed3};
const int numLeds = 3;

// Variables
bool abnormalMode = false;
int potValue = 0;
int mappedValue = 0;
bool lastButtonState = HIGH;
unsigned long lastBlinkTime = 0;
bool greenLedState = false;
int yellowLedIndex = 0;  // Active Yellow LED index

void setup() {
  pinMode(buttonPin, INPUT);
  pinMode(greenLed, OUTPUT);
  pinMode(redLed, OUTPUT);
  pinMode(buzzer, OUTPUT);
  for (int i = 0; i < numLeds; i++) {
    pinMode(yellowLeds[i], OUTPUT);
  }
  Serial.begin(9600);
}

void loop() {
  // Read Button
  bool buttonState = digitalRead(buttonPin);
  if (buttonState == HIGH && lastButtonState == LOW) {
    abnormalMode = !abnormalMode; // Toggle mode
    delay(200); // Debounce
  }
  lastButtonState = buttonState;

  if (abnormalMode) {
    // ABNORMALITY MODE
    for (int i = 0; i < numLeds; i++) {
      analogWrite(yellowLeds[i], 255);
    }
    digitalWrite(greenLed, HIGH);
    digitalWrite(redLed, HIGH);
    tone(buzzer, 4000); // High pitch
  } else {
    // NORMAL MODE
    noTone(buzzer); // Ensure buzzer is off
    potValue = analogRead(potPin);
    mappedValue = map(potValue, 0, 1023, 0, 255);
    Serial.print("Potentiometer Value: ");
    Serial.println(mappedValue);

    // Green LED blinking (based on pot > 700)
    int blinkDelay = 500;  // Default blink delay
    if (potValue > 700) {
      blinkDelay = map(potValue, 700, 1023, 500, 100);  // Faster blinking when pot > 700
    }
    if (millis() - lastBlinkTime >= blinkDelay) {
      lastBlinkTime = millis();
      greenLedState = !greenLedState;
      digitalWrite(greenLed, greenLedState);
    }

    // Yellow LEDs cycling with PWM control
    static unsigned long lastCycleTime = 0;
    if (millis() - lastCycleTime >= 700) {  // Fixed 700ms cycle
      lastCycleTime = millis();

      // Turn off all yellow LEDs first
      for (int i = 0; i < numLeds; i++) {
        analogWrite(yellowLeds[i], 0);
      }

      // Move to next yellow LED
      yellowLedIndex = (yellowLedIndex + 1) % numLeds;
    }

    // Set brightness (PWM) of the current active LED based on potentiometer
    int pwmValue = 0;
    if (mappedValue > 170) {
      pwmValue = map(mappedValue, 255, 170, 255, 170);
    } else if (mappedValue > 85) {
      pwmValue = map(mappedValue, 170, 85, 170, 85);
    } else {
      pwmValue = map(mappedValue, 85, 0, 85, 0);
    }
    analogWrite(yellowLeds[yellowLedIndex], pwmValue);

    // Reset system when potentiometer value returns to 0
    if (mappedValue == 0) {
      digitalWrite(redLed, LOW);
      noTone(buzzer);
      for (int i = 0; i < numLeds; i++) {
        analogWrite(yellowLeds[i], 0);  // Turn off all yellow LEDs
      }
    }

    // When potentiometer reaches max value (255), turn on red LED and buzzer
    if (mappedValue == 255) {
      digitalWrite(redLed, HIGH);  // Red LED steady on
      tone(buzzer, 3500);          // Low pitch sound
    } else {
      noTone(buzzer); // Ensure buzzer is off if not at max
    }
  }
}
