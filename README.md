# Messenger library for arduino
Messenger is a "toolkit" that facilitates the parsing of ASCII messages. Messenger buffers characters until it receives a carriage return (CR) or newline (NL). It then considers the message complete and available. The message is split into many elements as defined by a separator. The default separator is the space character, but can be any character other than NULL, LF or CR.

Note: This library is created by Thomas Ouellet Fredericks this is simply a fork with some tiny modifications and put on github for easy editing but credits go to Thomas Ouellet Fredericks

## Install

To install the messenger library follow the steps below

* Clone or download the library
* Extract the library to your arduino install path

*On Mac OSX*  

```bash
cp Messenger /Applications/Arduino.app/Contents/Resources/libraries/

```


## Example

```c++
// This example sets all the values of the digital pins with a list through a callback function 

#include <Messenger.h>
// Instantiate Messenger object with the default separator (the space character)
Messenger message = Messenger(); 

// Create the callback function
void messageReady() {
    int pin = 0;
       // Loop through all the available elements of the message
       while ( message.available() ) {
	// Set the pin as determined by the message
         digitalWrite( pin, message.readInt() );
         pin=pin+1;
      }
}


void setup() {
  // Initiate Serial Communication
  Serial.begin(115200); 
  // Attach the callback function to the Messenger
  message.attach(messageReady);
}


void loop() {
  // The following line is the most effective way of using Serial and Messenger's callback
  while ( Serial.available() )  message.process(Serial.read () );
}
```