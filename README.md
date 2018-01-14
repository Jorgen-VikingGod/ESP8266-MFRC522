# ESP8266-MFRC522
MFRC522 RFID module connected to ESP8266 (ESP-12) WiFi module

Many thanks to nikxha from the ESP8266 forum (for initial work) and [omersiar](https://github.com/omersiar) (for more details of wiring and additional examples)

## Requirements
You have to install Arduino IDE 1.6.10 or later. Links are listed below.
* **Arduino** > **Preferences** > "Additional Boards Manager URLs:" and add: **http://arduino.esp8266.com/stable/package_esp8266com_index.json**
* **Arduino** > **Tools** > **Board** > **Boards Manager** > type in **ESP8266** and install the board
* Download MFRC522 library and extract to Arduino library folder or you can use "Library Manager" on the IDE itself to install the MFRC522 library.

### Download Links
* [Arduino IDE](https://www.arduino.cc/en/Main/Software) - The development IDE
* [ESP8266 Core for Arduino IDE](https://github.com/esp8266/Arduino) - ESP8266 Core
* [MFRC522 Library](https://github.com/miguelbalboa/rfid) - MFRC522 RFID Library

## Simple Example

### Wiring RFID RC522 module
The following table shows the typical pin layout used:

| Signal        | MFRC522       | WeMos D1 mini  | NodeMcu | Generic      |
|---------------|:-------------:|:--------------:| :------:|:------------:|
| RST/Reset     | RST           | D3 [1]         | D3 [1]  | GPIO-0 [1]   |
| SPI SS        | SDA [3]       | D8 [2]         | D8 [2]  | GPIO-15 [2]  |
| SPI MOSI      | MOSI          | D7             | D7      | GPIO-13      |
| SPI MISO      | MISO          | D6             | D6      | GPIO-12      |
| SPI SCK       | SCK           | D5             | D5      | GPIO-14      |

* [1] (1, 2) Configurable, typically defined as RST_PIN in sketch/program.
* [2]	(1, 2) Configurable, typically defined as SS_PIN in sketch/program.
* [3]	The SDA pin might be labeled SS on some/older MFRC522 boards

### Define RFID module
```arduino
#include "MFRC522.h"
#define RST_PIN	5 // RST-PIN for RC522 - RFID - SPI - Modul GPIO5 
#define SS_PIN	4 // SDA-PIN for RC522 - RFID - SPI - Modul GPIO4 
MFRC522 mfrc522(SS_PIN, RST_PIN);	// Create MFRC522 instance
```

### Initialize RFID module
```arduino
void setup() {
  Serial.begin(115200);    // Initialize serial communications
  SPI.begin();	         // Init SPI bus
  mfrc522.PCD_Init();    // Init MFRC522
}
```

### Read RFID tag
```arduino
void loop() { 
  // Look for new cards
  if ( ! mfrc522.PICC_IsNewCardPresent()) {
    delay(50);
    return;
  }
  // Select one of the cards
  if ( ! mfrc522.PICC_ReadCardSerial()) {
    delay(50);
    return;
  }
  // Show some details of the PICC (that is: the tag/card)
  Serial.print(F("Card UID:"));
  dump_byte_array(mfrc522.uid.uidByte, mfrc522.uid.size);
  Serial.println();
}

// Helper routine to dump a byte array as hex values to Serial
void dump_byte_array(byte *buffer, byte bufferSize) {
  for (byte i = 0; i < bufferSize; i++) {
    Serial.print(buffer[i] < 0x10 ? " 0" : " ");
    Serial.print(buffer[i], HEX);
  }
}
```

## More Examples
* [ESP RFID](https://github.com/omersiar/esp-rfid) - Embedded RFID Access Control Project for MFRC522
* [Paralelni Polis RFID Access System](https://github.com/ParalelniPolis/rfid-locks) - Access Control with Server - Client communication.
