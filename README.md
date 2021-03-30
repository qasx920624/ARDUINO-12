# ARDUINO-12
這次的課程是<p>
<DHT sensor><p>
也就是所謂的溫度濕度感應器<p>
而題目是如果溫度感應超過27°C<p>
就控制風扇開始轉動<p>
反之則使風扇停下<p>
程式如下
```c++
#include <Adafruit_Sensor.h>
#include <DHT.h>
#include <DHT_U.h>

#define DHTPIN 2     

#define DHTTYPE    DHT22     

DHT_Unified dht(DHTPIN, DHTTYPE);
uint32_t delayMS;
void setup() {
  Serial.begin(9600);
  dht.begin();
  sensor_t sensor;
  delayMS = sensor.min_delay /1000;
}
double a;
void motor(int xx)
{
  if(xx==0)
  {
    analogWrite(5,150);
    analogWrite(6,0);
  }
  
  if (xx==1)
  {
    analogWrite(5,0);
    analogWrite(6,0);
  }
}
void loop() {
  delay(delayMS);
  sensors_event_t event;
  dht.temperature().getEvent(&event);
  if (isnan(event.temperature)) {
    Serial.println(F("Error reading temperature!"));
  }
  else {
    Serial.print(F("Temperature: "));
    Serial.print(event.temperature);
    Serial.println(F("°C"));
    a = event.temperature;
  }
  dht.humidity().getEvent(&event);
  if (isnan(event.relative_humidity)) {
    Serial.println(F("Error reading humidity!"));
  }
  else {
    Serial.print(F("Humidity: "));
    Serial.print(event.relative_humidity);
    Serial.println(F("%"));
  }
  if(a>=27)
  {
    motor(0);
    tone(3,500);
    delay(500);
    noTone(3);
    delay(500);
  }
  else {motor(1);}
}
```
