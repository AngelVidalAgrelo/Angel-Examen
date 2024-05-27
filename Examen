//Vidal Agrelo Angel 27/05/2024
// Definiciones de pines
const int motorPin1 = D1;
const int motorPin2 = D2;
const int enablePin = D3; // Control de velocidad con PWM
const int buttonPin = D4;
const int potPin = A0;
const int ledPins[] = {D5, D6, D7, D8, D9};

// Variables de estado
bool motorRunning = false;
unsigned long lastButtonPressTime = 0;
unsigned long motorStartTime = 0;

// Constantes
const unsigned long MOTOR_TIMEOUT = 90000; // 90 segundos
const unsigned long BUTTON_DEBOUNCE_DELAY = 200; // 200 ms
const int LED_BLINK_COUNT = 3;
const int LED_BLINK_DELAY = 200;

void setup() {
    Serial.begin(115200);

    // Configuración de pines del motor
    pinMode(motorPin1, OUTPUT);
    pinMode(motorPin2, OUTPUT);
    pinMode(enablePin, OUTPUT);

    // Configuración del pin del pulsador
    pinMode(buttonPin, INPUT_PULLUP);

    // Configuración de pines de los LEDs
    for (int i = 0; i < 5; i++) {
        pinMode(ledPins[i], OUTPUT);
        blinkLed(ledPins[i]);
    }

    // Inicialización del estado del motor
    stopMotor();
}

void loop() {
    unsigned long currentTime = millis();

    // Lectura y gestión del pulsador
    if (digitalRead(buttonPin) == LOW) {
        delay(50); // Debounce
        if (digitalRead(buttonPin) == LOW && currentTime - lastButtonPressTime > BUTTON_DEBOUNCE_DELAY) {
            motorRunning = !motorRunning;
            if (motorRunning) {
                startMotor();
                motorStartTime = currentTime;
            } else {
                stopMotor();
            }
            lastButtonPressTime = currentTime;
        }
    }

    // Lectura del potenciómetro y ajuste de velocidad
    int potValue = analogRead(potPin);
    int motorSpeed = map(potValue, 0, 1023, 0, 255);
    analogWrite(enablePin, motorSpeed);

    // Actualización del estado de los LEDs
    updateLeds(motorSpeed);

    // Verificación del tiempo de inactividad
    if (motorRunning && currentTime - motorStartTime > MOTOR_TIMEOUT) {
        stopMotor();
        motorRunning = false;
    }

    // Salida de información para depuración
    Serial.print("Motor running: ");
    Serial.print(motorRunning ? "Yes" : "No");
    Serial.print(", Motor speed: ");
    Serial.println(motorSpeed);

    delay(200);
}

void startMotor() {
    digitalWrite(motorPin1, HIGH);
    digitalWrite(motorPin2, LOW);
    Serial.println("Motor started");
}

void stopMotor() {
    digitalWrite(motorPin1, LOW);
    digitalWrite(motorPin2, LOW);
    analogWrite(enablePin, 0);
    Serial.println("Motor stopped");
}

void updateLeds(int motorSpeed) {
    int numberOfLeds = map(motorSpeed, 0, 255, 0, 5);
    for (int i = 0; i < 5; i++) {
        digitalWrite(ledPins[i], i < numberOfLeds ? HIGH : LOW);
    }
}

void blinkLed(int pin) {
    for (int i = 0; i < LED_BLINK_COUNT; i++) {
        digitalWrite(pin, HIGH);
        delay(LED_BLINK_DELAY);
        digitalWrite(pin, LOW);
        delay(LED_BLINK_DELAY);
    }
}
