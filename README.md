# Autotelegram
![autotelegram](./docs/assets/autotelegram.png)

## Overview

Autotelegram is a Python library tha helps you build Telegram bots fast and effeciently.
It is a spin-off of the [autobot](https://github.com/OSCA-Kampala-Chapter/autobot) project and provides a Pythonic wrapper around the Telegram bot API to enable you make queries to your telegram bot directly from python code <br />

Autotelegram cares about developer productivity, and that's why it tries to abstract away the tedious
and laborious complex code needed to develop telegram bots by providing you a nice and clean API to 
help you quickly spin-up a telegram bot in minutes. <br />

### Installation
The assumption here is that you have python 3.6 or higher installed on your system and that you have installed `pip` on your system. <br />

To install autotelegram, run the following command in your command line.


```
pip install https://github.com/OSCA-Kampala-Chapter/autotelegram/archive/refs/tags/autotelegram-0.2.0-beta.zip
```

### Getting started 🚀
 <br />

Let's get started by building a simple echo bot.


###  🚨 Please note: 🚨 

- With the previous API, we had to do a lot of manual work using the context API. If you still want to use it, it's availabe for use. <br />
- We however recommend using the new API builds on top of the context API to give you a nicer and simpler interface for building bots. 

### Pre-requisites 📝
Before we can proceed building our bot, we need the following things.

- A telegram bot token.
- A telegram account.

To get a telegram bot token, you need to create a bot with telegram's bot father. You can get detailed instructions on how to do that [here](https://core.telegram.org/bots/features#creating-a-new-bot)

### Creating the bot 🤖

📌 Step 1: Import the `Context` and `PollingApp` classes.

```python
from autotelegram.telegram.context import Context
from autotelegram.telegram.application import PollingApp
``` 

📌 Step 2: Create an instance of the `Context` class and pass in the bot token.

```python
TOKEN = "token-for-the-bot"
ctx:Context = Context(TOKEN)
app = PollingApp(ctx)
```

📌 Step 3: Write the async function that will process every incoming update.

```python

async def echo (update,ctx):
    message = update.message
    text = message.text
    await message.respond_with_text(text)
```

📌 Step 4: Call the `run` method of the `PollingApp` class and pass in the async function we wrote in step 3.

```python
    
if __name__ == "__main__":
    app.run(echo)
```

Here's the entire code for the bot.

```python
from autotelegram.telegram.context import Context
from autotelegram.telegram.application import PollingApp

TOKEN = "token-for-the-bot"
ctx:Context = Context(TOKEN)
app = PollingApp(ctx)

async def echo (update,ctx):
    message = update.message
    text = message.text
    await message.respond_with_text(text)

if __name__ == "__main__":
    app.run(echo)
```

### Running the bot 🏃‍♂️

To run the bot, run the following command in your command line.

```
python echo_bot.py
```
