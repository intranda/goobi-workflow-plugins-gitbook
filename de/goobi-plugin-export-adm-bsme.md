---
description: >-
  Export Plugin für Goobi workflow zum Erzeugen spezieller Exportformate in die Software Imagen Media Archive Management
---

# Individueller Export für das DMS Imagen Media Archive Management

## Übersicht

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_export_adm_bsme
Repository               | [https://github.com/intranda/goobi-plugin-export-adm-bsme](https://github.com/intranda/goobi-plugin-export-adm-bsme)
Lizenz              | GPL 2.0 oder neuer 
Letzte Änderung    | 14.10.2024 09:27:36


## Einführung
Die vorliegende Dokumentation beschreibt die Installation, Konfiguration und den Einsatz des Export-Plugins für die Erzeugung spezieller Exportpakete für die Software Imagen Media Archive Management. Innerhalb des Plugins werden hierbei derzeit 5 spezielle Publikationstypen berücksichtigt und jeweils individuell verarbeitet.

## Installation
Um das Plugin nutzen zu können, müssen folgende Dateien installiert werden:

```bash
/opt/digiverso/goobi/plugins/export/plugin-export-adm-bsme-base.jar
/opt/digiverso/goobi/config/plugin_intranda_export_adm_bsme.xml
```

Nach der Installation des Plugins kann dieses innerhalb des Workflows für die jeweiligen Arbeitsschritte ausgewählt und somit automatisch ausgeführt werden. Ein Workflow könnte dabei beispielhaft wie folgt aussehen:

![Beispielhafter Aufbau eines Workflows](images/goobi-plugin-export-adm-bsme_screen1_de.png)

Für die Verwendung des Plugins muss dieses in einem Arbeitsschritt ausgewählt sein:

![Konfiguration des Arbeitsschritts für den Export](images/goobi-plugin-export-adm-bsme_screen2_de.png)


## Überblick und Funktionsweise
Dieses Plugin wird innerhalb des Workflows automatisch als Export-Plugin ausgeführt und erzeugt innerhalb eines konfigurierten Verzeichnisses die jeweils benötigten Daten. Dabei handelt es sich je nach Publikationstyp um:

- Bilddateien
- Plaintext-Dateien mit OCR-Ergebnissen
- ALTO-Dateien mit OCR-Ergebnissen
- METS-Dateien
- METS-Anchor-Dateien
- XML-Export-Dateien

![Beispielhafter Einblick in ein Exportverzeichnis für mehrere Publikationstypen](images/goobi-plugin-export-adm-bsme_screen3.png)

Insbesondere der Aufbau der XML-Export-Dateien ist je nach Publikationstyp sehr unterschiedlich. Hier einmal ein Beispiel für ein `Generic Print`-Publikationstyp:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<image>
  <ImageInfo>
    <Rights_to_Use>Yes</Rights_to_Use>
    <Right_Details>ADMN</Right_Details>
    <Media_Source>Goobi</Media_Source>
    <Media_Type>Image</Media_Type>
    <Publication_Name>الاتحاد - Al Ittihad</Publication_Name>
    <Source_Organization>Source Organization information</Source_Organization>
    <Barcode>123456789</Barcode>
    <Subject>Subject information</Subject>
    <Event_Date>2024-04-10</Event_Date>
    <Event_Name>Event Name information</Event_Name>
    <Photographer>Photographer information</Photographer>
    <Format>35mm</Format>
    <Persons_in_Image>Persons in Image information</Persons_in_Image>
    <location>Event Locations information</location>
    <Description>Description information</Description>
    <Backprint>Backprint information</Backprint>
    <Technical_Notes />
  </ImageInfo>
  <Files>
    <master>
      <Format>image/tiff</Format>
      <ResolutionUnit>PPI</ResolutionUnit>
      <Resolution>200.0</Resolution>
      <BitDepth>8</BitDepth>
      <ColorSpace>color</ColorSpace>
      <ScanningDevice>Bookeye 5</ScanningDevice>
      <ScanningDeviceID />
      <Width>1272</Width>
      <Height>1680</Height>
      <file>123456789-0001.tif</file>
    </master>
    <master>
      <Format>image/tiff</Format>
      <ResolutionUnit>PPI</ResolutionUnit>
      <Resolution>200.0</Resolution>
      <BitDepth>8</BitDepth>
      <ColorSpace>color</ColorSpace>
      <ScanningDevice>Bookeye 5</ScanningDevice>
      <ScanningDeviceID />
      <Width>1272</Width>
      <Height>1680</Height>
      <file>123456789-0002.tif</file>
    </master>
    <text Format="text/plain">123456789-0002.txt</text>
    <master>
      <Format>image/tiff</Format>
      <ResolutionUnit>PPI</ResolutionUnit>
      <Resolution>200.0</Resolution>
      <BitDepth>8</BitDepth>
      <ColorSpace>color</ColorSpace>
      <ScanningDevice>Bookeye 5</ScanningDevice>
      <ScanningDeviceID />
      <Width>1192</Width>
      <Height>1608</Height>
      <file>123456789-0003.tif</file>
    </master>
    <text Format="text/plain">123456789-0003.txt</text>
  </Files>
</image>
```

## Konfiguration
Die Konfiguration des Plugins erfolgt in der Datei `plugin_intranda_export_adm_bsme.xml` wie hier aufgezeigt:

```xml
<config_plugin>

	<!-- directories where to export to -->
	<targetDirectoryNewspapers>/opt/digiverso/export/bsme/mnt/export/Newspapers/</targetDirectoryNewspapers>
	<targetDirectoryMagazines>/opt/digiverso/export/bsme/mnt/export/Magazines/</targetDirectoryMagazines>
	<targetDirectoryPositives>/opt/digiverso/export/bsme/mnt/export/Positives/</targetDirectoryPositives>
	<targetDirectoryNegatives>/opt/digiverso/export/bsme/mnt/export/Negatives/</targetDirectoryNegatives>
	<targetDirectorySlides>/opt/digiverso/export/bsme/mnt/export/Slides/</targetDirectorySlides>
	<targetDirectoryGeneric>/opt/digiverso/export/bsme/mnt/export/Generic/</targetDirectoryGeneric>

	<!-- additional PDF copy directory, leave empty if not needed -->
	<pdfCopyNewspapers>/opt/digiverso/export/bsme/mnt/pdf/Newspapers/</pdfCopyNewspapers>
	<pdfCopyMagazines>/opt/digiverso/export/bsme/mnt/pdf/Magazines/</pdfCopyMagazines>

	<!-- main viewer url -->
	<viewerUrl>https://adm.goobi.cloud/viewer</viewerUrl>

	<!-- configured values to be used inside of the export xml, 
	    you can use variable replacer expressions here like e.g.: 
		- $(meta.CatalogIDDigital) 
		- $(meta.topstruct.TitleDocMain) 
		- $(process.Template) -->
	<volumeNumber>$(meta.CurrentNo)</volumeNumber>
	<rightsToUse>$(meta.AdmRightToUse)</rightsToUse>
	<rightsDetails>$(meta.AdmRightDetails)</rightsDetails>
	<source>Goobi</source>
	<mediaType>$(meta.AdmMediaType)</mediaType>
	<mediaGroup>$(meta.AdmMediaGroup)</mediaGroup>
	<sourceOrganisation>$(meta.AdmSourceOrganization)</sourceOrganisation>
	<frequency>$(meta.AdmIssueFrequency)</frequency>
	<eventName>$(meta.AdmEventName)</eventName>
	<eventNameEnglish>$(meta.AdmEventNameEnglish)</eventNameEnglish>
	<eventDate>$(meta.AdmEventDate)</eventDate>
	<eventTime>$(meta.AdmEventTime)</eventTime>
	<subject>$(meta.Subject)</subject>
	<subjectArabic>$(meta.AdmSubjectArabic)</subjectArabic>
	<subjectEnglish>$(meta.AdmSubjectEnglish)</subjectEnglish>
	<photographer>$(meta.AdmPhotographer)</photographer>
	<personsInImage>$(meta.AdmPersonsInImage)</personsInImage>
	<locations>$(meta.AdmEventLocations)</locations>
	<description>$(meta.Description)</description>
	<descriptionArabic>$(meta.AdmEventDescriptionArabic)</descriptionArabic>
	<editorInChief>$(meta.AdmEditorInChief)</editorInChief>
	<format>$(meta.Format)</format>
	<envelopeNumber>$(meta.AdmEnvelopeNumber)</envelopeNumber>
	<backprint>$(meta.AdmBackprint)</backprint>

	<!-- mets parameter -->
	<!-- if a field is empty or missing, project configuration is used -->
	<metsUrl addFileExtension="true">https://adm.goobi.cloud/viewer/sourcefile?id=
	</metsUrl>
	<resolverUrl>https://adm.goobi.cloud/viewer/piresolver?id=
	</resolverUrl>
	<metsPointerPath>https://adm.goobi.cloud/viewer/sourcefile?id=$(meta.topstruct.CatalogIDDigital).xml
	</metsPointerPath>
	<metsPointerPathAnchor>https://adm.goobi.cloud/viewer/sourcefile?id=$(meta.CatalogIDDigital).xml
	</metsPointerPathAnchor>
	<metsPointerAddFileExtension>true</metsPointerAddFileExtension>
	<rightsOwner>Abu Dhabi Media Company</rightsOwner>
	<rightsOwnerLogo>https://adm.goobi.cloud/viewer/resources/themes/reference/images/dfg_viewer_logo.png
	</rightsOwnerLogo>
	<rightsOwnerSiteURL />
	<rightsOwnerContact />
	<digiprovPresentation>https://adm.goobi.cloud/viewer/piresolver?id=$(meta.CatalogIDDigital)
	</digiprovPresentation>
	<digiprovReference />
	<digiprovPresentationAnchor>https://adm.goobi.cloud/viewer/piresolver?id=$(meta.topstruct.CatalogIDDigital)
	</digiprovPresentationAnchor>
	<digiprovReferenceAnchor />
	<rightsLicense />
	<rightsSponsor />
	<rightsSponsorLogo />
	<rightsSponsorSiteURL />
	<purl />
	<contentIds />

	<!-- global metadata settings -->
	<metadata>
		<purl>_purl</purl>
		<identifier>CatalogIDDigital</identifier>
		<issueDate>DateIssued</issueDate>
		<dateOfOrigin>DateOfOrigin</dateOfOrigin>
		<yearDate>CurrentNoSorting</yearDate>
		<titleLabel>TitleDocMain</titleLabel>
		<modsTitle>MainTitle</modsTitle>
		<issueNumber>CurrentNo</issueNumber>
		<issueName>IssueName</issueName>
		<issueNotes>AdmIssueNote</issueNotes>
		<sortNumber>CurrentNoSorting</sortNumber>
		<language>DocLanguage</language>
		<location>PhysicalLocation</location>
		<resourceType>TypeOfResource</resourceType>
		<anchorId>AnchorID</anchorId>
		<anchorTitle>AnchorTitle</anchorTitle>
		<accessConditionUse>AccessConditionUse</accessConditionUse>
		<accessConditionDetails>AccessConditionDetails
		</accessConditionDetails>
		<frequency>Frequency</frequency>
	</metadata>

	<docstruct>
		<newspaper>Newspaper</newspaper>
		<year>Year</year>
		<month>Month</month>
		<day>Day</day>
		<issue>NewspaperIssue</issue>
		<supplement>NewspaperSupplement</supplement>
		<newspaperStub>NewspaperStub</newspaperStub>
	</docstruct>

</config_plugin>

```

Die darin verwendeten Parameter werden hier detailliert: 

Parameter                   | Erläuterung
----------------------------|----------------------------------------
`targetDirectoryNewspapers` | Zielverzeichnis für Zeitungen
`targetDirectoryMagazines`  | Zielverzeichnis für Zeitschriften
`targetDirectoryPositives`  | Zielverzeichnis für Positives
`targetDirectoryNegatives`  | Zielverzeichnis für Negative
`targetDirectorySlides`     | Zielverzeichnis für Slides
`targetDirectoryGeneric`    | Zielverzeichnis für Generic Prints
`pdfCopyNewspapers`         | Zielverzeichnis zur Generierung von PDF-Dateien für Zeitungen
`pdfCopyMagazines`          | Zielverzeichnis zur Generierung von PDF-Dateien für Zeitschriften
`viewerUrl`                 | URL für den Goobi viewer
`rightsToUse`               | Angabe von Nutzungsrechten
`rightsDetails`             | Details über die Nutzungsrechte
`source`                    | Angabe der Quelle der Digitalisate
`mediaType`                 | Typ der Medien
`sourceOrganisation`        | Organisation, die für die Inhalte verantwortlich ist
`frequency`                 | Erscheinungshäufigkeit
`eventName`                 | Nennung des dokumentierten Ereignisses
`eventDate`                 | Angabe des Datums, wann das Ereignis stattfand
`eventTime`                 | Angabe des Uhrzeit, wann das Ereignis stattfand
`subject`                   | Allgemeine Schlagworte
`subjectArabic`             | Angabe der Schlagworte in Arabisch
`subjectEnglish`            | Angabe der Schlagworte in Englisch
`photographer`              | Informationen zum Fotografen des Bildes
`personsInImage`            | Abgebildete Personen im Bild
`locations`                 | Angabe zum Ort der Aufnahme
`description`               | Erläuterungen und Beschreibungen zur Aufnahme
`editorInChief`             | Verantwortlicher Herausgeber
`format`                    | Formatinformationen
`envelopeNumber`            | Identifier des Umschlags, in dem die Dokumente aufbewahrt werden
`backprint`                 | Informationen über Inhalte auf der Rückseite


Für eine einfachere Inbetriebnahme befindet sich in `install`-Ordner des Plugins einn Verzeichnis mit den zwei passende Regelsätze als Referenz, die zu der hier aufgeführte Konfigurationsdatei passen.