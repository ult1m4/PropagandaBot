<div align="center">
<img src="static/image/logo.png" alt="propaganda" width="200px" />
<h1>Propaganda Bot</h1>
</div>

Propaganda Bot is a [Mumble](https://www.mumble.info/) music bot, forked from Botamusique.

Use basic commands such as !play youtube-url.

I plan on removing some functionalities to keep this as streamlined as possible: a simple YouTube/Media player bot with volume control that is easy to maintain.

Botamusique has deprecated, but props to @azlux for making this work. All I've done is update everwhere that youtube-dl was being used to use yt-dlp, and I disabled updating.

You will have to manually update yt-dlp with pip when you need it.

## Features

1. **Support multiple music sources:**
    - Music in local folders (which can be uploaded through the web interface).
    - Youtube/Soundcloud URLs and playlists (everything supported by yt-dlp).
    - Radio stations from URL and http://www.radio-browser.info API. (To be removed...)
2. **Modern and powerful web remote control interface.** Powered by Flask. Which supports:
    - Playlist management.
    - Music library management, including uploading, browsing all files and edit tags, etc.
3. **Powerful command system.** Commands and words the bot says are fully customizable. Support partial-match for commands. (To be made simpler...)
4. **Ducking.** The bot would automatically lower its volume if people are talking. (To be removed...)

-----
## Quick Start Guide
1. [Installation](#installation)
1. [Configuration](#configuration)
1. [Run the bot](#run-the-bot)
1. [Operate the bot](#operate-the-bot)
1. [Update](#update)
1. [Known issues](#known-issues)
1. [Contributors](#contributors)

### Installation

#### Dependencies
1. Install python3 and pip install -r requirements.txt

### Configuration
1. Use the default configuration file with IP/etc based on your use case.
   
#### Basic settings
1. Usually, the first thing is to set the Murmur server you'd like the bot to connect to. You may also specify which channel the bot stays, and tokens used by the bot.
```
[server]
host = 127.0.0.1
port = 64738
```

2. You need to specify a folder that stores your music files. The bot will look for music and upload files into that folder. You also need to specify a temporary folder to store music files download from URLs.
```
[bot]
music_folder = music_folder/
tmp_folder = /tmp/
```

3. **Web interface is disabled by default** for performance and security reasons. But it is extremely powerful, so we encourage you to have a try. To enable it, set
```
[webinterface]
enabled = True
```

Default binding address is
```
listening_addr = 127.0.0.1
listening_port = 8181
```

You can access the web interface through http://127.0.0.1:8181 if you keep it unchanged.

Note: Listening to address `127.0.0.1` will only accept requests from localhost. If you would like to connect from the public internet, you need to set it to `0.0.0.0`, and set up username and password to impose access control. In addition, if the bot is behind a router, you should also properly set forwarding rules in you NAT configuration to forward requests to the bot.

4. Generate a certificate (Optional, but recommended)

By default, murmur server uses certificates to identify users. Without a valid certificate, you wouldn't able to register the bot into your Murmur server. Some server even refused users without a certificate. Therefore, it is recommended to generate a certificate for the bot. If you have a certificate (for say, `botmusique.pem` in the folder of the bot), you can specify its location in
```
[server]
certificate=botamusique.pem
```

If you don't have a certificate, you may generate one by:
`openssl req -x509 -nodes -days 3650 -newkey rsa:2048 -keyout botamusique.pem -out botamusique.pem -subj "/CN=botamusique"`


#### Sections explained
- `server`: configuration about the server. Will be overridden by the `./mumbleBot.py` parameters.
- `bot`: basic configuration of the bot, eg. name, comment, folder, default volume, etc.
- `webinterface`: basic configuration about the web interface.
- `commands`: you can customize the command you want for each action (eg. put `help = helpme` , the bot will respond to `!helpme`)
- `radio`: a list of default radio (eg. play a jazz radio with the command `!radio jazz`)
- `strings`: you can customize all words the bot can say.
- `debug`: option to activate ffmpeg or pymumble debug. (Can be very verbose)


### Run the bot
If you have set up everything in your `configuration.ini`, you can
`venv/bin/python mumbleBot.py --config configuration.ini`

Or you can
`venv/bin/python mumbleBot.py -s HOST -u BOTNAME -P PASSWORD -p PORT -c CHANNEL -C /path/to/botamusique.pem`

If you want information about auto-starting and auto-restarting of the bot, you can check out the wiki page [Run propagandabot as a daemon in the background](https://github.com/azlux/botamusique/wiki/Run-botamusique-as-a-daemon-In-the-background).

**For the detailed manual of using propaganda, please see the [wiki](https://github.com/azlux/botamusique/wiki).**

### Operate the bot

You can control the bot by both commands sent by text message and the web interface.

By default, all commands start with `!`. You can type `!help` in the text message to see the full list of commands supported, or see the examples on the [wiki page](https://github.com/azlux/botamusique/wiki/Command-Help-and-Examples).

The web interface can be used if you'd like an intuitive way of interacting with the bot. Through it is fairly straightforward, a walk-through can be found on the [wiki page](https://github.com/azlux/botamusique/wiki/Web-interface-walk-through).

### Update

If you enable `audo_check_update`, the bot will check for updates every time it starts.
If you are using the recommended install, you can send `!update` to the bot (command by default).

If you are using git, you need to update manually:
```
git pull --all
git submodule update
venv/bin/pip install --upgrade -r requirements.txt
```


### Known issues

1. During installation, you may encounter the following error:
```
ImportError: libtiff.so.5: cannot open shared object file: No such file or directory
```
You need to install a missing library: `apt install libtiff5`

2. In the beginning, you may encounter the following error even if you have installed all requirements:
```
Exception: Could not find opus library. Make sure it is installed.
```
You need to install the opus codec (not embedded in all system): `apt install libopus0`

3. If you have a large amount of music files (>1000), it may take some time for the bot to boot, since
it will build up the cache for the music library on booting. You may want to disable this auto-scanning by
setting ``refresh_cache_on_startup=False`` in `[bot]` section and control the scanning manually by
``!rescan`` command and the *Rescan Files* button on the web interface.

4. Alpine Linux requires some extra dependencies during the installation (in order to compile Pillow):
```
python3-dev musl-lib libmagic jpeg-dev zlib-dev gcc
```
For more information, see [#122](https://github.com/azlux/botamusique/issues/122).


### Contributors
If you want to participate, you're welcome to fork and submit pull requests (fixes, refinement, good features).

Thank you to all the previous creators, collaborators, and maintainers, to which this bot wouldn't exist at all. I merely patched it. Big kudos:
- @azlux
- @TerryGeng
- @mertkutay
