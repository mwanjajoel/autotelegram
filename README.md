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

```python
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

### Creating a bot 🤖

To understand how Autotelegram works, let us build a simple echo bot that echos back what the user sends to it. <br />

📌 Step 1: Import the `Context`class.

```python
from autotelegram.telegram.context import Context
``` 

📌 Step 2: Import `asyncio` and `time` 
Autotelegram is built on top os asyncio, so we'll need to import it in order to call some methods on the bot Context.
We shall also need the time package so as to tell the program to sleep for 10 seconds before making requests to the bot again.

```python
import asyncio
```

Our echo bot will get updates from the telegram API using the `get_updates` method of the `Context` class in Autotelegram since it's a wrapper with a Pythonic style to it <br />
Please note that `Telegram` provides 2 ways of getting updates i.e 

- Use of the `getUpdates` method 
- Use of a Webhook. 

Our focus for now is using the `getUpdates` method since we do not have a dedicated server for creating webhooks. <br />

📌 Step 3: Add the `TOKEN` received from the [BotFather](https://telegram.me/BotFather)

Telegram also provides us a url to our bot so that we can receive and send updates to it. The url is in the form of https://api.telegram.org/bot`<TOKEN>`/. TOKEN is the token string we've received from BotFather about the address of our bot. There is no space between bot and the Token. You don't have to worry about this as Autotelegram manages the URL for you. All you need to do is provide the token to the `Context`.

```python
TOKEN = "insert your bot token"
```

📌 Step 4: Write the function that echoes.

```python
    
async def main ():
    bot = Context(TOKEN)
    while True:
        updates = await bot.get_updates()
        for update in updates:
            if (msg := update.message):
                cid = msg.chat.id            # the ID of the chat that sent the message
                txt = msg.text               # the message which was sent
                await bot.send_message(chat_id = cid, text = txt)     # send the message back to the sender
        time.sleep(10)
```




📌 Step 5: Create the run function

```python
if __name__ == "__main__":
    asyncio.run(main())
```


Here's the entire code for the bot.

```python
from autotelegram.telegram.context import Context
import asyncio
import time

TOKEN = "your bot token"

async def main ():
    bot = Context(TOKEN)
    while True:
        updates = await bot.get_updates()
        for update in updates:
            if (msg := update.message):
                cid = msg.chat.id            # the ID of the chat that sent the message
                txt = msg.text               # the message which was sent
                await bot.send_message(chat_id = cid, text = txt)     # send the message back to the sender
                
        time.sleep(10)
        
if __name__ == "__main__":
    asyncio.run(main())
```

### Running the bot 🏃‍♂️

To run the bot, run the following command in your command line.

```python
python echo_bot.py
```

### Summary of the echo bot
- We created out bot context with the TOKEN. This is our interface for interacting with the telegram bot. <br />
- Autotelegram then immediately starts an infinte loop to run our program recuringly. We await on the async `get_updates` method of the bot context to receive updates. <br />
- Telegram provides us updates inform of a JSON Object. To make the response easier to interact with, Autotelegram parses the JSON into a tree of objects representing the different Telegram objects. (Telegram represents every element of the application such as message, sticker, voicenote, document, picture, url link and so many other elements as Telegram objects.)  <br />
- In our program, we're accessing the message object and reading its chat and text values. The Chat object represents the chat from which the message came from, and it has an `id` parameter which uniquely identifies this chat. The chat could be another user or group or channel.<br />
- After getting the `text` and `i`d of the chat we got the message from, we use the `send_message` method on the bot to send back a message to chat. we specify the `chat_id` to send the text to and `text` to send. <br />
- There are other optional parameters we can pass to `send_message` as documented in the telegram bot API docs. <br />
- The program will sleep for 10 seconds and the resume. To close the program, press CTRL-Z.