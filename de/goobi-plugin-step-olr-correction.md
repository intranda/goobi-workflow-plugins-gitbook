---
description: >-
  Step-Plugin für die Korrektur von erkannten Inhaltsverzeichnissen und die Generierung von PICA-Datensätzen
---

# Korrektur von Inhaltverzeichnissen nach einer OLR

## Übersicht

Name                     | Wert
-------------------------|-----------
Identifier               | plugin_intranda_step_olr-correction
Repository               | [https://github.com/intranda/goobi-plugin-step-olr-correction](https://github.com/intranda/goobi-plugin-step-olr-correction)
Lizenz              | GPL 2.0 oder neuer 
Letzte Änderung    | 22.08.2024 07:31:43


## Einführung
Diese Dokumentation beschreibt die Installation, Konfiguration und Verwendung des Plugins.

## Installation
Das Programm besteht aus diesen Dateien:

``` bash
plugin_intranda_step_olr-correction.jar
plugin_intranda_step_olr-correction.xml
```

Die Datei `plugin_intranda_step_olr-correction.jar` enthält die Programmlogik und sollte in diesen Pfad kopiert werden: `/opt/digiverso/goobi/plugins/step`.

Die Datei `plugin_intranda_step_olr-correction.xml` ist die Konfigurationsdatei und sollte in den Ordner `/opt/digiverso/goobi/config/` kopiert werden

Nach der Installation des Plugins kann dieses in einem Arbeitsschritt ausgewählt werden.

![Konfiguration des Arbeitsschrittes zur Verwendung des Plugins](images/goobi-plugin-step-olr-correction_screen1_de.png)

## Konfiguration
Die Konfiguration des Plugins erfolgt in der Datei `plugin_intranda_step_olr-correction.xml` wie hier aufgezeigt:

```xml
<config_plugin>
    <config>
        <!-- which projects to use for (can be more then one, otherwise use *) -->
        <project>*</project>
        <step>*</step>
        <!-- which images to use -->
        <useOrigFolder>true</useOrigFolder>
        
         <!-- Are the files originally digital rather than scanned? -->
        <bornDigital>true</bornDigital>      
        
        <!-- which image sizes to use for the big image -->
        <imagesize>800</imagesize>
        <imagesize>3000</imagesize>
        <!-- which image format to generate -->
        <imageFormat>jpg</imageFormat>
        
        <!-- colors for different groups -->
        <class>
            <type>pagenum</type>
            <color>#00FFFA</color>
        </class>
        <class>
            <type>title</type>
            <color>#FF5D15</color>
        </class>
        <class>
            <type>author</type>
            <color>blue</color>
        </class>
        <class>
            <type>institution</type>
            <color>#2F8C3C</color>
        </class>
        <class>
            <type>entry</type>
            <color>grey</color>
        </class>        
    </config>
</config_plugin>


```

### Allgemeine Parameter 
Der Block `<config>` kann für verschiedene Projekte oder Arbeitsschritte wiederholt vorkommen, um innerhalb verschiedener Workflows unterschiedliche Aktionen durchführen zu können. Die weiteren Parameter innerhalb dieser Konfigurationsdatei haben folgende Bedeutungen: 

| Parameter | Erläuterung | 
| :-------- | :---------- | 
| `project` | Dieser Parameter legt fest, für welches Projekt der aktuelle Block `<config>` gelten soll. Verwendet wird hierbei der Name des Projektes. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. | 
| `step` | Dieser Parameter steuert, für welche Arbeitsschritte der Block `<config>` gelten soll. Verwendet wird hier der Name des Arbeitsschritts. Dieser Parameter kann mehrfach pro `<config>` Block vorkommen. | 


### Weitere Parameter 
Neben diesen allgemeinen Parametern stehen die folgenden Parameter für die weitergehende Konfiguration zur Verfügung: 


Parameter               | Erläuterung
------------------------|------------------------------------
`bornDigital`| Wenn dies zutrifft, wird davon ausgegangen, dass die Dateien digital erstellt wurden und es sich nicht um gescannte Seiten handelt. |