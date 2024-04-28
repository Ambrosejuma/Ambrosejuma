#include <SPI.h>
#include <XBee.h>

const int redPin = 9;
const int yellowPin = 8;
const int greenPin = 7;

const int redTime = 30000; // 30 seconds in milliseconds
const int yellowTime = 15000; // 15 seconds in milliseconds
const int greenTime = 30000; // 30 seconds in milliseconds

XBee xbee = XBee(XBee::ADDRESS); // Replace with your XBee address

void setup() {
  pinMode(redPin, OUTPUT);
  pinMode(yellowPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  
  Serial.begin(9600);
  while (!xbee.begin(Serial, XBee::BAUD_57600)) {
    Serial.println("Failed to initialize XBee");
    delay(1000);
  }
}

void loop() {
  // Traffic light sequence
  digitalWrite(redPin, HIGH);
  delay(redTime);
  digitalWrite(redPin, LOW);

  digitalWrite(yellowPin, HIGH);
  delay(yellowTime);
  digitalWrite(yellowPin, LOW);

  digitalWrite(greenPin, HIGH);
  delay(greenTime);
  digitalWrite(greenPin, LOW);

  // Send signal to slaves via XBee
  byte data[] = {1}; // Data packet indicating green light (you can customize)
  xbee.writePacket(0x00, // Destination address (all slaves)
                   0,      // Frame type (can be adjusted based on XBee settings)
                   data, sizeof(data));
  delay(100); // Optional delay between transmissions
}
