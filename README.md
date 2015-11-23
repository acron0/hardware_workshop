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

![image arduino](https://raw.github.com/Eleonore9/hardware_workshop/master/img/Arduino.jpg)

It is usually programmed using the Arduino programming language on the Arduino IDE. But it can also be programmed using C++ or C.[[Read more]](https://gist.github.com/baalexander/8530398)

Arduino makes it accessible to programme microcontrollers. It is a good way to get started with robotics, experiment, or build prototypes.


## Install

To avoid installation issue (aaah, software!) we'll use the CodeBender platform.
You can create an account [here](https://codebender.cc/register/)!
CodeBender is a cross-platform web based editor for microcontroller boards. It helps you write, compile and upload code to your board.
![image codebender](https://raw.github.com/Eleonore9/hardware_workshop/master/img/codebender.png)


## Code examples
Note: for more examples see [github.com/Eleonore9/Arduino_hacks](https://github.com/Eleonore9/Arduino_hacks)

### Light two LEDs

[On codebender](https://codebender.cc/sketch:189141)

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

[On codebender](https://codebender.cc/sketch:189145)

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


### Light intensity sensor
[On codebender](https://codebender.cc/sketch:189156)

```
//Sketch for a light dependent resistor (LDR) module

//Global variables
int LDRValue; //LDR value
int light_sensitivity = 40; //Thresold value
int light_sensitivity2 = 10; //Thresold value
float Rsensor; //Resistance of sensor

void setup()
{
    Serial.begin(9600); //Starts the Serial connection
    pinMode(13, OUTPUT); //Using the LED at the pin 13
}

void loop()
{
    LDRValue = analogRead(0);//Reads LDR values at A0
    //Serial.println(LDRValue);
    Rsensor = (float) (1023 - LDRValue) * 10 / LDRValue;
    //Serial.println(Rsensor, DEC); 
    delay(4000); //Sets the speed by which LDR send a value to Arduino
    if (Rsensor < light_sensitivity)
    {
        if (Rsensor < light_sensitivity2)
        {
            Serial.print(Rsensor);
            Serial.println(" - The light is super bright!");
        }
        else
        {
        digitalWrite(13, HIGH);
        Serial.print(Rsensor);
        Serial.println(" - The light is on");
        }
    }
    else
    {
        digitalWrite(13, LOW);
        Serial.print(Rsensor);
        Serial.println(" - The light is off");
    }
}
```


### Light a LED strip
[On codebender](https://codebender.cc/sketch:189164)

```
    // From https://learn.adafruit.com/neopixel-painter/test-neopixel-strip
    
    // Simple NeoPixel test.  Lights just a few pixels at a time so a
    // 1m strip can safely be powered from Arduino 5V pin.  Arduino
    // may nonetheless hiccup when LEDs are first connected and not
    // accept code.  So upload code first, unplug USB, connect pixels
    // to GND FIRST, then +5V and digital pin 6, then re-plug USB.
    // A working strip will show a few pixels moving down the line,
    // cycling between red, green and blue.  If you get no response,
    // might be connected to wrong end of strip (the end wires, if
    // any, are no indication -- look instead for the data direction
    // arrows printed on the strip).
     
    #include <Adafruit_NeoPixel.h>
     
    #define PIN      6
    #define N_LEDS 144
     
    Adafruit_NeoPixel strip = Adafruit_NeoPixel(N_LEDS, PIN, NEO_GRB + NEO_KHZ800);
     
    void setup() {
      strip.begin();
    }
     
    void loop() {
      chase(strip.Color(255, 0, 0)); // Red
      chase(strip.Color(0, 255, 0)); // Green
      chase(strip.Color(0, 0, 255)); // Blue
    }
     
    static void chase(uint32_t c) {
      for(uint16_t i=0; i<strip.numPixels()+4; i++) {
          strip.setPixelColor(i  , c); // Draw new pixel
          strip.setPixelColor(i-4, 0); // Erase pixel a few steps back
          strip.show();
          delay(25);
      }
    }
```
