---
title: 'Architektur DE'
---

Eine Übersicht über die verschiedenen Bestandteile, die SFT ausmachen.

## Übersicht

Das folgende Architekturbild zeigt eine Übersicht über die Bestandteile des SFT-Projekts:

![architecture](/modules/smart-football-table/architecture/SmartFootballTable_Architecture.png){:class="img-responsive"}

Eine schlanke Architektur mit wenigen "Point of Failures" war von Beginn an das Ziel des Projekts.
Es gibt keinen Grund, dies bei der kleinen Anzahl an Services anders zu lösen.

Das Herzstück stellt der MQTT-Broker da. Jeder Service kommuniziert über MQTT.
Damit stellt der Broker auch das kritischste ELement dar; fällt er aus, funktioniert die Anwendung nicht mehr.
Um den Broker finden sich dann die Services mit ihrer dedizierten Aufgabe, sei es das Kameramodul, welches sich um die Ballerkennung kümmert oder die Soundausgabe.

Auf diese Weise können auf simple Art und Weise weitere Services angebunden (oder auch entfernt) werden. Solange sie mit MQTT kommunizieren können ist eine Anbindung problemfrei möglich.

## Komponenten

Insgesamt sind zurzeit folgende Komponenten teil von SFT:

* MQTT-Broker
* Ball-Erkennung (Kamera + OpenCV, in Python) -> [GitHub-Link](https://github.com/smart-football-table/smart-football-table-detection)
* Analyse der erkannten Ballpositionen (in Java) -> [GitHub-Link](https://github.com/smart-football-table/smart-football-table-cognition)
* simple UI für die Anzeige der Analyse (in Angular) -> [GitHub-Link](https://github.com/smart-football-table/smart-football-table-ui)
* ein kleiner Service zur Soundausgabe (in Bash gelöst) -> [GitHub-Link](https://github.com/smart-football-table/smart-football-table-sound)
* Steuerung der LED-Leiste (in Java mit Microcontroller) -> [GitHub-Link](https://github.com/smart-football-table/smart-football-table-ledcontrol)

## Containerisierung mit Docker

Jeder Service läuft in einem Docker-Container (mit Ausnahme von der Ball-Erkennung in Python, da aktuell die Freigabe des Videos nach aussen noch nicht funktioniert).
Mithilfe einer übergreifenden docker-compose-Konfiguration kann die Anwendung auf diese Weise systemunabhängig schnell und einfach hochgefahren werden.

## Submodule in Git

Ein zentrales Repository, welches die Services als Submodule hinterlegt, vereinfacht das Sicherstellen einer lauffähigen Anwendung.
Die Subdmodule sind dabei auf einen bestimmten Commit verlinkt, um einen gleichen Stand sicherzustellen.

## Gründe sowie Vor- und Nachteile dieser Architektur

Der Schnitt der Services auf die oben beschriebene Art und Weise basiert auf einigen Annahmen, welche wir getroffen haben:

* Module sollen unabhängig voneinander entwickelt werden können, ebenso unabhängig voneinander auch refactored werden.
* Dabei hilft Entkopplung: Unabhängig voneinander arbeiten zu können war eine Grundlage für die getroffenen Architekturentscheidungen. Dafür bedarf es nicht zwangsweise Microservices.
* Quereffekte in hoher Zahl wie beispielsweise das Wegbrechen von Modulen sollen selten auftreten können. Noch wichtiger: Ein Teilausfall soll keinen Komplettausfall nach sich ziehen (mit genannter Ausnahme des Brokers. Bei einem Totalausfall kann aber fast immer beim Broker nachgesehen werden und das Problem ist gefunden).

## Technologieauswahl

Eine Frage, die aufkommen kann, ist: Warum wurden für die verschiedenen Module so viele verschiedene Technologien gewählt?
Auch wenn es eine valide Option ist, einfach eine Sprache wie Java zu wählen, haben wir uns an folgenden Gedanken gehalten:

*Welches Werkzeug passt zu meinem Problem bzw. zu meinem Team und dessen Fähigkeiten?*

Die erhöhte Komplexität durch verschiedene Werkzeuge wird in unseren Augen erfolgreich kompensiert durch die bessere Anpassungsfähigkeit der Werkzeuge an das Problem. Sofern im Team mit den gewählten Technologien umgegangen werden kann gibt es keinen Grund, sich durch eine Standardisierung einzuschränken.
