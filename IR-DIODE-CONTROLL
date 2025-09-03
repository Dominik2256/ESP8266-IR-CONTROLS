#include <IRremoteESP8266.h>
#include <IRrecv.h>
#include <IRutils.h>

const uint16_t RECV_PIN = 5;         // GPIO5 – IR odbiornik
const uint16_t RELAY_PIN = 12;       // GPIO12 – dioda (PWM)
const uint32_t EQ_CODE     = 0xFF906F;
const uint32_t FADE_CODE   = 0xFFC23D; // przycisk od 0 do 100%
const uint32_t BTN_CODES[9] = {
  0xFF30CF, // 1
  0xFF18E7, // 2
  0xFF7A85, // 3
  0xFF10EF, // 4
  0xFF38C7, // 5
  0xFF5AA5, // 6
  0xFF42BD, // 7
  0xFF4AB5, // 8
  0xFF52AD  // 9
};
const uint32_t PLUS_CODE   = 0xFFA857;
const uint32_t MINUS_CODE  = 0xFFE01F;

IRrecv irrecv(RECV_PIN);
decode_results results;

bool relayState = true;
int blinkDelay = 500;  // domyślny delay
bool pwmMode = false;

void setup() {
  Serial.begin(115200);
  irrecv.enableIRIn();
  pinMode(RELAY_PIN, OUTPUT);
  digitalWrite(RELAY_PIN, LOW);  // dioda zapalona
  Serial.println("Czekam na sygnał IR...");
}

void blinkLed(int times) {
  for (int i = 0; i < times; i++) {
    digitalWrite(RELAY_PIN, HIGH);
    delay(blinkDelay);
    digitalWrite(RELAY_PIN, LOW);
    delay(blinkDelay);
  }
}

void fadeLed() {
  pwmMode = true;
  for (int duty = 0; duty <= 1023; duty += 10) {
    analogWrite(RELAY_PIN, duty);  // PWM (0–1023)
    delay(15);
  }
  delay(300);  // chwilowy stop
  for (int duty = 1023; duty >= 0; duty -= 10) {
    analogWrite(RELAY_PIN, duty);
    delay(15);
  }
  pwmMode = false;
  digitalWrite(RELAY_PIN, relayState ? LOW : HIGH); // wróć do poprzedniego stanu
}

void loop() {
  if (irrecv.decode(&results)) {
    if (results.value != 0xFFFFFFFFFFFFFFFF) {
      Serial.println("--------------");
      Serial.print("Kod HEX: ");
      Serial.println(resultToHexidecimal(&results));

      if (results.value == EQ_CODE) {
        relayState = !relayState;
        digitalWrite(RELAY_PIN, relayState ? LOW : HIGH);
        Serial.println(relayState ? "Diody WLACZONA" : "Diody WYLACZONA");
      }

      if (results.value == FADE_CODE) {
        Serial.println("Start rozjasniania i sciemniania LED...");
        fadeLed();
      }

      for (int i = 0; i < 9; i++) {
        if (results.value == BTN_CODES[i] && !pwmMode) {
          Serial.print("Migam ");
          Serial.print(i + 1);
          Serial.println(" razy.");
          blinkLed(i + 1);
        }
      }

      if (results.value == PLUS_CODE) {
        blinkDelay = max(50, blinkDelay - 50);
        Serial.print("Zwiekszono tempo. Opoznienie: ");
        Serial.println(blinkDelay);
      }
      if (results.value == MINUS_CODE) {
        blinkDelay = min(2000, blinkDelay + 50);
        Serial.print("Zmniejszono tempo. Opoznienie: ");
        Serial.println(blinkDelay);
      }
    }
    irrecv.resume();
  }
}
