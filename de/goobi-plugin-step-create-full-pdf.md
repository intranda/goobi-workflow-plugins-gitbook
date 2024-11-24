---
description: >-
  Dies ist die technische Dokumentation für das Goobi-Plugin für das automatische Generieren von PDF-Dateien aus Bildern
---

# Generierung von PDF-Dateien

## Übersicht

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_step_createfullpdf
Repository               | [https://github.com/intranda/goobi-plugin-step-create-full-pdf](https://github.com/intranda/goobi-plugin-step-create-full-pdf)
Lizenz              | GPL 2.0 oder neuer 
Letzte Änderung    | 17.09.2024 16:58:45


## Einführung
Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz dieses Plugins zum Generieren der PDF Dateien aus Bildern.


## Installation
Zur Nutzung des Plugins muss es an folgenden Ort kopiert werden:

```bash
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_createfullpdf-base.jar
```

Die Konfiguration des Plugins wird unter folgendem Pfad erwartet:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_createfullpdf.xml
```


## Überblick und Funktionsweise
Nachdem das Plugin korrekt installiert wurde, kann es in der Nutzeroberfläche für die Verwendung innerhalb des Workflows bei dem gewünschten Arbeitsschritt konfiguriert werden. Hierfür muss als Plugin der Wert `intranda_step_createfullpdf` ausgewählt und die Ausfühlung sollte als automatisch festgelegt werden.

![Auswahl des Plugins innerhalb der Workflowkonfiguration](images/goobi-plugin-step-create-full-pdf_screen1_de.png)


## Konfiguration
Die Konfiguration des Plugins erfolgt in der Datei `plugin_intranda_step_createfullpdf.xml` wie hier aufgezeigt:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<config_plugin>
    <!-- order of configuration is: 
         1.) project name and step name matches 
         2.) step name matches and project is * 
         3.) project name matches and step name is * 
         4.) project name and step name are * 
    -->
    <config>
        <!-- which projects to use for (can be more then one, otherwise use *) -->
        <project>*</project>

        <!-- which stepss to use for (can be more then one, otherwise use *) -->
        <step>*</step>

         <!-- Choose the source images folder that shall be used for PDF generation. Possible values are 'media' and 'master' -->
        <imagesFolder>media</imagesFolder>

   		<!-- single page pdf files shall be generated and kept (@enabled) -->
		<singlePagePdf enabled="false" /> 

        <!-- Full PDF file is generated and kept (@enabled) 
        	- @mode controls if PDF shall be generated based on METS file ('mets') or based on singlePagePdfs ('singlepages')
        	- @pdfConfigVariant sets up which config variant in contentServerConfig.xml should be used. If not set, then use default. -->
        <fullPdf enabled="true" mode="mets" pdfConfigVariant="pdfa"/>

        <!-- If set, then this ABSOLUTE path will be used to export the results. 
        	If no path is defined the PDF will be stored in the ocr/pdf-folder of the process -->
        <exportPath>/opt/digiverso/goobi/tmp</exportPath>
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


| Wert | Beschreibung |
| :--- | :--- |
| `imageFolder` | Dieser Parameter erwartet den Namen des Bildordners. Mögliche Werte sind `media` und `master`. Alle anderen Angaben werden als `media` interpretiert. |
| `singlePagePdf` | Das Attribut `enabled` dieses Parameters bestimmt, ob Einzelseiten-PDFs erzeugt werden sollen. |
| `fullPdf` | Das Attribut `enabled` dieses Parameters bestimmt, ob ein Gesamt-PDF erzeugt werden soll. Das Attribut `mode` steuert dabei, wie dieses PDF erzeugt werden soll. Hierfür steht der Wert `mets` zur Verfügung, um das PDF basierend auf der METS-Datei zu erzeugen. Alternativ kann als Wert `singlepages` verwendet werden, um das Gesamt-PDF aus den zuvor erzeugten Einzelseiten-PDFs zu generieren. Die Einzelseiten-PDFs werden dabei nur dann temporär erzeugt, wenn sie innerhalb der Konfiguration nicht bereits aktiviert wurden. Das Attribut `pdfConfigVariant` hingegen ist optional und legt fest, welche Konfigurationsvariante verwendet werden soll. Ist sie nicht gesetzt, wird `default` verwendet. |
| `exportPath` | Dieser optionale Parameter kann verwendet werden, um einen Pfad für den Export der PDF-Dateien festzulegen. Wenn er verwendet wird, wird ein absoluter Pfad erwartet. Ist er nicht angegeben, werden die PDF-Dateien innerhalb des `ocr`-Verzeichnisses des Vorgangs erzeugt. |