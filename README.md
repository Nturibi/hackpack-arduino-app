# Part I: Using sensors with Arduino 
## Arduino Digital/Analog pins 
## Types of sensors 
## Reading from sensors 
## Using data from sensors 
## Sensors available 

# Part II: Interfacing an Arduino with Desktop Applications 
## Building Python and Processing apps that talk to Arduinos (via USB)

This hackpack covers the process of writing simple Arduino programs that interact with a computer. This permits fairly effortless interaction between sensors and desktop applications, such as visualizations of sensor data, peripheral control of computer systems, or controlling robotics devices. 

We begin with a brief outline of serial communication and proceed to create a simple app that communicates from an Arduino to a desktop computer. Note that a similar procedure can be used to connect an Arduino to a Raspberry Pi. 

Serial communication through a UART port enables an Arduino to send messages to other devices, such as a desktop computer or another Arduino. Serial communication is a frequently used protocol to send data among devices; prior to USB, many computer peripherals were serial devices. Today, UART ports are frequently used to send data from an embedded device, such as an Arduino or Raspberry Pi, to sensors, including a GPS or LCD display. For more information on the mechanics of serial communication, consult: https://learn.sparkfun.com/tutorials/serial-communication. 

Clone this repository by running `git clone https://github.com/TreeHacks/hackpack-arduino-app.git` from your command line. It includes this readme and sample code. 

## Arduino: Using the Serial Monitor 

The serial monitor is perhaps the most important debugging tool for Arduino programs. See the "Hello, World" program as an example: 

```
// The setup function is required and runs a single time when the program starts
void setup() {
  Serial.begin(9600);
}

// The loop function loops infinitely after the setup function returns 
void loop() {
	Serial.println("Hello, world!"); 
	delay(1); // Delay one second 
}

```

To see "Hello, World" on your computer, open the Arduino application, choose the proper board (under tools -> board), and open the serial monitor. The example initializes serial communication with a speed of 9600 bits per second, so select 9600 after opening the serial monitor. 

This guide does not cover using more complex sensors and devices with an Arduino, but a general outline of sending this data to a computer follows: 

1) Collect the data in some variable 
2) Print the variable to the Serial port 
3) Receive it with a desktop application 

## Desktop: Writing apps to work with Arduino 
### Python 
To start, install pySerial by running `pip install pyserial` in your terminal window. 

Now, we will look at a basic program to read data from an Arduino: 

```
import serial 
ser = serial.Serial('/dev/tty.usbserial', 9600)

while True:
	print ser.readline()

```

Save this in a file "serial_demo.py". This python program creates a serial port and prints any data received over serial. If you connect your Arduino and, from the directory containing "serial_demo.py" type "python serial_demo.py", you should see "Hello, World" printed on your computer. More complex applications and two way communication can be implemented using `ser.write(val)` to send messages back to the Arduino.

### Processing 
Processing can be downloaded at https://processing.org/. With similar syntax to Java, it is designed for building fairly simple graphical applications. Thus, making two or three dimensional visualizations of Arduino data is simple in Processing. 

This guide will discuss a simple example in Processing for serial communication. 

```
Serial myPort;  // Create Serial object
String val;  	// For serial data

void setup() {
	// Serial.list() provides a list (of strings) of all open serial ports. 
	// You may have to change the 0 below to 1 or 2 depending on the port your 
	// Arduino is using. 
	println(Serial.list()); 
	String portName = Serial.list()[0]; // May change 0 to 1 or 2
	myPort = new Serial(this, portName, 9600);
	size(100, 100); 
}

void draw() {
  	if ( myPort.available() > 0 ) {  		// If data is available in the serial port, 
  		val = myPort.readStringUntil('\n'); // store it in val
  	} 
  	if ( val == "Hello, world" ) {
  		background(random(255), random(255), random(255)); // change the background color
  	}
	println(val); // print data to console 
}
```

Similar to Arduino, Processing sketches include a `setup` function, that runs once, and a `draw` function that runs repeatedly until the application quits. This program creates and opens a Serial port and prints any data received from the port to the console. 

### Other Languages 

Though Processing and python provide some of the simplest methods for interfacing an Arduino with a computer, many other languages, including Matlab and Java, provide serial communication libraries for interacting with an Arduino. 

## Reading and Writing 

It is possible to perform two way communication with the Arduino as well. Generally, it is good practice to perform some sort of handshake between the microcontroller and the computer to ensure both devices are sending messages when expected. This can be as simple as: 

```
void testContact() {
	// While there are no bytes incoming on the Serial 
	while (Serial.available() <= 0) {
		Serial.println("ABC"); 
	}
}
```

Note that the line `while (Serial.available() <= 0)` will be true while there are no bytes waiting for the Arduino to process. 

## Credit

https://learn.sparkfun.com/tutorials/connecting-arduino-to-processing for some Processing examples, http://playground.arduino.cc/Interfacing/Python for python examples. 

by Andrew 