## Practica 7 

En esta practica utilizaremos un modulo I2S DAC para reproducir un fragmento de audio que almacenaremos en la memoria integrada en la ESP32

**Codigo**
```cpp
#include "Arduino.h"
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"

AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;

void setup(){

  Serial.begin(115200);
  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out -> SetGain(0.125);
  out -> SetPinout(26,25,22);
  aac->begin(in, out);

}

void loop(){
  if (aac->isRunning()) {
    aac->loop();
    } else {

      aac -> stop();
      Serial.printf("Sound Generator\n");
      delay(1000);
  }
}
```
**Libreiras**
```cpp
#include "Arduino.h"
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"
```
En este programa hay muchas librerias pero se incluyen todas en una general Audio ESP32 ademas tambien incluimos la arduino para poder hacer funciones basicas 

**Declaraciones**

```cpp
AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;
```
Creamos los objetos relacionados con la Reproduccion del audio que utilizaremos mas tarde.

**SETUP**
```cpp
void setup(){

  Serial.begin(115200);
  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out -> SetGain(0.125);
  out -> SetPinout(26,25,22);
  aac->begin(in, out);

}
```

En el Setup iniciamos el serial, almacenamos el sample de audio en la memoria de la ESP y asignamos valores a las variables de audio y finalmente configuramos el DAC

**LOOP**
```cpp

void loop(){
  if (aac->isRunning()) {
    aac->loop();
    } else {

      aac -> stop();
      Serial.printf("Sound Generator\n");
      delay(1000);
  }
}

```
En el loop accionamos el audio con una condicional y hacemos un delay para que se vaya reproduciendo en bucle

NOTA: 
para poder reproducir el archivod e audio este ha de estar en la carpeta donde se encuentra el codigo y este tiene que estar en formato .h escrito cada muestra en formato hexadecimal para que se pueda almacenar en la memoria de la ESP32.
VIDEO DEMO: https://drive.google.com/file/d/11PUDTBNX9zNMiMFrIGXPJR-A9EAWhxNN/view?usp=sharing