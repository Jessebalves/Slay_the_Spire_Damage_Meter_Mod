# Slay the Spire 2 Damage Meter
<details>
<summary>Table of Contents</summary>
  
- [Summary](#summary)
- [Features](#features)
- [Visuals](#visuals)
- [Technologies](#technologies)
- [What I Learned](#what-i-learned)
- [Setup and Installation](#setup-and-installation)
- [Usage](#usage)
- [Code Examples](#code-examples)
- [How to Contribute](#how-to-contribute)
- [Contact](#contact)
- [Acknowledgments](#acknowledgments)

</details>

## Summary
*Mod of Slay the Spire 2 that keeps track of players damage dealt per turn, average damage per turn, total damage over the run, and damage taken over the run. Intended to use for multiplayer :).*

## Features
<!--List the standout features of your project and what makes it unique.-->
- **Damage Dealt per Combat Tracked**: Able to see damage amount dealt by the player/players for .

## Visuals
<!--*Insert images or gifs showing your project in action. Consider before/after shots, workflows, or demos.*-->


## Technologies
<!--Detail the technologies, languages, frameworks, and tools used in the project.-->
- C#
- Net SDK
- GoDot

## What I Learned
<!-- Highlight specific skills or concepts you learned or improved upon while working on this project. This section should directly address potential employer interests. -->
- **Concept 1**: Description.

## Setup and Installation
<!--*Provide a clear, step-by-step guide to set up the project locally.*-->
1. Clone the repo: `git clone https://github.com/Jessebalves/TrashScheduleNJ.git`
2. Install Visual Studio Code.
3. Once installed, navigate to extensions. Download the extension named Live Server to view live changes made to the code base.
4. Right click on the file you want to open with Live Server, then select the Live Server option to view live changes.

Steps(Mac OS): 
1. Navigate to ~/Library/Application\ Support/steam/steamapps/common/Slay\ the\ Spire\ 2/SlayTheSpire2.app/Contents/Resources/data_sts2_macos_arm64/sts2.dll.
2. The asset pack is at ~/Library/Application Support/Steam/steamapps/common/Slay the Spire 2/SlayTheSpire2.app/Contents/Resources/Slay the Spire 2.pck.
3. Make copies of 0Harmony.dll and sts2.dll to a different directory. This is to prevent interference with and overwriting the original files.

### Server install (Ubuntu)
`install.sh` sets up everything the live site needs on a fresh Ubuntu server (tested on 26.04 LTS): the system packages, a Python virtual environment, the MySQL database and address table, the `python_server` service and the nginx site.

1. Clone the repo on the server: `git clone https://github.com/Jessebalves/TrashScheduleNJ.git`
2. Run the installer: `cd TrashScheduleNJ && sudo ./install.sh`
3. If MySQL root already has a password, pass it in: `sudo MYSQL_ROOT_PASSWORD='...' ./install.sh`

To update the server later, run `./update.sh` from the checkout. It pulls the latest commits for the current branch and runs the installer again. It builds the address table again only when the files in `data/` have changed (or `REIMPORT=1` is set). When running `./update.sh` you will be prompted for the MySQL Root password.

Not automated: DNS records and the TLS certificate. Place the certificate and key at `/etc/ssl/cert.pem` and `/etc/ssl/key.pem`, then run `install.sh` again to switch nginx to HTTPS. Until then the site is served over HTTP.

Database credentials are never stored in the repo. The installer creates a read-only MySQL user for the web server and writes its password to `/etc/trashschedulenj/trashschedule.env`. 

To remove the site, run `sudo ./uninstall.sh` (it lists what it will remove and asks first; `sudo DRY_RUN=1 ./uninstall.sh` only shows it). It removes the service, the nginx site, the code, the venv, `/etc/trashschedulenj`, and the MySQL database and user (`KEEP_DATABASE=1` keeps them). nginx and MySQL themselves stay installed unless `REMOVE_PACKAGES=1` is set. That option purges every MySQL database and nginx site on the server.

### Google Maps address search
The home page lets visitors pick their address from a Google Maps list. This needs a Google Maps API key:

1. In the Google Cloud console, create a key and turn on **Maps JavaScript API** and **Places API (New)** for it.
2. Restrict the key to your website (HTTP referrer `https://trashschedulenj.com/*`) and to those two APIs. The key is sent to every visitor's browser, the restriction is what stops other websites from using it.
3. Give it to the installer once, it is remembered after that: `sudo GOOGLE_MAPS_API_KEY='...' ./install.sh`

The key is stored in `/etc/trashschedulenj/trashschedule.env`, not in the repo. Without a key the page shows boxes to type the house number, street and town in instead.

### Running it on your own computer
No database is needed for development. This reads the addresses straight from the `data/` folder:

```
pip install -r requirements.txt
ADDRESS_SOURCE=csv python3 test_server.py
```

Then open http://127.0.0.1:5000 in your browser. Run the tests with `python3 -m unittest discover tests`.

### Adding a town
A town is two files and no code, see [towns/README.md](towns/README.md):

- `towns/<town>.json`: the pickup types, days, holidays and notes
- `data/<town>.csv`: the streets and which ward or route each one is on

## Usage
*This project has limited usage due to restricted API keys and security behind our own database. Users can connect their own database through the python file running flask.*

## Code Examples
<!-- *Show small, but significant snippets of code from your project.* -->
<img width="598" height="812" alt="Screenshot 2026-08-07 131721" src="https://github.com/user-attachments/assets/360b9c12-0eaf-4b01-9e45-fbe4ffc56df0" />


## How to Contribute
<!--*Encourage contributions and provide guidelines for how others can help.*-->
Our system uses Flask on the backend. The easiest way to help is to add your town: write its rules in `towns/<town>.json` and its street list in `data/<town>.csv`, as described in [towns/README.md](towns/README.md). A new pickup type, such as grass and leaves, is one more entry in the town's file and shows up as its own row in the table.

## Contact
<!--- *First and last name* - *Email address*-->
- *Jesse Alves* - *jessebalves@gmail.com*
- *Daniel Lordelo* - *daniellordelo@gmail.com*
<!--- *Any other contact information*-->

## Acknowledgments
<!--*Credits to individuals or resources that helped you during the project.*-->
- The Architect and the Slay the Spire 2 Team. 
