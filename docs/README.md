# Homework Help Bot Docs

The Homework Help Bot, aka HWH Bot was built to automate mundane, daily functions within the 
Homework Help server such as changing roles, logging actions and prettifying messages. It was built
using the Discord.js library.

## Requirements

1. Node.js >= 14
2. Discord.js >= 12
3. npm or yarn

## Installing

If its functionality mirrors your server and you'd like to use it, or if you just want to learn
from it, here's how you install it:

```sh
# Clone the repository
git clone https://github.com/spjy/hwh-bot.git

# Change directories into the repo root 
cd hwh-bot

# Install dependencies via npm
npm install

# Or install dependecies via yarn
yarn install
```

Finally, set up your environment variables; copy and paste `.env.schema` and
fill at least `DISCORD_TOKEN`; the others are optional.

## Running

To run the bot, simply type:

```sh
# Run the bot via npm
npm start

# Or run the bot via yarn
yarn start
```

## Directory Structure

The bot is event based, and naturally the file structure represents that. The `/src` folder
contains all of the source code of the bot. Each Discord.js defined event that HWH Bot uses
is separated into their respective folders, namely in `/src/events`, and related scripts are 
contained in the event's folder. For example, message based events are contained within 
`/src/events/message` and its respective scripts are inside that event's folder.

```
│   .env
│   .env.schema
│   .eslintrc.json
│   .gitattributes
│   .gitignore
│   Dockerfile
│   LICENSE
│   package.json
│   Procfile
│   README.md
│   yarn.lock
│
└───src
    │   index.js
    │   
    └───events
        │   index.js
        │   
        ├───guildBanAdd
        │       index.js
        │       log.js
        │       
        ├───guildBanRemove
        │       index.js
        │       log.js
        │       
        ├───guildMemberAdd
        │       index.js
        │       log.js
        │       
        ├───guildMemberRemove
        │       index.js
        │       log.js
        │       
        ├───message
        │       dialogflow.js
        │       dm.js
        │       index.js
        │       mention.js
        │       mentionable.js
        │       report.js
        │       role.js
        │       rules.js
        │       suggestRole.js
        │       tipa5.js
        │       tips.js
        │       warning.js
        │       
        └───messageReactionAdd
                index.js
                suggestRole.js
```

### Main `/src/index.js` file

`/src/index.js` is the final piece of the puzzle to mend each component together. In here,
we run Discord.js, define constants and instantiate events.

Constants are values that remain unchanged and increase readability. For instance, we define
user, role and channel IDs (which remain constant) and increase readability in the code below.

Events are defined based on the [Discord.js defined client events](https://discord.js.org/#/docs/main/stable/class/Client). You should only import relevent scripts
within each respective event function. For example, I would not use a `messageReactionAdd` script
inside a `message` event.


### Event Folders

As mentioned before, event folders are inside `/src/events` and are named based on the Discord.js
defined events. Scripts within each event folder are automatically imported by the `index.js`
file and stored into a [Discord.js Collection](https://discord.js.org/#/docs/main/stable/class/Collection) and are available for use only inside the `/src/index.js` file. You can access the event by running:

```javascript
client.events
  .get('eventName::scriptName')
  .execute();
```

Each script within the event should be modularized per command. For example, if a report command
should be in a separate script from a role change command.

The file name of the script should be camel cased and reflect what the script does in some way.
For example if the script allows a user to suggest a role, the file would be called `suggestRole.js`
Additionally, a general template should be followed:

**Commented Template**
```javascript
// Import Raven for error handling
const Raven = require('raven');

// Export statement
module.exports = {
  description: '', // Brief description of script
  async execute() { // Function containing logic
    try {
      // Script contents
    } catch (err) {
      Raven.captureException(err); // Send errors to Raven
    }
  }
};
```

**Blank Template**
```javascript
const Raven = require('raven');

module.exports = {
  description: '',
  async execute() {
    try {

    } catch (err) {
      Raven.captureException(err);
    }
  }
};
```
