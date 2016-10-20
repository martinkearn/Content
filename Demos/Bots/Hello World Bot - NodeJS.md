# Hello World Bot (in NodeJS)
This demo creates a 'hello world' project using the Microsoft Bot Framework in NodeJS.

Based on https://docs.botframework.com/en-us/node/builder/overview/#navtitle

### Pre Reqs
* NPM installed
* Visual Studio Code installed
* The Bot Emulator installed from: https://docs.botframework.com/en-us/tools/bot-framework-emulator/
* Ensure previous porjects are removed

## Setup a project folder
Setup a folder to work from and install the necesary NPM packages

1. Open a command line and navigate to c:\demodump (or similar)

2. `MD NodeBot` to create a folder called 'NodeNot'

3. `CD NodeBot` to navigate to the directory

4. `npm init` to setup a porject.json
  * Accept all defaults
  
5. `npm install --save botbuilder` to install the BotBuilder SDK

6. `npm install --save restify` to install restify
  * Restify helps build REST apis on top of NodeJS
  
## Create App.js
Create the app

1. `code .` in the command prompt to open VS Code in the current location

2. Create a new file called `app.js`

3. Add the following code:
```
var restify = require('restify');
var builder = require('botbuilder');

// Setup Restify Server
var server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
   console.log('%s listening to %s', server.name, server.url); 
});
  
// Create chat bot
var connector = new builder.ChatConnector({
    appId: process.env.MICROSOFT_APP_ID,
    appPassword: process.env.MICROSOFT_APP_PASSWORD
});
var bot = new builder.UniversalBot(connector);
server.post('/api/messages', connector.listen());

bot.dialog('/', function (session) {
    session.send("Hello World");
});
```

## Run the bot and test in the emulator

1. `node app.js` in the command prompt to run the bot

2. Open the 'Microsoft Bot Framework Channel Emulator' and set the Bot Url `http://localhost:3978/api/messages`

3. Say "hello" in the emulator, await a repsonse and explore the JSON that gets sent back and forth
