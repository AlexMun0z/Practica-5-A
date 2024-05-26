# Practica 5-A
## Part A: ESCÀNER I2C
### Codi Complet
```cpp
#include <Arduino.h>
#include <Wire.h> 

void setup() {
   Wire.begin(); 
    Serial.begin(115200); 
    while (!Serial); // Leonardo: wait for serial monitor 
     Serial.println("\nI2C Scanner");
}

void loop() {
   byte error, address; 
   int nDevices; 
   Serial.println("Scanning..."); 
   nDevices = 0; 
   for(address = 1; address < 127; address++ ) 
   { 
    // The i2c_scanner uses the return value of 
    // the Write.endTransmisstion to see if 
    // a device did acknowledge to the address. 
     Wire.beginTransmission(address); 
     error = Wire.endTransmission(); 
     if (error == 0) 
     { 
      Serial.print("I2C device found at address 0x"); 
      if (address<16) 
        Serial.print("0"); 
        Serial.print(address,HEX); 
        Serial.println("  !"); 
        nDevices++; 
     } 
     else if (error==4) 
     { 
      Serial.print("Unknown error at address 0x"); 
      if (address<16) 
        Serial.print("0"); 
        Serial.println(address,HEX); 
     }     
  } 
  if (nDevices == 0) 
    Serial.println("No I2C devices found\n"); 
  else 
    Serial.println("done\n"); 
    delay(5000);           // wait 5 seconds for next scan
}
```
### Funcionament per parts del codi

__1. Inclusió de Biblioteques__
```cpp
#include <Arduino.h>
#include <Wire.h> 
```
- Arduino.h: Biblioteca estàndart d'Arduino
- Wire.h: Proporciona la funcionalitat necessària per a la comunicació I2C.
    
__2. Setup__
```cpp
void setup() {
   Wire.begin(); 
    Serial.begin(115200); 
    while (!Serial); // Leonardo: wait for serial monitor 
     Serial.println("\nI2C Scanner");
}
```
S'estableix la configuració inicial del programa:
- Wire.begin(): Inicialitza la comunicació I2C.
- Serial.begin(115200): Inicialitza la comunicació sèrie en 115200 bauds.
- while (!Serial);: Espera que s'estableixi la comunicació serial.
- Serial.println("\nI2C Scanner"): Imprimeix el missatge "I2C Scanner" al monitor serial per indicar que l'escàner ha començat.
  
__3. Loop__
```cpp
void loop() {
   byte error, address; 
   int nDevices; 
   Serial.println("Scanning..."); 
   nDevices = 0; 
   for(address = 1; address < 127; address++ ) 
   { 
    // The i2c_scanner uses the return value of 
    // the Write.endTransmisstion to see if 
    // a device did acknowledge to the address. 
     Wire.beginTransmission(address); 
     error = Wire.endTransmission(); 
     if (error == 0) 
     { 
      Serial.print("I2C device found at address 0x"); 
      if (address<16) 
        Serial.print("0"); 
        Serial.print(address,HEX); 
        Serial.println("  !"); 
        nDevices++; 
     } 
     else if (error==4) 
     { 
      Serial.print("Unknown error at address 0x"); 
      if (address<16) 
        Serial.print("0"); 
        Serial.println(address,HEX); 
     }     
  } 
  if (nDevices == 0) 
    Serial.println("No I2C devices found\n"); 
  else 
    Serial.println("done\n"); 
    delay(5000);           // wait 5 seconds for next scan
}

```
- Serial.println("this is ESP32 Task");: Envia un missatge a través de la comunicació sèrie.
- Serial.println("Scanning..."): Imprimeix el missatge "Scanning..." al monitor serial per indicar que està començant una nova exploració.
- nDevices = 0: Inicialitza el comptador de dispositius trobats a 0.
- Wire.beginTransmission(address): Inicia una transmissió I2C a l'adreça address
- error = Wire.endTransmission(): Acaba la transmissió i captura el codi d'error. Un error de 0 indica que s'ha trobat un dispositiu en aquesta adreça. Si error==0: Imprimeix l'adreça del dispositiu trobat en format hexadecimal.
  Incrementa el comptador nDevices.
  Si error==4 indica un error desconegut a l'adreça.

  Si després d'escanejar totes les adreçes no ha trobat cap dispositiu, treu per pantalla el missatge: No I2C devices found. Si ha trobat dispositius imprimeix el missatge: done.


Resum del funcionament: 

Es realitza contínuament escaneigs del bus I2C i reporta les adreces dels dispositius trobats, repetint el procés cada 5 segons.
És útil per verificar i diagnosticar la presència de dispositius I2C connectats a la placa Arduino.
