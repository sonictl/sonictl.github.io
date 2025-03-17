---
layout: post
title:  "Node.js realtime communication with ESP8266 via socket.io"
date: 2022-01-05 22:19:54 +0800
categories: jekyll
slug: p20220105221954
---
# Node.js realtime communication with NodeMCU with socket.io

Reference: https://salfade.com/tutorials/realtime-communication-with-nodemcu-with-socketio

### Goal of this post

Establish a real-time communication between a server and the NodeMCU, this can be really useful like a situation where the server has to notify the client asynchronously,

### Let’s get started with the back-end

We are going to use a Node server, where we can integrate Socket.io init. [Install](https://nodejs.org/en/download/) Node.js if you haven’t it yet.

In node, we ‘ll be using the framework called [express](https://www.tutorialspoint.com/nodejs/nodejs_express_framework.htm) to deal with the communication with HTTP requests. Then integrate socket.io and start listening to events defined.

Create a new directory and create a file named as package.json where we specify the node packages that’s going to be used.

Add these dependencies to be installed

```
{
 "dependencies": {
  "express": "^4.17.1",
  "socket.io": "^2.2.0"
 }
}
```

Now install the dependencies

```
npm install or yarn install
```

It will create a folder named as node-modules inside your working directory containing a lot of files including those dependencies

### Configure the Node.js Server

Now create a file named as **index.js** ( can be in any name as you wish )

In **index.js** file, require express and HTTP and socket.io as listed below

```
// express framwork for a basic http server
var app = require('express')();
// create the http server
var http = require('http').createServer(app);
// require the socket.io and bind it to a port 
var io = require('socket.io')(3005);
```

Now attach the socket.io variable with the HTTP server created

```
io.attach(http, {
 pingInterval: 10000,
 pingTimeout: 5000,
 cookie: false
});
```

### Listen for Socket.io Events

Now we just need to listen to an event and log it for debugging. the timeout is just a function which emits an event called **reply** after each 5 seconds.

```
io.on('connection', function (socket) {

  console.log('user connected');

  socket.on('disconnect', function () {
    console.log('user disconnected');
  });
  socket.on('message', function (msg) {
   console.log("message: "+msg);
  });
  timeout();
});

function timeout() {
  setTimeout(function () {
   io.emit('reply',"A message from server");
    timeout();
  }, 5000);
}
```

Finally we just start listening on the HTTP server created By

```
http.listen();
```

this will start listening on a ephemeral port or you can specify a port itself,Now the Server is almost set up

### On the NodeMCU Side

Let’s try to deal with the NodeMCU module, for that we will be using the IDE provided by Arduino to make it easier.

Install the packages and the board details to Arduino IDE, I recommend you to follow this [link](https://www.instructables.com/id/Quick-Start-to-Nodemcu-ESP8266-on-Arduino-IDE/) if you have not set up the IDE to work with NodeMCU 

Now open Arduino IDE and go to tools 

- Select the proper board, in my case its NodeMCU 0.9 (ESP-12 module ),
- Set the upload speed, 11500 ( can vary according to the port and the board )
- Set the Port where the device has been connected

### Connect to wifi

We are set to start now,

First, we have to connect to a wifi network, for that, we are going to use a library called 

```
#include <ESP8266WiFi.h>
```

Go to tools -> manage libraries then , search for the ESP8266wifi and install it by clicking it. once it is installed we are ready to connect to the wifi network

```
void setup() {
 WiFi.begin("network-ssid", "password-to-network");
 Serial.print("Connecting");
 while (WiFi.status() != WL_CONNECTED)
 {
  delay(500);
  Serial.print(".");
 }
 Serial.println();

 Serial.print("Connected, IP address: ");
 Serial.println(WiFi.localIP());
}
```

Follow this [link](http://salphuric-nova.test/wink/posts/,https://arduino-esp8266.readthedocs.io/en/latest/esp8266wifi/readme.html) for more details on configuring the wifi, but this would be enough to work with.

### Install Socket.io Client to NodeMCU

Again go to manage library and search for, SocketIoClient And install it, Now we can use this library to connect to the server we have created.inlcude the headers as below

```
#include <WebSocketsClient.h>
#include <SocketIoClient.h>
```

Create a global variable called socketIO, so it can be accessed in any function.

```
SocketIoClient socketIO;
```

Then we need to connect to the server using the server address, in this case the ip address of my PC where it runs the node server.

Finally, listen for events from the server. And emit events if needed.

After connecting to wifi

```
  // server address, port and URL
  socketIO.begin("the ip address or domain", 3005);
  // event handler for the event message
  socketIO.on("reply",messageEventHandler);
```

Where messageEvent handler is ,

```
void messageEventHandler(const char * payload, size_t length) {
 Serial.printf("got message: %s\n", payload);
}
```

Now we should get the message printed in the serial monitor,

So to start the listening, we need to call the loop function of socket.io client inside the loop function in Arduino.

```
void loop() {
     socketIO.loop();
}
```

But when we want to emit something to the server, we can do it by calling the method call to emit where it requires to give the event name and the payload, for example, let’s emit something every 6 seconds.

```
uint64_t messageTimestamp;
void loop() {
  
  socketIO.loop();
  uint64_t now = millis();
  if(now - messageTimestamp > 6000) {
    messageTimestamp = now;
    // Send event     
  socketIO.emit("message", "\"this is a message from the client\"");   
  }    
}
```

