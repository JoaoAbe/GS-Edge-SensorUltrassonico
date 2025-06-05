# 🌎 Global Solution: Edge Computing & IoT  
## Sistema Físico de Monitoramento de Nível de Bueiros com Sensor Ultrassônico

---

## 🌧️ Enchentes no Brasil

O Brasil possui um **clima tropical**, caracterizado por **temperaturas elevadas** e **grandes volumes de chuva**, especialmente entre **dezembro e março**. As enchentes são fenômenos naturais, mas foram **agravadas** pela ação **antrópica**.

Entre os fatores que causam enchentes, destacam-se:

- Encurtamento dos rios  
- Excesso de chuvas  
- Impermeabilização do solo  
- **Incapacidade de escoamento da água pelos bueiros**  

Nosso projeto busca solucionar este último problema.

---

## 🚧 O que são Bueiros?

Bueiros são sistemas de drenagem responsáveis pelo **escoamento do excesso de água** nas cidades, prevenindo alagamentos e enchentes.

---

## ❗ Problema

A falta de manutenção e conscientização faz com que os bueiros sejam frequentemente **entupidos por lixo** ou pela **acumulação excessiva de água**, impedindo o escoamento adequado e aumentando o risco de enchentes.

---

## 💡 Solução

Desenvolvemos um sistema de **monitoramento do nível de enchimento dos bueiros** utilizando um **sensor ultrassônico HC-SR04**.

O funcionamento ocorre da seguinte forma:

- **Luz verde** e mensagem no display: Bueiro **vazio**  
- **Luz amarela** e mensagem no display: Bueiro com **enchimento de 50%**  
- **Luz vermelha**, mensagem no display e **alerta sonoro**: Bueiro **cheio**  

---

## 🧪 Testes e Simulações

O sistema foi:

✅ **Simulado no Tinkercad**  
✅ **Testado com sucesso** nas condições propostas

---

## 👨‍💻 Integrantes do Grupo

- Vitor Fernandes dos Santos — RM: 566275  
- João Victor de Souza Abe — RM: 561446  

---

## 🧰 Tecnologias Utilizadas

- Arduino IDE  
- Tinkercad  

---

## 🎥 Link para Vídeo

> ([Link do vídeo no youtube](https://youtu.be/bQoJ8NPJZyc?si=Z4Nf33kLZ8Bi0wkF))

---

## 🧪 Link para a Simulação

> ([Link para a simulação](https://www.tinkercad.com/things/ifaF2c9QLoC-sensor-de-profundidade-alerta/editel?returnTo=https%3A%2F%2Fwww.tinkercad.com%2Fdashboard&sharecode=P3jMJxFlV9pO-3SKpiqX_1JeiHSXHl28CcfAyv95snM))

---

## 🔧 Materiais Utilizados

- 1 Arduino Uno  
- 1 Buzzer  
- 3 LEDs (verde, amarelo e vermelho)  
- 3 Resistores de 220Ω  
- 1 Sensor ultrassônico HC-SR04  
- 1 Display LCD I2C  
- Jumpers  
- 1 Protoboard  

---

## 🛠️ Como Montar o Projeto

### 1. Sensor Ultrassônico HC-SR04
- VCC → 5V  
- TRIG → Pino 8  
- ECHO → Pino 9  
- GND → GND  

### 2. Display LCD I2C
- VCC → 5V  
- GND → GND  
- SDA → A4  
- SCL → A5  

### 3. LED Vermelho
- Cátodo → Resistor de 220Ω → GND  
- Ânodo → Porta digital 3  

### 4. LED Amarelo
- Cátodo → Resistor de 220Ω → GND  
- Ânodo → Porta digital 4  

### 5. LED Verde
- Cátodo → Resistor de 220Ω → GND  
- Ânodo → Porta digital 5  

### 6. Buzzer
- Positivo → Porta digital 7  
- Negativo → GND  

---

## 📦 Bibliotecas Necessárias

- `<Wire.h>`  
- `<Adafruit_LiquidCrystal.h>`  

---

## 💻 Código Arduino

```cpp
#include <Wire.h> //Bibliotecas para o LCD
#include <Adafruit_LiquidCrystal.h> //Bibliotecas para o LCD

const int ledVerde = 5;
const int ledVermelho = 3;
const int ledAmarelo = 4; 

const int trigPin = 8;
const int echoPin = 9;

float duration, distance;  

const int buzzer = 7;

// Declarando o LCD de forma global usando Adafruit_LiquidCrystal
Adafruit_LiquidCrystal lcd(0);

void setup()
{
  pinMode(ledVerde, OUTPUT);
  pinMode(ledVermelho, OUTPUT);
  pinMode(ledAmarelo, OUTPUT);
  pinMode(buzzer, OUTPUT);
  pinMode(trigPin, OUTPUT); //Trigger como saída
  pinMode(echoPin, INPUT);	//Trigger como entrada
  Serial.begin(9600);
  
  lcd.begin(16, 2); // Inicializa o LCD (16 colunas e 2 linhas)
  lcd.setBacklight(1); // Liga a luz de fundo
}

void loop()
{
  digitalWrite(trigPin, LOW); //Trigger Low
  delayMicroseconds(2);  	
  digitalWrite(trigPin, HIGH); //Trigger High
  delayMicroseconds(10); 	   //Delay de 10 microsegundos para chegar no echo
  digitalWrite(trigPin, LOW);
  
  duration = pulseIn(echoPin, HIGH); //tempo que o pulso levou para ir até o objeto e voltar.
  distance = (duration * 0.0343) / 2;  //multiplica a duração pela velocidade do som e divide por 2 (ida e volta)
  
  Serial.print("Distance: ");  
  Serial.println(distance);  
  delay(100);  
  
  lcd.clear(); // limpa o LCD
  lcd.setCursor(0, 0);
  lcd.print("Dist: "); 
  lcd.print(distance); //Printa a distancia que o sensor capita
  lcd.print(" cm");
  
  if (distance <= 15) {
    digitalWrite(ledVerde, LOW);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVermelho, HIGH);
    digitalWrite(buzzer, HIGH);
    delay(1500);
    digitalWrite(buzzer, LOW);
    delay(1500);
    lcd.setCursor(0,1);
    lcd.print("Alerta!"); 
    delay(3000);
  } 
  else if (distance > 15 && distance <= 150) {
    digitalWrite(ledVerde, LOW);
    digitalWrite(ledAmarelo, HIGH);
    digitalWrite(ledVermelho, LOW);
    digitalWrite(buzzer, LOW);
    lcd.setCursor(0,1);
    lcd.print("Atencao!");
    delay(3000);
  } 
  else {
    digitalWrite(ledVerde, HIGH);
    digitalWrite(ledAmarelo, LOW);
    digitalWrite(ledVermelho, LOW);
    digitalWrite(buzzer, LOW);
    lcd.setCursor(0,1);
    lcd.print("Seguro!");
    delay(3000);
  }
}
