# Messenger library for arduino
Messenger is a "toolkit" that facilitates the parsing of ASCII messages. Messenger buffers characters until it receives a carriage return (CR) or newline (NL). It then considers the message complete and available. The message is split into many elements as defined by a separator. The default separator is the space character, but can be any character other than NULL, LF or CR.

*Note:* This library is created by Thomas Ouellet Fredericks this is simply a fork with some tiny modifications and put on github for easy editing but credits go to Thomas Ouellet Fredericks

## Install

To install the messenger library follow the steps below

* Clone or download the library
* Extract the library to your arduino install path

*On Mac OSX*  

    /Applications/Arduino.app/Contents/Resources/libraries/
    
*On Windows*  

    C:\Program Files\Arduino\libraries\
Of course if your installed your Arduino IDE to a different location then that is where you should look for the libraries folder


## API

### Messenger(byte separator)
or
### Messenger()
You can create an instance of Messenger by specifying a message separator. If you do not specify a separator, the space character will be selected.
General Methods

### void attach(callbackFuntion)
Attaches a callback function that is executed once a message is completed. This is the preferred way of working with Messenger (see example at the bottom of this page).


### byte process(int serialData)
Returns true if a message has been completed and is available. Once a message is completed, it will be split into elements that you should be read immediately with read or string methods because every call to process() erases the completed message if there are any leftover elements.


### byte available()
returns true if there are any elements available in the completed message. You must make a call to process() before using available().
Read Methods

You must make a call to process() to complete a message before trying to read it's elements. Every time you use a read method, the read element (even if it is not of the proper type) is removed from the completed message and Messenger will point to the next one. Use available() to check if their are any elements left to read.
### int readInt()
Returns the element as an integer. Returns 0 if the element was not a number.

### long readLong()
Returns the element as an long. Returns 0 if the element was not a number.

### char readChar()
Returns the element as a character. If the character is part of a word, the whole word is removed from the completed message.
String Methods

### void copyString(char* target, byte maxSize)
Copies the element as a string into the the array pointed by the target char array. maxSize must match the size of the target char array. The element is removed from the completed message.


### byte checkString(char* toCheck)
Compares the element to the string toCheck.
If there is a match, the method returns true (1) and removes the element from the completed message.
If there is no match, the method returns false (0) and does not remove the element from the completed message.

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