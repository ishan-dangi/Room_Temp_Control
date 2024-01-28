# Room_Temp_Control
//Automatic room tempetature control system
//Here is a embedded c code for arduino to run a automatic room temperature control system

const int temp_pin=A1;
const int heat_pin=13;
const int fan=11;
float mintemp=20, maxtemp=25;
#include<LiquidCrystal.h>
LiquidCrystal LCD(10,9,5,4,3,2);
 
void setup(){
 LCD.begin(16,2);
 pinMode(heat_pin,OUTPUT);
 pinMode(fan,OUTPUT);
 LCD.print("Room Temp (c): ");
 LCD.setCursor(2,1);
 LCD.print(mintemp);LCD.print("-");LCD.print(maxtemp);
 delay(2000);
}
void loop(){
 float eqv_volt, sensorTemp;
 eqv_volt=analogRead(temp_pin)*5.0/1023;
 sensorTemp=100.0*eqv_volt-50.0;
 LCD.clear();
 LCD.print("sensor reading: ");
 LCD.setCursor(2,1);
 LCD.print(sensorTemp); LCD.print("C");
 delay(2000);
 if(sensorTemp>maxtemp){
 LCD.clear();
 LCD.print("tempe is high");
 LCD.setCursor(0,1); LCD.print("turn on fan");
 for (int i=0;i<=255;i++){
 analogWrite(fan, i);
 }
 delay(5000);
 LCD.clear();
 LCD.print("temp is normal");
 LCD.setCursor(0,1); LCD.print("tur off fan");
 for (int i=255;i>=0;i--){
 analogWrite(fan, i);
 }
 delay(2000);
 }
 else if(sensorTemp<mintemp){
 LCD.clear();
 LCD.print("temp is low");
 LCD.setCursor(0,1); LCD.print("turn on heater");
 digitalWrite(heat_pin,HIGH);
 delay(5000);
 
 LCD.clear();
 LCD.print("temp is normal");
 LCD.setCursor(0,1); LCD.print("turn off heater");
 digitalWrite(heat_pin,HIGH);
 delay(1000);
 LCD.clear();
 }
 else if(sensorTemp>mintemp && sensorTemp<maxtemp){
 LCD.clear();
 LCD.print("Temp is normal");
 LCD.setCursor(2,1);
 LCD.print("Turn off all");
 delay(1000);
 }
 else{
 LCD.clear();
LCD.print("something went wrong");
 delay(1000);
 LCD.clear();
 }
 delay(1000);
}
