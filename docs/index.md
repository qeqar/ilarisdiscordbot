# Home

Ein Assistent zum Würfeln und Nachschlagen für alle die Ilaris über Discord spielen. 

## Features

- Für Ilaris optimierte Würfelfunktionen
- Gruppenorgansisation für größere Server/Events
- Nachschlagen im Regelbuch
- Anzeigen von Manöverkarten
- Zugriff auf Kreaturen (ilaris-online.de)

# Hosting

If you want to host this bot, please be aware that it is work in progress and
so is this guide. You don't need programming experience, but run a few
commands on the terminal. For linux/ubuntu you might get along with copy/paste.
Just raise an issue if you need more guidance.

### Get a bot token
First you need to acquire a bot token from discord. This token is basically the
login credential for your bot. Don't share it with anyone. If you don't know
how to get one: [Follow this guide](https://discordpy.readthedocs.io/en/stable/discord.html)

### Install from Source:
Clone the repository:
```sh
git clone git@github.com:XaverStiensmeier/ilarisdiscordbot.git
cd ilarisdiscordbot
```

Create and activate a virtual environment
(change the path if you like)
```sh
python3 -m venv ~/.venvs/discordbot
source ~/.venvs/discordbot/bin/activate
```

and install dependencies
```sh
pip install -r requirements.txt
```

save your token in a file (to be used by the bot)
```sh
mkdir data
echo YOUR_TOKEN >> data/token
```

#### Generate missing resources
Some additional ressources are required, that are not part of the repository
(yet). This include image collecionts of 
[Gatsu's Manöverkartenplugin](https://dsaforum.de/viewtopic.php?p=2002977#p2002979)
and all pages of the ilaris rule book: 

1. `resources/manoeverkarten/`
2. `resources/ilaris/` contains the core rules (`ilaris-001.png`, ..., `ilaris-219.png`)

To generate image files from downloaded pdfs you can use `poppler-utils`:
```sh
sudo apt install poppler-utils
pdftoppm -png FileIn.pdf outName
```
[Need more info on converting PDFs to images?](https://www.tecmint.com/convert-pdf-to-image-in-linux-commandline/)

### Run the bot
Make sure you are in the bots project folder and the environment is sourced 
(can be skipped if you just finished the installation)
```sh
cd ~/ilarisbot
source ~/.venvs/discordbot/
```
and start the actual bot with:
```sh
python ilaris_bot.py
```
As long as `ilaris_bot` is running, your ilarisdiscordbot instance is up. Enjoy.
Check the commandline or `data/discord.log` file if anything goes wrong.

### Keep the bot running
You might want to let the bot run forever on a server or remote machine.
Here tools like `screen` or `tmux` can be handy to create virtual terminals
that can be detached and run in the background. You can also compile a Docker
image and run it as a service (-d starts it in the background) that restarts 
automatically on crash or reboot:
```sh
docker compose build .
docker compose up -d
```
you can stop the bot with 
```sh
docker compose down
```

## How to dev
Contribute code by creating a PR to the dev branch. Reviewed code will be merged to dev.
After collecting a couple of features and some local test runs, it may be merged to main
and deployed. 

### Run locally in a dev environment
You can start the bot with extra settings and datapaths, if you not want to mess up your
actual data or settings. Using a seperate settings file and a dev subfolder in the data
folder (for seperate data and log files) run:
```sh
python ilaris_bot.py --settings config/settings_dev.yml --datapath data/dev
```
Missing folders and files are created automatically.

### Test the bot
There are a few tests in the `tests` folder that can be run with: 
```sh
pytest
```
They do not cover all commands or actions and don't interpret the message 
content yet, but its a quick way to make sure that code changes do not directly
cause errors in this commands. Just run it before committing changes.
