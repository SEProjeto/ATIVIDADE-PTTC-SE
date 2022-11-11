## O que foi feito em cada etapa:

1ºEtapa









## Código-Fonte:

#include <IRremote.h>
#include <Adafruit_LiquidCrystal.h>
int RECV_PIN = 11;
int buzzerPin = 7;
int vibratorPin = 6;
int sensorInclinacaoPin = 10;
int estadoSensorInclinacao = 0;

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
  //lcd_1.print("hello world");
  
}

void loop(){
  
  // LCD 
  //lcd_1.setCursor(0, 1);
  //lcd_1.print(seconds);
  lcd_1.setBacklight(1);
  //delay(500); // Wait for 500 millisecond(s)
  //lcd_1.setBacklight(0);
  //delay(500); // Wait for 500 millisecond(s)
  //seconds += 1;
  
  
  
  
  // Sensor infravermelho
  
  if(irrecv.decode(&results)){             //1 is turn led on
  Serial.println(results.value, HEX);
    switch(results.value){
    case 0xFD08F7:
      lcd_1.setBacklight(1);
      lcd_1.setCursor(0, 0);
      lcd_1.print("Olho aberto");
      digitalWrite(5, HIGH);
      digitalWrite(6, HIGH);
      digitalWrite(7, HIGH);
      break;
       	
    case 0xFD8877:
      lcd_1.setBacklight(1);
      lcd_1.setCursor(0, 0);
      lcd_1.print("Olho fechado");
      digitalWrite(5, LOW); // Red Dot is Off
      digitalWrite(6, LOW);
      digitalWrite(7, LOW);
    }
    
    // Sensor de Inclinação
  estadoSensorInclinacao = digitalRead(sensorInclinacaoPin);
  if (estadoSensorInclinacao == 1)
  {
    lcd_1.setBacklight(1);
    lcd_1.setCursor(0, 0);
    lcd_1.print("Inclinado");
    digitalWrite(5, HIGH);
    if (results.value == 0xFD08F7 ) 
    {
      lcd_1.setBacklight(1);
      lcd_1.setCursor(0, 1);
      lcd_1.print("Inclinado");
    }
  }
  else
  {
  	digitalWrite(5, LOW);
  }
    
    
    
    irrecv.resume();
    lcd_1.setBacklight(1);
  
  }
  delay(100);

}
