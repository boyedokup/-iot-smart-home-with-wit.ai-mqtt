## Quick Guide to Creating a Voice Enabled App with Wit.Ai and MQTT

This Project has a simple app using Facebook's [Wit.Ai](https://wit.ai/) and[ Paho MQTT Client](https://www.hivemq.com/blog/mqtt-client-library-encyclopedia-paho-js/) to allow a user control the state of devices in their homes. 
Just like how Siri, Google Assistant and Cortana works, this app also takes voice commands and publishes it to an MQTT cloud broker

Before I walk you through the code let's have abit of fun, clone the repo and host it on any local server

- Clone this project
- Drop the project into any local server e.g XAMPP
- The entire app is in the /public directory, no need to compile  
- Follow the instruction when you launch the app in your browswer


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
  
  
  `dsdsd`
  

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/boyedokup/-iot-smart-home-with-wit.ai-mqtt/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
