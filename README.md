__DEPRECATED:__ _Please note that this project has been replaced by [aioTelegramBot](https://github.com/sourcesimian/aioTelegramBot) based on [Python 3 asyncio](https://docs.python.org/3/library/asyncio.html)._

---
# Telegram Bot Service (unofficial) in Twisted Python 3

A customisable bot for the [Telegram Bot API](https://core.telegram.org/bots/api) written in [Twisted](https://twistedmatrix.com) Python 3

Uses:
* [TelegramBotAPI](https://github.com/sourcesimian/pyTelegramBotAPI): An implementation of the Telegram Bot API messages and some simple clients.
* [pyPlugin](https://github.com/sourcesimian/pyPlugin): Simple framework-less plugin loader for Python.

## Installation
```
pip3 install txTelegramBot
```

## Usage
* Create a ```config.ini``` as follows, and set it up with your
[Telegram Bot API token](https://core.telegram.org/bots/api#authorizing-your-bot), etc:
    ```
    [telegrambot]
    # Your Telegram Bot API token
    token = 123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11

    [proxy]
    # address = www.example.com:8080

    [message_plugins]
    1 = plugins/*.py
    # 2 = other/plugins/foo.py

    [env]
    # Set any additional environment variables your plugins may need
    BOT_NAME = txTelegramBot
    # FOO = bar
    ```

* Write a plugin using the following template and add it in the \[message_plugins\] section above:
    ```
    from twisted.internet import defer
    from TelegramBotAPI.types import sendMessage
    from TelegramBot.plugin.message import MessagePlugin


    class MyPlugin(MessagePlugin):
        def startPlugin(self):
            pass

        def stopPlugin(self):
            pass

        @defer.inlineCallbacks
        def on_message(self, msg):
            m = sendMessage()
            m.chat_id = msg.chat.id
            m.text = 'You said: %s' % msg.text
            rsp = yield self.send_method(m)
            defer.returnValue(True)
    ```

* Run the bot:
    ```
    $ twistd -n telegrambot -c ./config.ini
    ```
