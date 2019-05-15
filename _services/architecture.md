---
title: 'Architecture'
---

The system behind SFT became large really fast. Rethinking the architecture happens way more often than thought. This post explains the actual solution.

## Overview

![architecture](/modules/smart-football-table/architecture/SmartFootballTable_Architecture.png){:class="img-responsive"}

The modules combined in SFT are the following:

- Ball Detection (Camera + OpenCV, written in Python)
- Analysis (written in Java)
- Messaging (solved with MQTT)
- An UI for the results (Angular)
- A controller for the sound device (solved with bash)
- LED-Module (Java and Microcontroller)

With exception of the ball detection and analysis (following soon), each module is packed inside a Docker container. Not only does this avoid to install every neccessary technology on your own system, but with an docker-compose file starting the application is faster than doing it manually. Combining the modules in one git repository with git-submodule improves the usability of installation.
