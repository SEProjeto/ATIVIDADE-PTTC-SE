# Desenvolvimento

## **Semana 1 - 21/10 a 28/10**

- Definição sobre quem seriam os membros da Equipe;
- Discussão e definição do tema do projeto;
- Criação de um repositório no Github com a introdução do projeto e referências.
________

## **Semana 2- 04/11 a 11/11**

- Alteração do arquivo README.md. Agora ele conta um resumo do projeto e links para os outros documentos do projeto;
- Criação do arquivo introducao.md. A introdução que estava no arquivo README.md foi movida para este arquivo;
- Criação do arquivo materiais.md. Esse arquivo contém todos os componentes necessários para a construção do projeto físico;
- Elaboração do projeto virtual adaptado no Tinkercad.

## Montagem virtual no Tinkercad

### Etapa 1 - Sensor Infravermelho com Controle Remoto, LED e LCD

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
      digitalWrite(pinLed, LOW); // LED Vermelho ligado
      break;
       	
    case 0xFD8877:
      lcd_1.setBacklight(1);
      lcd_1.setCursor(0, 0);
      lcd_1.print("Olho fechado");
      digitalWrite(5, HIGH); // LED Vermelho desligado
    }
     
    irrecv.resume();
    lcd_1.setBacklight(1);
  
  }
  delay(100);

}

```

Imagens: <br>
Figura 1 <br>
<img src="https://user-images.githubusercontent.com/116377673/202001754-e6d60f74-e1b0-4723-a6c7-f36f5651d1c3.png" width="430" height="270" /> <br>
Figura 2 <br>
<img src="https://user-images.githubusercontent.com/116377673/202005083-7e55a992-584c-4311-8dc8-668992b723de.png" width="430" height="270" /> <br>
Figura 3 <br>
<img src="https://user-images.githubusercontent.com/116377673/202004494-c7919249-4326-4f77-8a41-7bfa1dfaf85a.png" width="430" height="270" /> <br>

O nosso protótipo Óculos Anti-Sono tem o objetivo de impedir que motoristas inconscientes causem acidentes nas estradas. O circuito acima simula a versão mais básico do nosso projeto. <br>
Para o projeto virtual, nós tentamos simular os estados de olho aberto e olho fechado com o Sensor Infravermelho e o Controle Remoto. O botão 1 do controle simula o estado de olho aberto e o botão número 2 simula o estado de olho fechado. Quando o botão 2 é pressionado, o LED irá acender, indicando que o motorista esta com o olho fechado. <br>
A tela LCD é usada para ilustrar os estados do circuito. Quando um botão é pressionado, ele muda seu letreiro de acordo. É possível ver seu funcionamento nas figuras acima. 

________

## Etapa 2 - Projeto Final - Sensor Infravermelho com Controle Remoto, LED, Buzzer, Motor de Vibração e Sensor de Inclinação.


Código:
```
#include <IRremote.h>
#include <Adafruit_LiquidCrystal.h>
int RECV_PIN = 11;
int buzzerPin = 7;
int vibratorPin = 6;
int sensorInclinacaoPin = 10;
int estadoSensorInclinacao = 0;
int estadoOlho = LOW; // LOW = FECHADO - HIGH = ABERTO
int ultimoEstadoSensorInclinacao = LOW;

// LCD
int seconds = 0;
Adafruit_LiquidCrystal lcd_1(0);


IRrecv irrecv(RECV_PIN);
decode_results results;
void setup()
{
  pinMode(buzzerPin, OUTPUT);
  pinMode(sensorInclinacaoPin, OUTPUT);
  pinMode(vibratorPin, OUTPUT);
  Serial.begin(9600);
  irrecv.enableIRIn();
  
  //LCD
  lcd_1.begin(16, 2);
  lcd_1.print("1 - Olho Aberto");
  lcd_1.setCursor(0, 1);
  lcd_1.print("2 - Olho Fechado");
  
}

void loop(){
  
  // Sensor infravermelho
  
  if(irrecv.decode(&results)){            
  Serial.println(results.value, HEX);
    switch(results.value){
    case 0xFD08F7: // 1 - Olho Aberto
      lcd_1.clear();
      lcd_1.setCursor(0, 0);
      lcd_1.print("Olho aberto");
      if (estadoSensorInclinacao == HIGH)
  	{
        lcd_1.setBacklight(1);
        lcd_1.setCursor(0, 1);
        lcd_1.print("Inclinado");
  	}
      estadoOlho = LOW;
      break;
       	
    case 0xFD8877: // 2 - Olho Fechado
      lcd_1.clear();
      lcd_1.setCursor(0, 0);
      lcd_1.print("Olho fechado");
      if (estadoSensorInclinacao == HIGH)
  	  {
        lcd_1.setBacklight(1);
        lcd_1.setCursor(0, 1);
        lcd_1.print("Inclinado");
  	  }
      estadoOlho = HIGH;
    }
  }
  
  // Sensor de Inclinação
  estadoSensorInclinacao = digitalRead(sensorInclinacaoPin);
  if (estadoSensorInclinacao == HIGH)
  {
    estadoOlho = HIGH;
  }
  else if (estadoSensorInclinacao == HIGH && results.value == 0xFD08F7 )
  {
    estadoOlho = HIGH;
  }
  else if (estadoSensorInclinacao == LOW && results.value == 0xFD08F7 )
  {
    estadoOlho = LOW;
  }
  
  if (estadoSensorInclinacao != ultimoEstadoSensorInclinacao )
  {
   	lcd_1.clear(); 
    if (results.value == 0xFD8877)
    {
      lcd_1.setCursor(0, 0);
      lcd_1.print("Olho fechado");
    }
    else
    {
      lcd_1.setCursor(0, 0);
      lcd_1.print("Olho aberto");
    }
    
    if (estadoSensorInclinacao == HIGH)
  	{
      lcd_1.setBacklight(1);
      lcd_1.setCursor(0, 1);
      lcd_1.print("Inclinado");
  	}
  }

   ultimoEstadoSensorInclinacao = estadoSensorInclinacao;
   digitalWrite(5, estadoOlho); 
   digitalWrite(6, estadoOlho);
   digitalWrite(7, estadoOlho);
   irrecv.resume();
   lcd_1.setBacklight(1);
}

```

Imagens: <br>

Figura 4 <br>
<img src="https://user-images.githubusercontent.com/116377673/202731028-af67ff95-98a6-4572-ab15-763eeedfc5be.png" width="430" height="270" /> <br>
Figura 5 <br>
<img src="https://user-images.githubusercontent.com/116377673/202731627-19a07fcd-c9a7-47fe-89dc-44b298209e55.png" width="430" height="270" /> <br>
Figura 6 <br>
<img src="https://user-images.githubusercontent.com/116377673/202731913-f7e28818-fb3d-4fba-b0c6-bef504274c0d.png" width="430" height="270" /> <br>
Figura 7 <br>
<img src="https://user-images.githubusercontent.com/116377673/202732334-a8bb3e11-3fc9-4781-86fe-3d3e7dcd5756.png" width="430" height="270" /> <br>
Figura 8 <br>
<img src="https://user-images.githubusercontent.com/116377673/202732511-f92b1264-9f91-4709-8c8b-1268cfacf8be.png" width="430" height="270" /> <br>

O nosso projeto final contará com mais três compononentes, além dos já vistos anteriormente na Etapa 1. Eles são: 1) Buzzer; 2) Motor de Vibração; 3) Sensor de Inclinação. <br>

Os componentes Buzzer e Moto de Vibração foram introduzidos como um Sistema de Alarme para alertar o motorista que os seus olhos estão fechados. O Buzzer funciona emitindo um ruído e o Moto de Vibração, como o próprio nome diz, vibra. Eles só serão ativados quando o botão 2 do Controle Remoto for pressionado e quando o Sensor de Inclinação estiver no seu estado ativo (HIGH).  <br>

O Sensor de Inclinação foi introduzido ao nosso projeto para verificar se o motorista esta em uma posição de estado inconsciente.  O sensor trabalha com dois estados: não inclinado (LOW) e inclinado (HIGH). Se o motorista estiver inclinado, o Sistema de Alarme será ativado, caso ao contrário, nada acontece. O Sensor de Inclinação funciona de forma independente, isso quer dizer que o Sistema de Alarme será ativado mesmo que o botão 1, o botão que simula o estado de olho aberto, do Controle Remoto seja pressionado (Figura 8). <br>

O código da tela de LCD foi adaptado para atender o Sensor de Inclinação.

________

## **Semana 3 - 15/11 a 18/11**

- Criação do arquivo Desenvolvimento.md;
- Finalização do projeto virtual;
- Apresentação final do projeto.
