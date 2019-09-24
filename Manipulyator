#include <Wire.h>
#include <Servo.h> //используем библиотеку для работы с сервоприводом
//прототипы функций
void sendData();
//Сервоприводы
Servo servo1; //объявляем переменную servo типа Servo1
Servo servo2; //объявляем переменную servo типа Servo2
Servo servo3; //объявляем переменную servo типа Servo3
Servo servo4; //объявляем переменную servo типа Servo4

//адрес устройства
#define SLAVE_ADDRESS 0x05
int number = 0;
int state = 0;

int IN1 = 4; //Жёлтый провод на драйвере двигателя (чёрный провод двигателя)(от драйвера к ардуино черный)
int IN2 = 7; //Оранжевый провод на драйвере двигателя (Красный уровень двигателя)(от драйвера к ардуино красный)
int IN3 = 8; //Фиолетовый провод на драйвере двигателя (Красный уровень двигателя)(от драйвера к ардуино черный)
int IN4 = 10; //Тёмно-синий провод на драйвере двигателя (чёрный провод двигателя)(от драйвера к ардуино красный)

int s = 0;
//Переменные для управления сервоприводами
byte S1LeftRight = 90;
byte S2ForwardBack = 90;
byte S3ForwardBack = 90;
byte S4OpenClose = 75;

void setup() {
  Serial.begin(9600);         // start serial for output
  //инициализация пинов
  servo1.attach(3); //привязываем привод к порту 10
  servo2.attach(5); //привязываем привод к порту 10
  servo3.attach(6); //привязываем привод к порту 10
  servo4.attach(9); //привязываем привод к порту 10
  //Устанавливаем исходное положение
  servo1.write(90);
  delay(2000); //ждем 2 секунды
  servo2.write(90);
  delay(2000); //ждем 2 секунды
  servo3.write(90);
  delay(2000); //ждем 2 секунды
  servo4.write(75);
  delay(2000); //ждем 2 секунды
  //Инициализация моторов
  pinMode (IN1, OUTPUT);
  pinMode (IN2, OUTPUT);
  pinMode (IN3, OUTPUT);
  pinMode (IN4, OUTPUT);

  // initialize i2c as slave
  Wire.begin(SLAVE_ADDRESS);
  // define callbacks for i2c communication
  Wire.onReceive(receiveData);
  Wire.onRequest(sendData);
  Serial.println("Ready!");
}
void loop() {
  delay(100);
}
// callback for received data
void receiveData(int byteCount) {
  while (Wire.available()) {
    number = Wire.read();
    //Serial.print("data received: ");
    //Serial.println(number);
    //Моторы
    if (number == 8) {
      digitalWrite (IN1, HIGH);
      digitalWrite (IN2, LOW);
      digitalWrite (IN3, HIGH);
      digitalWrite (IN4, LOW);
      //Код для движения мотора
      //delay(2000);
    }
    else if (number == 5) {
      digitalWrite (IN1, LOW);
      digitalWrite (IN2, HIGH);
      digitalWrite (IN3, LOW);
      digitalWrite (IN4, HIGH);
    }
    else if (number == 6) {
      digitalWrite (IN1, LOW);
      digitalWrite (IN2, HIGH);
      digitalWrite (IN3, HIGH);
      digitalWrite (IN4, LOW);
    }
    else if (number == 4) {
      digitalWrite (IN1, HIGH);
      digitalWrite (IN2, LOW);
      digitalWrite (IN3, LOW);
      digitalWrite (IN4, HIGH);
    }
    //Сервоприводы
    //Узел № 3 сервопривод №3--в нутри узел 2-Вперёд-----------------------------
    else if (number == 119) { //w119 s115 a97 d100
      S3ForwardBack = S3ForwardBack + 5;
      if (S3ForwardBack >= 180) {
        S3ForwardBack = 180;
        servo3.write(S3ForwardBack);
        if (S2ForwardBack > 145) {
          S2ForwardBack = 145;
          servo2.write(S2ForwardBack);
        }
        else {
          S2ForwardBack = S2ForwardBack + 5;
          servo2.write(S2ForwardBack);
        }
      }
      else
        servo3.write(S3ForwardBack);
    }
    //Узел № 3 сервопривод №3--в нутри узел 2-назад-----------------------------
    else if (number == 115) { //w119 s115 a97 d100
      S2ForwardBack = S2ForwardBack - 5;
      if (S2ForwardBack <= 90) {
        S2ForwardBack = 90;
        servo2.write(S2ForwardBack);
        if (S3ForwardBack < 90) {
          S3ForwardBack = 90;
          servo3.write(S2ForwardBack);
        }
        else {
          S3ForwardBack = S3ForwardBack - 5;
          servo3.write(S3ForwardBack);
        }
      }
      else
        servo2.write(S2ForwardBack);
    }

    else if (number == 97) {
      S1LeftRight = S1LeftRight + 5;
      if (S1LeftRight < 0) {
        S1LeftRight = 10;
        servo1.write(S1LeftRight);
      }
      else if (S1LeftRight > 180) {
        S1LeftRight = 180;
        servo1.write(S1LeftRight);
      }
      else
        servo1.write(S1LeftRight);
    }
    else if (number == 100) {
      S1LeftRight = S1LeftRight - 5;
      if (S1LeftRight < 10) {
        S1LeftRight = 10;
        servo1.write(S1LeftRight);
      }
      else if (S1LeftRight > 180) {
        S1LeftRight = 170;
        servo1.write(S1LeftRight);
      }
      else
        servo1.write(S1LeftRight);
    }//-------------------------------------------
    else if (number == 113) {
      S4OpenClose = 75;
      servo4.write(S4OpenClose);
    }//-------------------------------------------
    else if (number == 101) {
      S4OpenClose = 5;
      servo4.write(S4OpenClose);
    }
    else {
      //Код для jcnfyjdrb мотора
      digitalWrite (IN1, LOW);
      digitalWrite (IN2, LOW);
      digitalWrite (IN3, LOW);
      digitalWrite (IN4, LOW);
    }
  }
}
// callback for sending data
void sendData() {
  Wire.write(number);
}
