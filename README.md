# Bachelorarbeit quickstart

## Inhaltsverzeichiss

- [Bachelorarbeit quickstart](#bachelorarbeit-quickstart)
  - [Inhaltsverzeichiss](#inhaltsverzeichiss)
  - [Einleitung](#einleitung)
  - [Voraussetzung](#voraussetzung)
  - [Installation](#installation)
  - [Einrichten und Starten](#einrichten-und-starten)
    - [Rest Application Service Konfigurieren](#rest-application-service-konfigurieren)
    - [Starten](#starten)
    - [Node Red Einrichten](#node-red-einrichten)

## Einleitung

Hier gibt es eine kurze Anleitung zum einrichten von der [EdgeX Foundry](https://www.edgexfoundry.org/) Plattform mit einer Anbindung an den TH IoT MQTT Broker. Genutzt wird die Version 1.2-Geneva ohne Sicherheitsfeatures.

Die "Brücke" wird über einen Node Red erstellt, welcher die Daten in das benötigte format umwandelt und auf einen lokalen Broker schreibt. Von diesem Broker werden die Daten dann in die Plattform gelesen.

## Voraussetzung

Hierfür wird [Docker](https://www.docker.com/) und [Docker Compose](https://docs.docker.com/compose/) benötigt.

Für Windows und Mac nutzer empfehle ich [Docker Desktop](https://www.docker.com/products/docker-desktop) zu installieren. Da ist Docker Compose enthalten. Linux Nutzer können sich hier eine Anleitung für [ihr](https://hub.docker.com/search?q=&type=edition&offering=community&operating_system=linux) system ansehen. Docker Compose ist mittlerweile in vielen Repo's enthalten.

## Installation

Zum installieren einfach das GitHub Repository mit git clone Klonen oder als zip herunterladen.

## Einrichten und Starten

### Rest Application Service Konfigurieren

Damit mein Rest Application Service gleich mitgestartet wird, müssen in der Datei docker-compose.yml in den Zeilen 355 bis 371 die "#" entfernt werden, damit diese nicht mehr als Kommentare gelten. Dieser ist standartmäßig auskommentiert, damit er bei der entwicklung nicht stört.

Das sieht dann wie folgt aus:

```yaml
    edgex-restapp:
    image: mmomper/edgex-restapp
    container_name: edgex-app-service-rest
    hostname: edgex-app-service-rest
    ports:
        - 8080:8080
        - 8443:8443
    networks:
        - edgex-network
    volumes:
        - persist:/persist
    environment:
        ADMIN_USERNAME: admin
        ADMIN_PASSWORD: "strongpassword"
        CLIENT_COREDATA_HOST: edgex-core-data
        CLIENT_COREMETADATA_HOST: edgex-core-metadata
        CLIENT_CORECOMMAND_HOST: edgex-core-command
```

An dieser stelle am besten auch gleich das Passwort ändern. Dafür "strongpassword" in Zeile 368 ersetzen.

### Starten

Die Plattform kann jetzt mit folgendem Befehl gestartet werden:

```sh
docker-compose up -d
```

### Node Red Einrichten

Als nächstes muss der Node Red so konfiguriert werden, dass dieser sich mit dem TH MQTT Broker verbindet und die Daten verarbeitet.

Bilder zur Anleitung liegen im Ordner "Node Red Einrichtung".

Hierfür die URL <http://localhost:1880> öffnen und das Burger-Menü oben rechts anwählen. Jetzt auf den Punkt "Import" klicken, "select a file to import" auswählen, die Datei "flows.json" auswählen, auf "Import" klicken und dann "import copy" wählen. Zu guter letzt den Reiter "EdgeX Converter" auf der oberen Seite wählen.

Jetzt müssen noch die Zugangsdaten für den MQTT Broker hinterlegt werden. Dafür auf der rechten Seite auf "Global Configuration Nodes" -> "mqtt-broker" -> "th labor" doppelklicken und unter dem Reiter "Sicherheit" die Zugangsdaten eintragen und auf Aktualisieren klicken.

Jetzt oben rechts auf "deploy" klicken. Danach sollte unter den lila Feldern ein grünes Symbol mit dem Text "verbunden" auftauchen. Damit ist der Node Red erfolgreich eingerichtet und die Plattform funktionsfähig.
