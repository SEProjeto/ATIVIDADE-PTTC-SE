# Desenvolvimento

## **Semana 1 - 21/02 a 28/02**

- Definição sobre quem seriam os membros da Equipe;
- Discussão e definição do tema do projeto;
- Criação de um repositório no Github com a introdução do projeto e referências.
________

## **Semana 2- 04/02 a 11/02**

- Alteração do arquivo README.md. Agora ele conta um resumo do projeto e links para os outros documentos do projeto;
- Criação do arquivo introducao.md. A introdução que estava no arquivo README.md foi movida para este arquivo;
- Criação do arquivo materiais.md. Esse arquivo contém todos os componentes necessários para a construção do projeto físico;
- Elaboração do projeto virtual adaptado no Tinkercad.

### Montagem virtual no Tinkercad

## Etapa 1 - Sensor Infravermelho com controle remoto, LED e LCD


Código:
```
#include <IRremote.h>
#include <Adafruit_LiquidCrystal.h>
int RECV_PIN = 11;
int pinLed = 5; 

// LCD
int seconds = 0;
Adafruit_LiquidCrystal lcd_1(0);

// Controle IR
IRrecv irrecv(RECV_PIN);
decode_results results;

void setup()
{
  Serial.begin(9600);
  irrecv.enableIRIn();
  lcd_1.begin(16, 2);
}

void loop(){
  lcd_1.setBacklight(1);
  
  // Sensor infravermelho
  if(irrecv.decode(&results)){             //1 is turn led on
  Serial.println(results.value, HEX);
    switch(results.value){
    case 0xFD08F7:
      lcd_1.setBacklight(1);
      lcd_1.setCursor(0, 0);
      lcd_1.print("Olho aberto");
      digitalWrite(pinLed, HIGH); // LED Vermelho ligado
      break;
       	
    case 0xFD8877:
      lcd_1.setBacklight(1);
      lcd_1.setCursor(0, 0);
      lcd_1.print("Olho fechado");
      digitalWrite(5, LOW); // LED Vermelho desligado
    }
     
    irrecv.resume();
    lcd_1.setBacklight(1);
  
  }
  delay(100);

}

```

Imagens: <br>
<img src="https://user-images.githubusercontent.com/116377673/202001754-e6d60f74-e1b0-4723-a6c7-f36f5651d1c3.png" width="430" height="270" /> <br>
<img src="https://user-images.githubusercontent.com/116377673/202005083-7e55a992-584c-4311-8dc8-668992b723de.png" width="430" height="270" /> <br>
<img src="https://user-images.githubusercontent.com/116377673/202004494-c7919249-4326-4f77-8a41-7bfa1dfaf85a.png" width="430" height="270" /> <br>

O circuito acima simula a versão mais básico do nosso protótipo. Ele conta com um sensor infravermelho adaptado para o uso dentro do ambiente virtual com o controle remoto. Como não é possivel simular proximidade em um espaço fisico dentro do Tinkercad, nós adaptamos o projeto para que o botão 1 do controle remoto seja o estado de quando o utilizador do óculos esteja com os olhos fechados e o botão 2 simular quando ele estiver com o olho aberto. Quando o botão é pressionado, o LED acende, dizendo que o motorista está com o olho aberto.
________

## Etapa 2 - Sensor Infravermelho com controle remoto e LED


Código:
```
#include <IRremote.h>
#include <Adafruit_LiquidCrystal.h>
int RECV_PIN = 11;
int buzzerPin = 7;
int vibratorPin = 6;
int ledPin = 5;

// LCD
int seconds = 0;
Adafruit_LiquidCrystal lcd_1(0);


IRrecv irrecv(RECV_PIN);
decode_results results;
void setup()
{
  pinMode(buzzerPin, OUTPUT);
  pinMode(vibratorPin, OUTPUT);
  Serial.begin(9600);
  irrecv.enableIRIn();
  
  //LCD
  lcd_1.begin(16, 2);
  
}

void loop(){
  lcd_1.setBacklight(1);
  
  // Sensor infravermelho
  if(irrecv.decode(&results)){             //1 is turn led on
  Serial.println(results.value, HEX);
    switch(results.value){
    case 0xFD08F7:
      lcd_1.setBacklight(1);
      lcd_1.setCursor(0, 0);
      lcd_1.print("Olho aberto");
      digitalWrite(ledPin, HIGH);
      digitalWrite(buzzerPin, HIGH);
      digitalWrite(vibratorPin, HIGH);
      break;
       	
    case 0xFD8877:
      lcd_1.setBacklight(1);
      lcd_1.setCursor(0, 0);
      lcd_1.print("Olho fechado");
      digitalWrite(ledPin, LOW); // Red Dot is Off
      digitalWrite(buzzerPin, LOW);
      digitalWrite(vibratorPin, LOW);
    }
    irrecv.resume();
    lcd_1.setBacklight(1);
  
  }
  delay(100);

}

```

Imagens: <br>
<img src="https://user-images.githubusercontent.com/116377673/202010671-3a352dc3-c847-4bd6-979f-3f9ac9695b88.png" width="430" height="270" /> <br>
<img src="https://user-images.githubusercontent.com/116377673/202011563-1f6b1fa8-eb61-4e4a-b229-30ebea110c20.png" width="430" height="270" /> <br>
<img src="https://user-images.githubusercontent.com/116377673/202011385-f8803244-8d66-4155-a20f-70df6eeedcfb.png" width="430" height="270" /> <br>



