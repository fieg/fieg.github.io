---
layout: post
title: Auto start SickBeard at login on Mac OSX
---

[SickBeard](http://sickbeard.com/) is an automated TV Show downloader, which I’m using together with my Plex installation. Out of the box Sickbeard does not start automatically when you boot OSX. I found a solution by using Automator.

1. Open Automator and choose a new Workflow
2. Add a ‘Run Shell Script’ action
3. Add to following script in the textbox: “/usr/local/bin/python /path/to/sickbeard/sickbeard.py -d”
4. Save the workflow as an Application in the SickBeard application folder
5. Goto System Preferences > Accounts > User > Login Items
6. Add the Application you just created with Automator

Done. It’s that simple.
