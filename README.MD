This project was developed using an Arduino microcontroller, ultra sonic sensor, piezzo buzzer and LED lights in order 
to create a proximity alarm. How this alarm works is when the ultra sonic sensor senses an object at 3 different distances
the piezzo buzzer starts to make a sound and the led lights start to flicker. As the object comes nearer and closer
the LED lights change their pattern of flickering and the piezzo buzzer sounds becomes stronger. Since somone might
not have the Arduino IDE installed i provide my code in this readme file.







//led lights pins
int led1 = 8;
int led2 = 9;
int led3 = 7;
int led4 = 6;
int led5 = 5;
//pins for ultrasonic sensor
#include<NewTone.h>
const int trigPin = 3;
const int echoPin = 4;
//pin for buzzer
#define ALARM 13
float sinVal;
int toneVal;






void setup()
{
  // put your setup code here, to run once:
  //placing the pins
  Serial.begin(9600);
  pinMode(led1, OUTPUT);
  pinMode(led2, OUTPUT);
  pinMode(led3, OUTPUT);
  pinMode(led4, OUTPUT);
  pinMode(led5, OUTPUT);
  digitalWrite(led1, LOW);
  digitalWrite(led2, LOW);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
  digitalWrite(led5, LOW);
  pinMode(ALARM, OUTPUT);
  pinMode(trigPin, OUTPUT);

}

void loop()
{
  long duration, distanceCm;
  // put your main code here, to run repeatedly:

  // The sensor is triggered by a HIGH pulse of 10 or more microseconds.
  // Give a short LOW pulse beforehand to ensure a clean HIGH pulse:
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  // Read the signal from the sensor: a HIGH pulse whose
  // duration is the time (in microseconds) from the sending
  // of the ping to the reception of its echo off of an object.

  pinMode(echoPin, INPUT);
  duration = pulseIn(echoPin, HIGH);
  distanceCm = microsecondsToCentimeters(duration);
  Serial.println(distanceCm);
  if (distanceCm > 110)
  {
    alarm();
  }
  else if (distanceCm >= 80 && distanceCm <= 110) {
    alarm1();
    tone(ALARM, 1000, 100);
  }
  else if (distanceCm >= 40 && distanceCm <= 80 ) {
    alarm2();
    tone(ALARM, 5000, 200);
  }
  else if (distanceCm < 40) {
    alarm3();

    tone(ALARM, 200000, 400);
    
  }
  delay(600);
  noTone(ALARM);
}


void alarm1()
{
  digitalWrite(led1, HIGH);
  delay(100);
  digitalWrite(led1, LOW);

}
void alarm2()
{
  alarm1();
  digitalWrite(led2, HIGH);
  digitalWrite(led3, HIGH);
  delay(100);
  digitalWrite(led2, LOW);
  digitalWrite(led3, LOW);
}
void alarm3()
{
  alarm2();
  digitalWrite(led4, HIGH);
  digitalWrite(led5, HIGH);
  delay(100);
  digitalWrite(led4, LOW);
  digitalWrite(led5, LOW);

}

void alarm()
{
  //digitalWrite(led1,HIGH);
  //digitalWrite(led2,HIGH);
  //digitalWrite(led3,HIGH);
  //digitalWrite(led4,HIGH);
  //digitalWrite(led5,HIGH);
  delay(100);
  digitalWrite(led1, LOW);
  digitalWrite(led2, LOW);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
  digitalWrite(led5, LOW);
}


long microsecondsToCentimeters(long microseconds)
{
  // The speed of sound is 340 m/s or 29 microseconds per centimeter.
  // The ping travels out and back, so to find the distance of the
  // object we take half of the distance travelled.
  return (( microseconds / 29) / 2);
}