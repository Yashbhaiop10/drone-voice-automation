# drone-voice-automation
Drone voice command automation
#include <SoftwareSerial.h>
#include <Wire.h>
#include <Adafruit_SSD1306.h>
#include <Adafruit_GFX.h>
#include <Servo.h>

SoftwareSerial mySerial(10, 11);  // RX, TX pins for the HC-05 module
Adafruit_SSD1306 display(128, 32, &Wire, -1); // OLED Display I2C pins
Servo myservo; // Servo object to control drone motor

void setup() {
  display.begin(SSD1306_SWITCHCAPVCC, 0x3C);
  display.display();
  delay(2000);
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(WHITE);
  display.setCursor(0,0);
  display.println("Voice Controlled Drone");
  display.display();
  delay(2000);
  
  mySerial.begin(9600);
  myservo.attach(9); // Connect drone motor to PWM pin 9
  myservo.write(0); // Set motor speed to 0 initially
}

void loop() {
  String voiceData;
  while (mySerial.available()) {
    char c = mySerial.read();
    voiceData += c;
    delay(10);
  }

  if (voiceData.length() > 0) {
    if (voiceData.indexOf("take off") >= 0) {
      myservo.write(180);
      delay(30
