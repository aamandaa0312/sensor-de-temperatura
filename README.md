# 🌡️ Teste com DHT11 e LEDs no ESP32

Este projeto utiliza o sensor DHT11 para leitura de temperatura e umidade com o ESP32. De acordo com a temperatura medida, um LED azul ou vermelho é acionado.
 ![Uploading Image.jpg…]()

## 🛠️ Materiais necessários
- 1x ESP32
- 1x Sensor DHT11
- 1x LED azul
- 1x LED vermelho
- 2x Resistores (220 Ω ou 330 Ω)
- Jumpers
- Protoboard

## 🔌 Ligações
### DHT11
- Pino DATA → Pino 2 do ESP32
- Pino VCC → 3.3V do ESP32
- Pino GND → GND do ESP32

### LEDs
- LED azul → Pino 4 (com resistor em série)
- LED vermelho → Pino 18 (com resistor em série)

## 📜 Código
```cpp
/*
  Read Temperature and Humidity
  DHT11 Library
  Author: Bonezegei (Jofel Batutay)
  Date : November 2023

  Tested using ESP32-WROOM32
*/

#include <Bonezegei_DHT11.h>

// param = DHT11 signal pin
Bonezegei_DHT11 dht(2);

// Definição dos pinos dos LEDs
#define LED_AZUL 4
#define LED_VERMELHO 18

// Definição do limite de temperatura
#define LIMITE_TEMP 25.0

void setup() {
  Serial.begin(115200);
  dht.begin();

  pinMode(LED_AZUL, OUTPUT);
  pinMode(LED_VERMELHO, OUTPUT);

  // Apaga os LEDs no início
  digitalWrite(LED_AZUL, LOW);
  digitalWrite(LED_VERMELHO, LOW);
}

void loop() {
  if (dht.getData()) {                         // get All data from DHT11
    float tempDeg = dht.getTemperature();      // return temperature in celsius
    float tempFar = dht.getTemperature(true);  // return temperature in fahrenheit
    int hum = dht.getHumidity();               // return humidity

    String str  = "Temperature: ";
           str += tempDeg;
           str += "°C  ";
           str += tempFar;
           str += "°F  Humidity:";
           str += hum;
    Serial.println(str.c_str());

    // Controle dos LEDs conforme a temperatura
    if (tempDeg < LIMITE_TEMP) {
      digitalWrite(LED_AZUL, HIGH);
      digitalWrite(LED_VERMELHO, LOW);
    } else {
      digitalWrite(LED_AZUL, LOW);
      digitalWrite(LED_VERMELHO, HIGH);
    }
  }

  delay(2000);  // delay mínimo de 2 segundos para DHT11 ler os dados
}
▶️ Como funciona
O ESP32 lê os dados de temperatura e umidade pelo sensor DHT11.

Se a temperatura estiver abaixo de 25 °C, o LED azul acende.

Se a temperatura estiver igual ou acima de 25 °C, o LED vermelho acende.

Os valores de temperatura (°C e °F) e umidade (%) são exibidos no Serial Monitor.
