//RelayClass.ion

#include "RelayClass.h"

Relay blueRelay;

int pin = 4;
float relayInterval = 5;
bool relayIsInInterval = false;
float relayStartTime;
bool value = true;

void setup() {
  Serial.begin(9600);
  blueRelay.attach(pin);

}

void loop() {

}

//RelayClass.cpp
#include "esp32-hal.h"
#include "esp32-hal-gpio.h"
#include "RelayClass.h"
#include "Arduino.h"

Relay::Relay(){
  
}

Relay::Relay(int relayPin)
{
  attach(relayPin);
}

void Relay::attach(int relayPin)
{
  pinMode(relayPin, OUTPUT);
}


void Relay::relayToggleOn(int relayPin)
{
  digitalWrite(relayPin, HIGH);
}

void Relay::relayToggleOff(int relayPin)
{
  digitalWrite(relayPin, LOW);
}

float Relay::MillisecondsToSeconds(int milliseconds) 
{
  float second = float(milliseconds) / 1000;
  return second;
}

bool Relay::IsTimePassedFromInterval(float startTimeInSeconds, float intervalInSeconds)
{
  float endTimeSeconds = startTimeInSeconds + intervalInSeconds;
  unsigned long currentMillis = millis();
  float currentSeconds = MillisecondsToSeconds(currentMillis);

  if(currentSeconds < endTimeSeconds)
  {
    return true;
  }
  else
  {
    return false;
  }
}

void Relay::relayReverse(bool relayValue, int relayPin)
{
  digitalWrite(relayPin, !relayValue);
  relayValue = !relayValue;  
}

int Relay::relayGetState(bool relayState)
{
  if(relayState == 1)
  {
    return 1;
  }
  return 0;
}

void Relay::relayToggleSwitch(int relayArray[], int relayArraySize, int relayFrequency, int relayPin) 
{
  for (int i = 0; i < relayArraySize; i++) {
    if (relayArray[i] == 1) 
    {
      relayToggleOff(relayPin);
    } 
    else if(relayArray[i] == 0)
    {
      relayToggleOn(relayPin);
    }
    else {}
    delay(relayFrequency); 
  }
}

void Relay::relayTurnOff(int relayPin)
{
  for(int i = 0; i < 180; i++)
  {
    digitalWrite(relayPin, HIGH);
  }
}
void Relay::relayTurnOn(int relayPin)
{
  for(int i = 180; i < 0; i--)
  {
    digitalWrite(relayPin, LOW);
  }
}

//RelayClass.h

#include "soc/soc_caps.h"
#ifndef RelayClass_h
#define RelayClass_h

#include "Arduino.h"

class Relay
{
  public:
    bool relayState;
    Relay();
    Relay(int relayPin);
    void attach(int relayPin);
    void relayToggleOn(int relayPin);
    void relayToggleOff(int relayPin);
    bool IsTimePassedFromInterval(float startTimeInSeconds, float intervalInSeconds);
    float MillisecondsToSeconds(int milliseconds);
    void relayReverse(bool relayValue, int relayPin);
    int relayGetState(bool relayState);
    void relayTurnOff(int relayPin);
    void relayTurnOn(int relayPin);
    void relayToggleSwitch(int relayArray[], int relayArraySize, int relayFrequency, int relayPin);
};

#endif 
