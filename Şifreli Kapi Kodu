#include <Keypad.h>
#include <Servo.h>
#include <EEPROM.h>
#include <LiquidCrystal.h>

#define yled 5  //green led
#define kled 10   // red led
#define role 1
#define buzzer 13


int Button = 8;           //Butonun bağlı olduğu pin

const byte numRows = 4;         //tuş takımındaki satır sayısı
const byte numCols = 4;         //tuş takımındaki stun sayısı

char keymap[numRows][numCols] =
{
  {'1', '2', '3', 'A'},
  {'4', '5', '6', 'B'},
  {'7', '8', '9', 'C'},
  {'*', '0', '#', 'D'}
};

char keypressed;                 //Anahtarların saklandığı yer 
char code[] = {'1', '2', '3', '4', '5'}; //varsayılan şifre

char check1[sizeof(code)];  //Yeni anahtarın saklandığı yer
char check2[sizeof(code)];  //Yeni anahtarın bir öncekiyle karşılaştırılabilmesi için tekrar saklandığı yer

short a = 0, i = 0, s = 0, j = 0; //Daha sonra kullanılan değişkenler

byte rowPins[numRows] = {1,2,3,4}; //Rows un bağlı olduğu pin numaraları
byte colPins[numCols] = {9,6,7,12}; //Cols un bağlı olduğu pin numaraları

LiquidCrystal lcd(A0,A1,A2,A3,A4,A5);
Keypad myKeypad = Keypad(makeKeymap(keymap), rowPins, colPins, numRows, numCols);
Servo myservo;

void setup()
{
                  
  pinMode(yled, OUTPUT);
  pinMode(buzzer, OUTPUT);
  pinMode(kled, OUTPUT);
  pinMode(role, OUTPUT);
  
  lcd.begin (16, 2);
  lcd.setCursor(0, 0);
  lcd.print("*Sifre Giriniz*");
  lcd.setCursor(1 , 1);

  lcd.print("Hosgeldiniz!!");     
  pinMode(Button, INPUT);
  myservo.attach(11);
  myservo.write(0);

}


void loop()
{
  digitalWrite(buzzer, LOW);
  digitalWrite(kled, LOW);
  digitalWrite(yled, LOW);

  keypressed = myKeypad.getKey();               //Sürekli bir tuşa basılmasını bekle
  if (keypressed == '*') { // kilidi açmak için önce *  tuşuna basılır
    
    digitalWrite(buzzer, HIGH);
    delay(100);
    digitalWrite(buzzer, LOW);
    
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.println("*SIFRE*");            
    ReadCode();                         
    if (a == sizeof(code))       
      OpenDoor();                  
    else {
      lcd.clear();
      lcd.setCursor(1, 0);
      lcd.print("SIFRE"); 
      lcd.setCursor(6, 0);
      lcd.print("YANLIS");
      lcd.setCursor(15, 1);
      lcd.print(" ");
      lcd.setCursor(4, 1);
      lcd.print("TEKRAR");
      
        digitalWrite(yled, LOW);
        digitalWrite(kled, HIGH);
        digitalWrite(buzzer, HIGH);
        delay(1000);
        digitalWrite(buzzer, LOW);
        delay(1000);
      
    }
    delay(2000);
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("*Sifre Giriniz*");
    lcd.setCursor(1 , 1);

    lcd.print("Hosgeldiniz!!");
    
  }

  if (keypressed == '#') {                //şifre değiştirme ekranına giriş yapmak için # tuşuna basılır
    ChangeCode();
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("*Sifre Giriniz*");
    lcd.setCursor(1 , 1);

    lcd.print("Hosgeldiniz!!");                 
  }

  if (digitalRead(Button) == HIGH) {  //buton düğmesi ile açma
    myservo.write(0);
    
    digitalWrite(buzzer, HIGH);
      delay(100);
      digitalWrite(buzzer, LOW);
    
  }

}

void ReadCode() {                 
  i = 0;                   
  a = 0;
  j = 0;

  while (keypressed != 'A') {                                   //A tuşu ile girilen şifreyi onaylama
    keypressed = myKeypad.getKey();
    if (keypressed != NO_KEY && keypressed != 'A' ) {    
      lcd.setCursor(j, 1);                                
      lcd.print("*");
      j++;
      if (keypressed == code[i] && i < sizeof(code)) {       
        a++;
        i++;
      }
      else
        a--;                                            
    }
  }
  keypressed = NO_KEY;

}

void ChangeCode() {                     
  lcd.clear();
  lcd.print("Sifre Degistir");
  delay(1000);
  lcd.clear();
  lcd.print("Mevcut Sifre");
  ReadCode();                     

  if (a == sizeof(code)) { 
    lcd.clear();
    lcd.print("Sifre Degisti");
    GetNewCode1();            
    GetNewCode2();           
    s = 0;
    for (i = 0 ; i < sizeof(code) ; i++) { 
      if (check1[i] == check2[i])
        s++;                               
    }
    if (s == sizeof(code)) {        

      for (i = 0 ; i < sizeof(code) ; i++) {
        code[i] = check2[i];       
        EEPROM.put(i, code[i]);       

      }
      lcd.clear();
      lcd.print("Sifre Degisti");
      delay(2000);
    }
    else {                        
      lcd.clear();
      lcd.print("Yanlis Sifre");
      lcd.setCursor(0, 1);
      lcd.print("Sifre Eslesmedi!!");
      delay(2000);
    }

  }

  else {                 
    lcd.clear();
    lcd.print("Yanlis");
    delay(2000);
  }
}

void GetNewCode1() {
  i = 0;
  j = 0;
  lcd.clear();
  lcd.print("Yeni Sifre Gir");   
  lcd.setCursor(0, 1);
  lcd.print("A Tusuna Bas");
  delay(2000);
  lcd.clear();
  lcd.setCursor(0, 1);
  lcd.print("A Tusuna Bas");    

  while (keypressed != 'A') {         
    keypressed = myKeypad.getKey();
    if (keypressed != NO_KEY && keypressed != 'A' ) {
      lcd.setCursor(j, 0);
      lcd.print("*");               
      check1[i] = keypressed;   
      i++;
      j++;
    }
  }
  keypressed = NO_KEY;
}

void GetNewCode2() {                        
  i = 0;
  j = 0;

  lcd.clear();
  lcd.print("Sifre Onayla");
  lcd.setCursor(0, 1);
  lcd.print("A Tusuna Bas");
  delay(3000);
  lcd.clear();
  lcd.setCursor(0, 1);
  lcd.print("A Tusuna Bas");

  while (keypressed != 'A') {
    keypressed = myKeypad.getKey();
    if (keypressed != NO_KEY && keypressed != 'A' ) {
      lcd.setCursor(j, 0);
      lcd.print("*");
      check2[i] = keypressed;
      i++;
      j++;
    }
  }
  keypressed = NO_KEY;
}

void OpenDoor() {            
  lcd.clear();
  lcd.setCursor(1, 0);
  lcd.print("Kapi Acildi");
  lcd.setCursor(4, 1);
  lcd.print("Hosgeldiniz!!");
  myservo.write(90);
  
  digitalWrite(yled, HIGH);
  digitalWrite(kled, LOW);
  digitalWrite(buzzer, HIGH);
  delay(100);
  digitalWrite(buzzer, LOW);
  delay(100);
  digitalWrite(buzzer, HIGH);
  delay(100);
  digitalWrite(buzzer, LOW);
  delay(1000);
  
 


}
