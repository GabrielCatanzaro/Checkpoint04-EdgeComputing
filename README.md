# Checkpoint04-EdgeComputing

integrantes:

Filipe Prado menezes / RM: 98765
Gabriel Gomes Catanzaro / RM:93445
Lucas Gomes Alcântara / RM: 98766
Pedro Henrique Salvitti / RM: 88166

----------------------------------------------------

Sensores:

Sensor Ultrassonico HC-SR04 / Sensor: LDR / Sensor: Sensor DHT11.

----------------------------------------------------

Código:

#include <DHT.h>
#include <ArduinoJson.h>
const int TAMANHO = 150;

#define DHTPIN 2
#define DHTTYPE DHT11
DHT dht(DHTPIN, DHTTYPE);

const int LDR_PIN = A0;
const int TRIG_PIN = 7;
const int ECHO_PIN = 8;

void setup() {
  Serial.begin(9600);
  dht.begin();
  pinMode(LDR_PIN, INPUT);
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
}

void loop() {

  StaticJsonDocument<TAMANHO> json;
  
  float temperature = dht.readTemperature();
  float humidity = dht.readHumidity();
  int ldrValue = analogRead(LDR_PIN);

  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  float duration = pulseIn(ECHO_PIN, HIGH);
  float distance = duration * 0.0343 / 2;


  json["temperature"] = temperature;
  json["humidity"] = humidity ;
  json["ldr"] = ldrValue;
  json["distance"] = distance;

  serializeJson(json, Serial);
  Serial.println();
  
  delay(2000);
}

----------------------------------------------------

Montagem do hardware:

- LDR:
  - Conecte um terminal do LDR ao pino analógico A0 do Arduino.
  - Conecte o outro terminal do LDR a um resistor de 10k ohms.
  - Conecte a outra extremidade do resistor ao GND (terra) do Arduino.

- DHT11:
  - Conecte o pino de dados do DHT11 ao pino digital 2 do Arduino.
  - Conecte o VCC do DHT11 ao 5V do Arduino.
  - Conecte o GND do DHT11 ao GND do Arduino.

- Sensor ultrassônico HC-SR04:
  - Conecte o pino Trigger do HC-SR04 ao pino digital 7 do Arduino.
  - Conecte o pino Echo do HC-SR04 ao pino digital 8 do Arduino.
  - Conecte o VCC do HC-SR04 ao 5V do Arduino.
  - Conecte o GND do HC-SR04 ao GND do Arduino.

----------------------------------------------------

Configuração e exibição Node-Red:

Configuramos a comunicação com o arduino e criamos um fluxo para exibir os dados dos sensores.

- Adicione um nó "Serial In" para receber os dados do Arduino.
- Conecte o nó "Serial In" a um nó "Function" para processar os dados.
- Use o nó "Dashboard" para criar painéis de exibição para os valores do LDR, DHT11 e sensor ultrassônico.

Usamos o nó "Function" para separar os dados recebidos da porta serial e enviá-los para os nós de exibição no Dashboard.

Configuramos os nós de exibição no Dashboard para mostrar os valores do LDR, DHT11 e sensor ultrassônico de forma clara.


