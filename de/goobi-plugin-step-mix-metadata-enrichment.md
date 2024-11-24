---
description: >-
  Dieses Step-Plugin für Goobi-Workflow nutzt JHove, um technische Metadaten aus Dateien zu extrahieren und die Ergebnisse in der METS-Datei eines Goobi-Vorgangs zu speichern.
---

# MIX Metadata Enrichment Plugin

## Übersicht

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_step_mix_metadata_enrichment
Repository               | [https://github.com/intranda/goobi-plugin-step-mix-metadata-enrichment](https://github.com/intranda/goobi-plugin-step-mix-metadata-enrichment)
Lizenz              | GPL 2.0 oder neuer 
Letzte Änderung    | 14.10.2024 10:52:57


## Einführung
Diese Dokumentation erläutert das Plugin zum MIX Metadata anreichern.

## Installation
Um das Plugin nutzen zu können, müssen folgende Dateien installiert werden:

```bash
/opt/digiverso/goobi/plugins/step/plugin-step-mix-metadata-enrichment-base.jar
/opt/digiverso/goobi/config/plugin_intranda_step_mix_metadata_enrichment.xml
/opt/digiverso/goobi/config/jhove/jhove.conf
```

Nach der Installation des Plugins kann dieses innerhalb des Workflows für die jeweiligen Arbeitsschritte ausgewählt und somit automatisch ausgeführt werden. Ein Workflow könnte dabei beispielhaft wie folgt aussehen:

![Beispielhafter Aufbau eines Workflows](images/goobi-plugin-step-mix-metadata-enrichment_screen1_de.png)

Für die Verwendung des Plugins muss dieses in einem Arbeitsschritt ausgewählt sein:

![Konfiguration des Arbeitsschritts für die Nutzung des Plugins](images/goobi-plugin-step-mix-metadata-enrichment_screen2_de.png)


## Überblick und Funktionsweise
Wenn das Plugin ausgeführt wird, werden alle Bilddateien in den konfigurierten Ordnern mit JHove analysiert und die technischen Metadaten im MIX Format extrahiert.
Diese technischen Metadaten werden dann in der Mets Datei des Vorgangs hinzugefügt und dort mit den jeweiligen Bilddateien verlinkt.


## Konfiguration
Die Konfiguration des Plugins erfolgt in der Datei `plugin_intranda_step_mix_metadata_enrichment.xml` wie hier aufgezeigt:

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
        <!-- which folder should be used for technical metadata extraction (results are also saved to derivatives in mets) -->
        <folder>master</folder>

        <!-- jhove configuration file path -->
        <jhoveConfig>/opt/digiverso/goobi/config/jhove/jhove.conf</jhoveConfig>
        <renameMappings>
            <value from="ImageCaptureMetadata/ScannerCapture/scannerManufacturer" to="ImageCaptureMetadata/DigitalCameraCapture/digitalCameraManufacturer" removeEmptyParents="true"/>
            <value from="ImageCaptureMetadata/ScannerCapture/ScannerModel/scannerModelName" to="ImageCaptureMetadata/DigitalCameraCapture/DigitalCameraModel/digitalCameraModelName" removeEmptyParents="true"/>
            <value from="ImageCaptureMetadata/ScannerCapture/ScannerModel/scannerModelNumber" to="ImageCaptureMetadata/DigitalCameraCapture/DigitalCameraModel/digitalCameraModelNumber" removeEmptyParents="true"/>
            <value from="ImageCaptureMetadata/ScannerCapture/ScannerModel/scannerModelSerialNo" to="ImageCaptureMetadata/DigitalCameraCapture/DigitalCameraModel/digitalCameraModelSerialNo" removeEmptyParents="true"/>
        </renameMappings>
        <extraMappings>
            <value source="//jhove:property[jhove:name='FNumber']//jhove:value[1]" target="ImageCaptureMetadata/DigitalCameraCapture/CameraCaptureSettings/ImageData/fNumber" transform="rational2real"/>
            <value source="//jhove:property[jhove:name='ExposureTime']//jhove:value[1]" target="ImageCaptureMetadata/DigitalCameraCapture/CameraCaptureSettings/ImageData/exposureTime" transform="rational2real"/>
            <value source="//jhove:property[jhove:name='ISOSpeedRatings']//jhove:value[1]" target="ImageCaptureMetadata/DigitalCameraCapture/CameraCaptureSettings/ImageData/isoSpeedRatings"/>
            <value source="//jhove:property[jhove:name='ShutterSpeedValue']//jhove:value[1]" target="ImageCaptureMetadata/DigitalCameraCapture/CameraCaptureSettings/ImageData/shutterSpeedValue" transform="rational2rationalType"/>
            <value source="//jhove:property[jhove:name='ApertureValue']//jhove:value[1]" target="ImageCaptureMetadata/DigitalCameraCapture/CameraCaptureSettings/ImageData/apertureValue" transform="rational2rationalType"/>
            <value source="//jhove:property[jhove:name='ExposureBiasValue']//jhove:value[1]" target="ImageCaptureMetadata/DigitalCameraCapture/CameraCaptureSettings/ImageData/exposureBiasValue" transform="rational2rationalType"/>
            <value source="//jhove:property[jhove:name='MaxApertureValue']//jhove:value[1]" target="ImageCaptureMetadata/DigitalCameraCapture/CameraCaptureSettings/ImageData/maxApertureValue" transform="rational2rationalType"/>
        </extraMappings>
    </config>

    <config>
        <project>*</project>
        <step>Custom Step</step>
        <folder>custom_folder</folder>

        <jhoveConfig>/opt/digiverso/goobi/config/jhove/jhove.conf</jhoveConfig>
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
------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
`folder`                | Angabe des Ordners, der von JHove analysiert werden soll um technische Metadaten zu extrahieren. <br /><br />Der konfigurierte Ordner wird verwendet, um die technischen Metadaten in Mets zu speichern. Es können durchaus `master` Bilder analysiert und Derivate dann um technische Metadaten ergänzt werden.                                                                      
`jhoveConfig`           | Der Pfad zur JHove Konfigurationsdatei. Eine Beispielkonfiguration liegt dem Plugin bei.                                                                                                                                                                                                                                                                                              
`renameMappings`        | In diesem Element können beliebig viele Umbenennungen in MIX definiert werden.<br /><br />Die Kindelemente müssen folgende Form haben: `<value from="a/b/c" to="d/e" removeEmptyParents="true\|false"/>`. Das Element `c`, welches in MIX in der Hierarchie `a/b/c` steht, wird in `e` als Kindelement von `d` umbenannt. Wenn `removeEmptyParents` auf `true` gesetzt ist, werden sowohl `b` als auch `a` entfernt, wenn sie keine weiteren Kindelemente haben.<br /><br />Das kann beispielsweise nützlich sein, wenn Daten in MIX vorhersehbar in den falschen Feldern stehen (Kamera wird als Scanner erkannt): `<value from="ImageCaptureMetadata/ScannerCapture/scannerManufacturer" to="ImageCaptureMetadata/DigitalCameraCapture/digitalCameraManufacturer" removeEmptyParents="true"/>`. 
`extraMappings`         | In diesem Element können beliebig viele MIX-Zusatzfelder definiert werden, die von JHove nicht automatisch korrekt erkannt werden.<br /><br />Die Kindelemente müssen folgende Form haben: `<value source="//some/xpath" target="a/b/c" transform="TRANSFORM"/>`. `source` enthält einen XPath Ausdruck zu einem Wert, der im JHove Ergebnis zu finden ist. `target` enthält den Pfad in MIX, wo der Wert gespeichert werden soll. `transform` kann optional angegeben werden, wenn eine Wertkonvertierung erforderlich ist. Es gibt aktuell zwei mögliche Konvertierung: `rational2real` und `rational2rationalType`. `rational2real` wandelt Brüche in Zahlen mit Punkt um (bspw. `1/4` zu `0.25`). `rational2rationalType` wandelt Brüche in einen speziellen MIX-Typen für Brüche um.<br /><br />Um zusätzlich die Blende zu speichern, könnte man sowas konfigurieren: `<value source="//jhove:property[jhove:name='FNumber']//jhove:value[1]" target="ImageCaptureMetadata/DigitalCameraCapture/CameraCaptureSettings/ImageData/fNumber" transform="rational2real"/>`.