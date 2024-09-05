---
description: >-
  Step Plugin für die automatische Anreicherung von Vokabularen
---

# Vokabularanreicherung

## Übersicht

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_step_vocabulary_enrichment
Repository               | [https://github.com/intranda/goobi-plugin-step-vocabulary-enrichment](https://github.com/intranda/goobi-plugin-step-vocabulary-enrichment)
Lizenz              | GPL 2.0 oder neuer 
Letzte Änderung    | 04.09.2024 09:08:24


## Einführung
Diese Dokumentation bietet einen kurzen Überblick über dieses Plugin, das verwendet wird, um Vokabulare basierend auf Metadaten, die in METS-MODS-Dateien innerhalb von Goobi-Workflows gefunden werden, anzureichern.

## Installation
Um das Plugin nutzen zu können, müssen folgende Dateien installiert werden:

```
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_vocabulary_enrichment.jar
/opt/digiverso/goobi/config/plugin_intranda_step_vocabulary_enrichment.xml
```

Sobald das Plugin installiert ist, kann es innerhalb des Workflows für die jeweiligen Arbeitsschritte ausgewählt und somit automatisch ausgeführt werden. 


## Überblick und Funktionalität
Das Programm untersucht die Metadatenfelder einer METS-MODS-Datei aus einem Goobi-Prozess. Für jedes in der Konfigurationsdatei definierte `<item>` durchsucht das Plugin die METS-MODS-Datei nach den angegebenen Metadaten. Die Suche kann auf die oberste Struktur eines Dokuments beschränkt werden, indem die Form `meta.topstruct.metadataName` verwendet wird. Wenn ein oder mehrere Metadaten gefunden werden, wird das benannte Vokabular durchsucht. Wenn das Vokabular keinen Eintrag mit dem im `target`-Feld spezifizierten Wert enthält, der mit dem Wert der gefundenen Metadaten übereinstimmt, wird ein neuer Vokabulareintrag erstellt, bei dem das `target`-Feld den Wert der gefundenen Metadaten erhält. Wenn es `generate`-Felder im `<item>` gibt, werden die Felder des neuen Vokabulareintrags, die dort angegeben sind, mit den im `content`-Attribut definierten Werten versehen.


## Konfiguration
Das Plugin wird in der Datei `plugin_intranda_step_vocabulary_enrichment.xml` wie folgt konfiguriert:

```xml
<config_plugin>
    <!-- order of configuration is: 1.) project name and step name matches 2.) step name matches and project is * 3.) project name matches and step name is 
        * 4.) project name and step name are * -->

    <config>
        <!-- which projects to use for (can be more then one, otherwise use *) -->
        <project>*</project>
        <step>*</step>

        <item>
            <source>meta.originalAccessPersons</source>
            <vocabulary>Access points</vocabulary>
            <target>original value</target>
            <generate field="type" content="AccessPersons" />
            <generate field="authority" content="viaf" />
            <generate field="authorityURI" content="https://viaf.org/" />
        </item>
        <item>
            <source>meta.originalAccessCorporate</source>
            <vocabulary>Access points</vocabulary>
            <target>original value</target>
            <generate field="type" content="AccessCorporate" />
            <generate field="authority" content="viaf" />
            <generate field="authorityURI" content="https://viaf.org/" />
        </item>
        <item>
            <source>meta.originalAccessLocations</source>
            <vocabulary>Access points</vocabulary>
            <target>original value</target>
            <generate field="type" content="AccessLocations" />
            <generate field="authority" content="geonames" />
            <generate field="authorityURI" content="http://www.geonames.org/" />
        </item>
        <item>
            <source>meta.originalAccessSubject</source>
            <vocabulary>Access points</vocabulary>
            <target>original value</target>
            <generate field="type" content="AccessSubject" />
            <generate field="authority" content="geonames" />
            <generate field="authorityURI" content="http://www.geonames.org/" />
        </item>
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


Parameter               | Erklärung
------------------------|------------------------------------
|`item` | Jedes Metadatum, das zu einem Vokabular hinzugefügt werden soll, wird in einem Item definiert. |
|`source`   | Dies definiert das Metadatenfeld, aus dem der Vokabulareintrag entnommen werden soll.    |
|`vocabulary`   | Der Name des zu ergänzenden Vokabulars.  |
|` target`  | Der Name des Feldes im Vokabular, das den Wert der Quelle erhalten soll. |
|`generate`   | Dies sind optionale Felder: Wenn sie angegeben sind, erhält jedes Feld im generierten Vokabulareintrag, das durch das Attribut `field` spezifiziert ist, den im Attribut `content` angegebenen Wert.  |