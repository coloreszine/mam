# mam

#include <MIDIUSB.h>
const int SENSOR = A0;
const int ADC_MIN = 0;     // Reemplazar por la calibración medida.
const int ADC_MAX = 1023;  // Siempre mayor que ADC_MIN.

void enviar(byte nota, bool activar) {
  midiEventPacket_t paquete = {
    (byte)(activar ? 0x09 : 0x08),
    (byte)(activar ? 0x90 : 0x80),  // Canal MIDI 1.
    nota,
    (byte)(activar ? 80 : 0)
  };
  MidiUSB.sendMIDI(paquete);
  MidiUSB.flush();
}

void setup() {
  pinMode(SENSOR, INPUT);
}

void loop() {
  int lectura = constrain(analogRead(SENSOR), ADC_MIN, ADC_MAX);
  byte nota = (byte)map(lectura, ADC_MIN, ADC_MAX, 48, 84);
  enviar(nota, true);
  delay(100);
  enviar(nota, false);
  delay(150);
}
