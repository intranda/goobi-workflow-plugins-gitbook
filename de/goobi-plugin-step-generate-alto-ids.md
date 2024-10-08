---
description: >-
  Dieses Step Plugin dient zur Generierung von fehlenden ALTO-IDs.
---

# Generate ALTO IDs

## Übersicht

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_step_generate_alto_ids
Repository               | [https://github.com/intranda/goobi-plugin-step-generate-alto-ids](https://github.com/intranda/goobi-plugin-step-generate-alto-ids)
Lizenz              | GPL 2.0 oder neuer 
Letzte Änderung    | 07.09.2024 14:15:36


## Einführung
In dieser Dokumentation wird das Plugin zur Erzeugung fehlender ALTO-IDs erläutert. Dies ist erforderlich, damit der ALTO-Editor richtig funktioniert. Einige externe OCR-Tools stellen diese ALTO-IDs nicht zur Verfügung. Dieses Plugin kann dann verwendet werden, um sie nachträglich zu erzeugen.

## Installation
Um das Plugin nutzen zu können, müssen folgende Dateien installiert werden:

```bash
/opt/digiverso/goobi/plugins/step/plugin-step-generate-alto-ids-base.jar
```

Nach der Installation des Plugins kann dieses innerhalb des Workflows für die jeweiligen Arbeitsschritte ausgewählt und somit automatisch ausgeführt werden. Ein Workflow könnte dabei beispielhaft wie folgt aussehen:

![Beispielhafter Aufbau eines Workflows](images/goobi-plugin-step-generate-alto-ids_screen1_de.png)

Für die Verwendung des Plugins muss dieses in einem Arbeitsschritt ausgewählt sein:

![Konfiguration des Arbeitsschritts für die Nutzung des Plugins](images/goobi-plugin-step-generate-alto-ids_screen2_de.png)


## Überblick und Funktionsweise
Beim Starten des Plugins werden alle ALTO Dateien auf fehlende IDs geprüft.
Sollten fehlende IDs gefunden werden, wird zuerst ein Backup aller OCR Ergebnisse mitsamt der ALTO Dateien erstellt.
Danach werden die fehlenden ALTO IDs in allen Dateien ergänzt.


## Konfiguration
Dieses Plugin erfordert keine Konfiguration.