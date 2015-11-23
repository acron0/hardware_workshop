#Hardware workshop

Aim: To have fun with microcontroller boards like Arduino!
From lighting up a LED to getting measurements from a sensor.


## All devices available
* [Espruino](http://www.espruino.com/EspruinoBoard)
* [Arduino Uno](https://www.arduino.cc/en/Main/ArduinoBoardUno)
* [WiFi shield](https://www.arduino.cc/en/Main/ArduinoWiFiShield) for Arduino Uno 
* [Arduino Nano clone](https://ashgillman.github.io/connecting-with-arduino-nano-clone-with-ch340g-baite/)
* Adafruit NeoPixel digital [LED strip](https://www.adafruit.com/products/1460)
* [Temperature sensor](http://linksprite.com/wiki/index.php5?title=Thermal_Module)
* [Light intensity sensor](http://linksprite.com/wiki/index.php5?title=LDR_Module)
* a piezo (used to play tunes or detect vibration)
* green and red LEDs
* a Servo motor
* a [Leap motion device](http://leapmotion.com/)


## Intro

Arduino is an open-source prototyping platform firstly aimed at the general public. [[Read more]](https://www.arduino.cc/en/Guide/Introduction)

![image arduino](https://raw.github.com/Eleonore9/hardware_workshop/master/img/arduino.png)

It is usually programmed using the Arduino programming language on the Arduino IDE. But it can also be programmed using C++ or C.[[Read more]](https://gist.github.com/baalexander/8530398)

Arduino makes it accessible to programme microcontrollers. It is a good way to get started with robotics, experiment, or build prototypes.


## Install

To avoid installation issue (aaah, software!) we'll use the CodeBender platform.
You can create an account [here](https://codebender.cc/register/)!
CodeBender is a cross-platform web based editor for microcontroller boards. It helps you write, compile and upload code to your board.
![image codebender](https://raw.github.com/Eleonore9/hardware_workshop/master/img/codebender.png)


### Light two LEDs

[Code on cc](https://codebender.cc/sketch:189141)

```
#define LED_PIN12 12
#define LED_PIN13 13

// I just alternate lighting two LEDs!
void setup()
{
    pinMode(LED_PIN12, OUTPUT);
    pinMode(LED_PIN13, OUTPUT);
}

void loop()
{
    digitalWrite(LED_PIN12, HIGH);
    delay(100);
    digitalWrite(LED_PIN12, LOW);
    delay(900);
    digitalWrite(LED_PIN13, HIGH);
    delay(100);
    digitalWrite(LED_PIN13, LOW);
    delay(900);	
}
```
	
### Temperature sensor

[Code on cc](https://codebender.cc/sketch:189145)

```
#include <Time.h> 

int sensorpin = 0; //Pin for the temperature sensor
 
void setup()
{
  Serial.begin(9600);  //Start the serial connection with the computer
                       //to view the result open the serial monitor 
}
 
void loop() // run over and over again
{
    //getting the voltage reading from the temperature sensor

    int reading = analogRead(sensorpin); 
     
    // converting that reading to voltage, for 3.3v arduino use 3.3
    float voltage = reading * 5.0;
    voltage /= 1024.0; 
     
    //print out the voltage
    //Serial.print(voltage); 
    //Serial.println(" volts");
     
    // now print out the temperature
    float temperatureC = (voltage - 0.5) * 100 ; //converting from 10 mv per degree wit 500 mV offset
                                                 //to degrees ((voltage - 500mV) times 100)
    //Serial.print(hour());
    //printDigits(minute());
    //printDigits(second());
    //Serial.print(",");
    Serial.print(temperatureC); 
    Serial.println(" degrees C");
     
    // now convert to Fahrenheight
    //float temperatureF = (temperatureC * 9.0 / 5.0) + 32.0;
    //Serial.print(temperatureF); 
    //Serial.println(" degrees F");
     
    delay(10000); //waiting a second
}
```
