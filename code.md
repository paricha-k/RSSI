#include <TridentTD_LineNotify.h>
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>
#include <cmath> // pow

#define SSID "b" // ชื่อ wifi
#define PASSWORD "beAm0542" // password wifi
#define LINE_TOKEN "bJG9KTGeZ6EaHKDIb4vI7qMRophXVoIeQqB1kNmZnq6" // token line
#define BUZZER_PIN 2 //



char auth[] = "bN4ip0oAC9izqzh2zMVvEY5hfjELc-lU"; //token blynk


void setup() {
  {
    Serial.begin(115200);
    Serial.println(); 
  }
 
  
  WiFi.begin(SSID, PASSWORD);
  Serial.printf("WiFi connecting to %s\n", SSID);
  Blynk.begin(auth, SSID, PASSWORD);
  while (WiFi.status() !=WL_CONNECTED)
  {
    Serial.print(".");
    delay(1);
  }
  Serial.printf("\nWiFi connected\nIP : ");
  Serial.println(WiFi.localIP());

  LINE.setToken(LINE_TOKEN);
  pinMode(BUZZER_PIN,OUTPUT);
   
}


void loop(){

    int rssi = WiFi.RSSI(); //รับค่า rssi
    Serial.println(WiFi.RSSI());
    
       if (rssi < -70 && rssi!= 0) { 
       LINE.notify("คุณอยู่ไกลตลับยา...");
       delay(500);
       }
       
    Blynk.run();
    
}


BLYNK_WRITE(V1){
  int buttonState = param.asInt(); 
  int rssi = WiFi.RSSI(); // รับค่า rssi
  double distance = pow(10,(((double)((-50)-rssi))/20)); // สูตรการหา distance จาก rssi 

  if(buttonState == 1){
      if( -90 <= rssi && rssi < -70){
            digitalWrite(BUZZER_PIN,500);
            delay(100);
            digitalWrite(BUZZER_PIN,LOW);
            delay(100);
            digitalWrite(BUZZER_PIN,500);
            delay(100);
            digitalWrite(BUZZER_PIN,LOW);
            delay(100);
            digitalWrite(BUZZER_PIN,500);
            delay(100);
            digitalWrite(BUZZER_PIN,LOW);
            delay(100);
            digitalWrite(BUZZER_PIN,500);
            delay(100);
            digitalWrite(BUZZER_PIN,LOW);
            delay(100);
            digitalWrite(BUZZER_PIN,500);
            delay(100);
            digitalWrite(BUZZER_PIN,LOW);
            delay(100);

            LINE.notify("คุณอยู่ห่างจากตลับยาประมาณ ("+String(distance)+") เมตร");
            delay(500);
            
            }
             
          else if(-70 <= rssi && rssi < -50){
            digitalWrite(BUZZER_PIN,500);
            delay(300);
            digitalWrite(BUZZER_PIN,LOW);
            delay(300);
            digitalWrite(BUZZER_PIN,500);
            delay(300);
            digitalWrite(BUZZER_PIN,LOW);
            delay(300);
            
            LINE.notify("คุณอยู่ห่างจากตลับยาประมาณ ("+String(distance)+") เมตร");
            delay(500);
            }
            
          else{
            digitalWrite(BUZZER_PIN,500);
            delay(600);
            digitalWrite(BUZZER_PIN,LOW);
            delay(600);
            digitalWrite(BUZZER_PIN,500);
            delay(600);
            digitalWrite(BUZZER_PIN,LOW);
            delay(600);
            digitalWrite(BUZZER_PIN,500);
            delay(600);
            digitalWrite(BUZZER_PIN,LOW);
            delay(600);

            LINE.notify("คุณอยู่ห่างจากตลับยาประมาณ ("+String(distance)+") เมตร");
            delay(500);
            }
      }
      
      else{
        digitalWrite(BUZZER_PIN,LOW);
        delay(500);
        }

}
