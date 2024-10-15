# Entwickeln

Wenn du den Bot anpassen oder weiterentwickeln möchtest oder einfach nur deine Daten bei dir behalten willst, kannst du deine eigene Instanz des Bots laufen lassen. In jedem Fall brauchst du einen eigenen Bot-Token und wenn er permanent erreichbar sein soll einen Server (ein raspberry pi sollte reichen). Um ihn nur zeitweise zu nutzen lässt er sich einfach von einer lokalen Windows/Mac/Linux Installation starten. Die folgenden Befehle und Infos richten sich an Benutzer mit einem frischen Ubuntu-System und den wichtigsten Grundkentnissen. Wir helfen bei Fragen aber gern so gut wir können: [Kontakt](kontakt.md)

## Bot Token
Hier findest du eine Anleitung zum erstellen eines Discord Bot Tokens](https://discordpy.readthedocs.io/en/stable/discord.html).
Mit diesem Token hat man die Kontrolle über den Bot, er sollte also geheim bleiben.

TODO: Intents/Berechtigungen


## Entwickler Installation

clone the repository:
```sh
git clone git@github.com:XaverStiensmeier/ilarisdiscordbot.git
cd ilarisdiscordbot
```

Erstelle und aktiviere eine virtuelle Umgebung mit allen Paketen die der Bot braucht:
Create and activate a virtual environment
(change the path if you like) and install all dependencies:
```sh
python3 -m venv .venv/ilarisbot
source .venv/ilarisbot/bin/activate
pip install -r requirements.txt
```

### Erster Start
Den Bot kannst du direkt mit `python ilaris_bot.py` starten. Beim ersten Start, fragt der Bot nach deinem Bot-Token und legt für dich eine settings.yml an. Dies kannst du auch manuell tun oder später bearbeiten (Siehe [Einstellungen](#Einstellungen)). Von jetzt an, kannst du den Bot in der venv (ggf. erneut `source .venv/ilarisbot/bin/activate`) mit diesem Befehl direkt starten. 

### Einstellungen

#### Generate missing resources
TODO: host resources for direct download.
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
