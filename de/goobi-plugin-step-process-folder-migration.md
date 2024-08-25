---
description: >-
  Step Plugin für Bearbeitung von Verzeichnissen und Ordnern
---

# Migration von Vorgangsverzeichnissen

## Übersicht

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_step_processfolder_migration
Repository               | [https://github.com/intranda/goobi-plugin-step-process-folder-migration](https://github.com/intranda/goobi-plugin-step-process-folder-migration)
Lizenz              | GPL 2.0 oder neuer 
Letzte Änderung    | 24.08.2024 15:13:25


## Einführung
Diese Dokumentation erläutert, wie dieses Plugin dafür genutzt werden kann, verschiedene Datei- oder Verzeichnisoperationen auszuführen.

## Installation
Um das Plugin nutzen zu können, müssen folgende Dateien installiert werden:

```bash
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_processfolder_migration.jar
/opt/digiverso/goobi/config/plugin_intranda_step_processfolder_migration.xml
```

Nach der Installation des Plugins kann dieses innerhalb des Workflows für die jeweiligen Arbeitsschritte ausgewählt und somit automatisch ausgeführt werden.

![Konfiguration des Arbeitsschritts für die Nutzung des Plugins](images/goobi-plugin-step-process-folder-migration_screen1_de.png)


## Überblick und Funktionsweise
Dieses Plugin führt automatisierte Datei- und Verzeichnismanipulationen durch. Dabei lassen sich in der Konfigurationsdatei verschiedene Regeln definieren, die festlegen, welche Aktionen auf welche Verzeichnisse angewendet werden sollen.

## Konfiguration
Die Konfiguration des Plugins erfolgt in der Datei `plugin_intranda_step_processfolder_migration.xml` wie hier aufgezeigt:

```xml
<config_plugin>
    <!--
        order of configuration is:
          1.) project name and step name matches
          2.) step name matches and project is *
          3.) project name matches and step name is *
          4.) project name and step name are *
	-->
    
    <config>
        <!-- which projects to use for (can be more then one, otherwise use *) -->
        <project>*</project>
        <step>*</step>
        
        <!-- a rule specifies what shall happen inside of the process folder
        	- `action` defines what shall be done:
        		- `move`: renaming of the source path to a new target path
        		- `delete`: deleting the folder or file
        		- `create`: create a folder under the given source path
        		- `copy`: renaming of the source path to a new target path
        	- `source` is the relative path inside of the process folder that is to be adapted
        	- `target`: is the new name that shall be given to the source path if this is available, 
        				a wildcard is only allowed for the part of the path if action is 'delete'
        	
        	Please notice that the rules are executed in the order they are listed here.
		 -->
        <rule action="move" source="images/master_{processtitle}_media" target="images/orig_{processtitle}_tif" />
        <rule action="move" source="images/{processtitle}_media" target="images/{processtitle}_tif" />
        <rule action="create" source="trallala/" />
        <rule action="copy" source="thumbs/master_{processtitle}_media_400" target="trallala/ich_war_hier_{processtitle}_tif_400" />
        <rule action="move" source="thumbs/master_{processtitle}_media_400" target="thumbs/orig_{processtitle}_tif_400" />
        <rule action="move" source="thumbs/master_{processtitle}_media_800" target="thumbs/orig_{processtitle}_tif_800" />
        <!-- 
        <rule action="delete" source="images/layoutWizzard-temp" />
        -->
        <rule action="delete" source="taskmanager" />
        <rule action="delete" source="ocr1" />
        <rule action="create" source="bla/blu/" />
        <rule action="delete" source="thumbs/orig_{processtitle}_tif_400" />
        <!-- 
        <rule action="delete" source="images/layoutWizzard-temp/analysis/0000000.?.tif" />
        <rule action="delete" source="images/layoutWizzard-temp/analysis/000000.?.?.tif" />
        <rule action="delete" source="images/layoutWizzard-temp/analysis/00000.*.tif" />
        <rule action="delete" source="images/layoutWizzard-temp/analysis/00000.*" />
        <rule action="delete" source="imageData.xml..*" />
        -->
        <rule action="delete" source="images/layoutWizzard-temp/analysis/000000.?.?.tif" />
        <rule action="delete" source="imageData.xml..*" />
        <rule action="delete" source="meta.xml..*" />
        <rule action="delete" source="meta_anchor.xml..*" />
        <rule action="delete" source=".*_db_export.xml" />
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
`action`                | Dies ist der Parameter, der bestimmt, welche Aktion auf dem angegebenen Verzeichnis oder der Datei ausgeführt wird. Die möglichen Werte sind: `delete`, `create`, `move` oder `copy`.|
`delete`                | Diese Aktion löscht einen Ordner oder ein Verzeichnis. |
`create`                | Diese Aktion erstellt einen neuen Ordner oder ein Verzeichnis. |
`move`                  | Diese Aktion bewegt einen Ordner oder ein Verzeichnis vom aktuellen Ort, zum Zielort. |
`copy`                  | Diese Aktion kopiert einen Ordner oder ein Verzeichnis und fügt es am Zielort ein. |
`source`                | Das ist der Ordner oder das Verzeichnis, von welchem aus eine Aktion ausgeführt werden soll, zum Beispiel das Kopieren von einer Datei aus diesem Ordner.|
`target`                | Mit diesem Parameter wird das Verzeichnis festgelegt, zu welchem eine Aktion ausgeführt werden soll (zum Beispiel das Kopieren einer Datei in diesen Ordner).|