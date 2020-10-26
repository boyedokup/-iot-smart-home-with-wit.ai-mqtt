## Quick Guide to Creating a Voice Enabled App with Wit.Ai and MQTT

![iot-smart-home](https://snipboard.io/l9QVPj.jpg)

This Project has a simple app using Facebook's [Wit.Ai](https://wit.ai/) and[ Paho MQTT Client](https://www.hivemq.com/blog/mqtt-client-library-encyclopedia-paho-js/) to allow a user control the state of devices in their homes. 
Just like how Siri, Google Assistant and Cortana works, this app also processes the voice commands through Wit.ai and publishes it to an MQTT cloud broker, then the home device listens.

Before I walk you through the code let's have abit of fun, clone the repo and host it on any local server

- Clone this project
- Drop the project into any local server e.g XAMPP
- The entire app is in the /public directory, no need to compile  
- Follow the instruction when you launch the app in your browser


 I hope you had some fun, now lets dive into the components of this app and how they all fit together.

### Wit.AI

Wit.Ai is a Natural Language Processing tool (NLP) that takes voice or text phrases and makes meaning out of them. Simply put we can give it audio and we
we get strcutured text in return (Speech-to-Text). Wit.ai has many uses depending on the type of project. I will strongly recommend this [tutorial link](https://wit.ai/docs/quickstart) to grasp the concepts better

Let me explain some key concepts you will need to train your model better with Wit.Ai.

- **Intent**
  - Intent as the word depicts is the intention for the voice command. For example we want to be able to get the device name, the status - On/Off and
  even the temperature. So we assign these intents to Intent variables in Wit.AI. My model set the intents to set_device, on_off and set_temperature. 
  You can name them they way you want, it should just depict your intent.
  
- **Entity**
  - Entities return structured text responses to your intents. For example the  get_temperature/set_temperature intent will return values of the temperature 
  from the voice command. When the user says set temperature to 10 degrees , the entity object will include the figure and it's measurement
  
- **Train and Validate**
  - As we know Wit.ai's models are trained just like Machine Learning models. The more the training the better the accuracy.
  
- **Pre-built Entities and Traits**
  - Wit.Ai has pre-built entities and traits you can use. For this project I used the Temperature Entity and On_Off trait. You can check for pre-built options
  from [here](https://wit.ai/docs/built-in-entities/20200513/)
  
- **Custom Entities **
  - There are no pre-built entities for spotting devices so I had to create a custom entity. It is very easy. I created an intent- device to track the device names like tv,
  fridge, ac, heater etc. I created a custom entity using the [keyword and free-text](https://wit.ai/docs/recipes) option. I have attached a screenshot.
  
  ![Wit.ai screenshot](https://snipboard.io/viVJ1t.jpg)
  
### Paho MQTT Client
 
 MQTT is a a lightweight data protocol for streaming tele-data with the least over-head cost. This means the protocal allows for realtime data streams across networks.
 Many IOT devices like Arduino can communicate with the MQTT protocol. [Paho MQTT Client](https://www.hivemq.com/mqtt-client-library-encyclopedia) provides libraries for many   frameworks to communicate over this protocol. We will use the [JS library](https://www.hivemq.com/blog/mqtt-client-library-encyclopedia-paho-js/) for this web app. 
 I will strongly recommend this [tutorial link](https://www.hivemq.com/mqtt-essentials/) to grasp the concepts better

There are two main concepts to understanding the MQTT protocol

- **Publish**
  - Data is published to an MQTT cloud broker like [HiveMQ](https://www.hivemq.com/mqtt-protocol/) and [Mosquito](https://test.mosquitto.org/) through Topics/Channels
  
- **Subscribe**
  - Devices or client apps subscribe to Topics to listen for any streams
  

# [Wit.AI Microphone Code Snippet](https://github.com/wit-ai/microphone/blob/master/quickstart.md)
```markdown

<html>
  <head>
    <link rel="stylesheet" href="microphone/css/microphone.min.css">
  </head>
  <body style="text-align: center;">
    <center><div id="microphone"></div></center>
    <pre id="result"></pre>
    <div id="info"></div>
    <div id="error"></div>
    <script src="microphone/js/microphone.min.js"></script>

    <script>
      var mic = new Wit.Microphone(document.getElementById("microphone"));
      var info = function (msg) {
        document.getElementById("info").innerHTML = msg;
      };
      var error = function (msg) {
        document.getElementById("error").innerHTML = msg;
      };
      mic.onready = function () {
        info("Microphone is ready to record");
      };
      mic.onaudiostart = function () {
        info("Recording started");
        error("");
      };
      mic.onaudioend = function () {
        info("Recording stopped, processing started");
      };
      mic.onresult = function (intent, entities) {
        var r = kv("intent", intent);

        for (var k in entities) {
          var e = entities[k];

          if (!(e instanceof Array)) {
            r += kv(k, e.value);
          } else {
            for (var i = 0; i < e.length; i++) {
              r += kv(k, e[i].value);
            }
          }
        }

        document.getElementById("result").innerHTML = r;
      };
      mic.onerror = function (err) {
        error("Error: " + err);
      };
      mic.onconnecting = function () {
        info("Microphone is connecting");
      };
      mic.ondisconnected = function () {
        info("Microphone is not connected");
      };

      mic.connect("CLIENT_TOKEN");
      // mic.start();
      // mic.stop();

      function kv (k, v) {
        if (toString.call(v) !== "[object String]") {
          v = JSON.stringify(v);
        }
        return k + "=" + v + "\n";
      }
    </script>
  </body>
  </html>

```
# [Paho MQTT Code Snippet](https://www.hivemq.com/blog/mqtt-client-library-encyclopedia-paho-js/)

```markdown
// Create a client instance
client = new Paho.MQTT.Client(location.hostname, Number(location.port), "clientId");

// set callback handlers
client.onConnectionLost = onConnectionLost;
client.onMessageArrived = onMessageArrived;

// connect the client
client.connect({onSuccess:onConnect});

// called when the client connects
function onConnect() {
  // Once a connection has been made, make a subscription and send a message.
  console.log("onConnect");
  client.subscribe("/World");
  message = new Paho.MQTT.Message("Hello");
  message.destinationName = "/World";
  client.send(message); 
}

// called when the client loses its connection
function onConnectionLost(responseObject) {
  if (responseObject.errorCode !== 0) {
    console.log("onConnectionLost:"+responseObject.errorMessage);
  }
}

// called when a message arrives
function onMessageArrived(message) {
  console.log("onMessageArrived:"+message.payloadString);
}

````


### Support or Contact

Having trouble, create issue or email me on boyedokup@gmail.com
