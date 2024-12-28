---
description: >-
  Workflow Plugin für den Import von Markenstudien und Werbemitteln an der WU Wien
---

# Massenimport für Markenstudien und Werbemittel

## Übersicht

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_workflow_wu_wma_import
Repository               | [https://github.com/intranda/goobi-plugin-workflow-wu-wma-import](https://github.com/intranda/goobi-plugin-workflow-wu-wma-import)
Lizenz              | GPL 2.0 oder neuer 
Letzte Änderung    | 18.12.2024 13:57:25


## Einführung
Dieses Workflow-Plugin erlaubt einen Massenimport für die Markenstudien und das Werbemittelarchiv an der Wirtschaftsuniversität Wien.

## Installation
Um das Plugin nutzen zu können, müssen folgende Dateien installiert werden:

```bash
/opt/digiverso/goobi/plugins/workflow/plugin-workflow-wu-wma-import-base.jar
/opt/digiverso/goobi/plugins/GUI/plugin-workflow-wu-wma-import-gui.jar
/opt/digiverso/goobi/config/plugin_intranda_workflow_wu_wma_import.xml
```

Für eine Nutzung dieses Plugins muss der Nutzer über die korrekte Rollenberechtigung verfügen.

![Ohne korrekte Berechtigung ist das Plugin nicht nutzbar](images/goobi-plugin-workflow-wu-wma-import_screen1_de.png)

Bitte weisen Sie daher der Gruppe die Rolle `Plugin_workflow_wu_wma_import` zu.

![Korrekt zugewiesene Rolle für die Nutzer](images/goobi-plugin-workflow-wu-wma-import_screen2_de.png)


## Überblick und Funktionsweise
Wenn das Plugin korrekt installiert und konfiguriert wurde, ist es innerhalb des Menüpunkts `Workflow` zu finden.

![Nutzeroberfläche des Plugins](images/goobi-plugin-workflow-wu-wma-import_screen3_de.png)

Hier kann nun auf der rechten Seite ein Import für ein Datenset gestartet werden.

![Gestarteter Import](images/goobi-plugin-workflow-wu-wma-import_screen4_de.png)

Nach Beginn des Imports der Daten ist ein Fortschritt über den Forschrittsbalken ersichtlich. Details über den Import der einzelnen Datensätze lassen sich im darunter aufgeführten Log einsehen.

## Konfiguration
Die Konfiguration des Plugins erfolgt in der Datei `plugin_intranda_workflow_wu_wma_import.xml` wie hier aufgezeigt:

```xml
<config_plugin>
	
	<title>Mass import for the WU</title>

	<!-- importsets with source folder definition -->
	<importSet title="Markenstudien 1" workflow="Marken-Workflow" source="/opt/digiverso/import/markenstudien01/" />
	<importSet title="Markenstudien 2" workflow="Marken-Workflow" source="/opt/digiverso/import/markenstudien02/" />
	<importSet title="Markenstudien 3" workflow="Marken-Workflow" source="/opt/digiverso/import/markenstudien03/" />
	<importSet title="Markenstudien 4" workflow="Marken-Workflow" source="/opt/digiverso/import/markenstudien04/" />
			
</config_plugin>

```

Die folgende Tabelle enthält eine Zusammenstellung der Parameter und ihrer Beschreibungen:

Parameter               | Erläuterung
------------------------|------------------------------------
`title`                 | Hier kann ein individueller Titel für die Anzeige im Menü und die Überschriften des Plugins festgelegt werden.
`importSet`             | Mit diesem Element lassen sich individuelle Importsets definieren. Sie bestehen jeweils aus einem `title` für den anzuzeigenden Namen, sowie einer Angabe für den Speicherort, von dem die Daten importiert werden sollen. Der Speicherort kann dabei beliebig tiefe Verzeichnisstrukturen aufweisen. Ausserdem läßt sich mit `workflow` festlegen, welche Produktionsvorlage für das Anlegen von Goobi-Vorgängen verwendet werden soll.