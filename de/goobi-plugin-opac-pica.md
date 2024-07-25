---
description: >-
  OPAC Plugin für die Datenübernahme von PICA Datensätzen
---

# PICA Import

## Übersicht

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_opac_pica
Repository               | [https://github.com/intranda/goobi-plugin-opac-pica](https://github.com/intranda/goobi-plugin-opac-pica)
Lizenz              | GPL 2.0 oder neuer 
Letzte Änderung    | 25.07.2024 12:02:03


## Einführung
Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz des Plugins. Mit Hilfe dieses Plugins können Daten aus einem externen System abgefragt und in Goobi übernommen werden. Der Katalog muss eine API bzw. URL haben, über die Datensätze als OPAC ausgeliefert werden können.


## Installation
Das Plugin besteht aus einer Datei:

```bash
plugin_intranda_opac_pica-base.jar
```

Diese Datei muss für den Nutzer `tomcat` lesbar an folgendem Pfad installiert werden:

```bash
/opt/digiverso/goobi/plugins/opac/plugin_intranda_opac_pica-base.jar
```


## Überblick und Funktionsweise
Wenn in Goobi nach einem Identifier gesucht wird, wird im Hintergrund eine Anfrage an die konfigurierte URL gestellt.

![Oberfläche von Goobi workflow zur Abfrage des Katalogs](images/goobi-plugin-opac-pica_screen1_de.png)

Nach der Abfrage des eigentlichen Datensatzes aus dem PICA-Katalog erfolgt das Mapping der Metadaten gemäß der im Regelsatz konfigurieren Regeln.


## Konfiguration
Das Plugin selbst verfügt nicht über keine eigene Konfiguration. Stattdessen erfolgt sämtliche Konfiguration über Anpassungen innerhalb von Goobi workflow bzw. der zugehörigen Regelsätze.

In der Datei `goobi_opac.xml` muss die Schnittstelle zum gewünschten Katalogsystem bekannt gemacht werden. Dies geschieht durch einen Eintrag, der wie folgt aussieht:

```xml
<catalogue title="PICA OPAC">
  <config address="kxp.k10plus.de" database="2.1"
    description="K10plus" iktlist="IKTLIST-GBV.xml" port="80"
    ucnf="UCNF=NFC&amp;XPNOFF=1" />
</catalogue>
```

Das Attribut `title` enthält den Namen, unter dem der Katalog in der Nutzeroberfläche ausgewählt werden kann, `address` die URL zum API-Endpoint und `database` die zu verwendende Datenbank.

Das Mapping der Inhalte eines PICA-Datensatzes erfolgt innerhalb des jeweilig verwendeten Regelsatzes von Goobi workflow. Genauere Informationen, wie dieses Mapping konfiguriert werden kann finden sich innerhalb der UGH-Dokumentation hier:

[https://docs.goobi.io/ugh-de/4/4.3/4.3.2](https://docs.goobi.io/ugh-de/4/4.3/4.3.2)