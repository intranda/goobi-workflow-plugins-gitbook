---
description: >-
  Step-Plugin zum Anreichern von ACTApro-Dokumenten mit zusätzlichen Metadaten
---

# Anreichern von ACTApro Dokumenten

## Übersicht

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_step_actapro
Repository               | [https://github.com/intranda/goobi-plugin-step-actapro](https://github.com/intranda/goobi-plugin-step-actapro)
Lizenz              | GPL 2.0 oder neuer 
Letzte Änderung    | 30.05.2025 19:22:44


## Einführung

Dieses Plugin wird verwendet, um Informationen an ACTApro zu senden. Beliebige Daten können hierbei zu einem existierenden Knoten innerhalb von ACTApro hinzugefügt werden.


## Installation
Um das Plugin nutzen zu können, müssen folgende Dateien installiert werden:

```bash
/opt/digiverso/goobi/plugins/step/plugin-step-actapro-base.jar
/opt/digiverso/goobi/config/plugin_intranda_step_actapro.xml
```

Außerdem muss das `intranda_administration_actapro_sync` Administraion Plugin installiert und konfiguriert werden.

Nach der Installation des Plugins kann dieses innerhalb des Workflows für die jeweiligen Arbeitsschritte ausgewählt und somit automatisch ausgeführt werden. Ein Workflow könnte dabei beispielhaft wie folgt aussehen:

![Beispielhafter Aufbau eines Workflows](images/goobi-plugin-step-actapro_screen1_de.png)

Für die Verwendung des Plugins muss dieses in einem Arbeitsschritt ausgewählt sein:

![Konfiguration des Arbeitsschritts für die Nutzung des Plugins](images/goobi-plugin-step-actapro_screen2_de.png)


## Überblick und Funktionsweise

Wenn das Plugin ausgeführt wird, werden als erstes die Metadaten gelesen und das Feld gesucht, in dem die ID des ACTApro Datensatzes enthalten ist.

Anschließend werden die konfigurierten Pflichtfelder geprüft. Hier kann validiert werden, ob Metadaten oder Eigenschaften existieren. 

Wenn die Vorbedingungen erfüllt wurden, wird der ACTApro-Datensatz via REST API geholt und um die konfigurierten Daten angereichert.
Das so angereicherte Dokument wird als letztes wieder an die ACTApro API geschickt.

Fehlt die ACTApro ID, die konfigurierten Pflichtfelder oder wird der ACTApro-Datensatz nicht gefunden, schlägt das Plugin mit einer Fehlermeldung im Journal fehl.


## Konfiguration
Die Konfiguration des Plugins erfolgt in der Datei `plugin_intranda_step_actapro.xml` wie hier aufgezeigt:

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

        <actaProIdFieldName>RecordID</actaProIdFieldName>

        <!-- required metadata/property that must exist in order to proceed -->
        <requiredField type="metadata">CatalogIDDigital</requiredField>
        <requiredField type="property">DocType</requiredField>

        
        <!-- define which field to update/add-->
        <!-- type defines the type field in the actapro document -->
        <!-- if it is a sub field of a superordinated field, in can be configured in groupType -->
        <!-- value: the value to write. can be a static text or anything from the VariableReplacer -->
        
        <!--  
        two types of fields can be generated. You can generate something like this:  
         "fields": [
              {
                "type": "some type",
                "value": "http://example.com/viewer/piresolver?id=12345"
               }
               ]
       -->
       <field type="some type" value="http://example.com/viewer/piresolver?id=$(meta.CatalogIDDigital)" />
       
       <!-- or create a structure like this: -->
        <!--
         "fields": [
               {
                    "type": "main type",
                    "fields": [
                      {
                        "type": "sub type",
                        "value": "http://example.com/viewer/piresolver?id=12345"
                      }
                    ]
                  }
               ]       
       -->           
        <field type="sub type" groupType="main type" value="http://example.com/viewer/piresolver?id=$(meta.CatalogIDDigital)" />



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
`actaProIdFieldName`    | Enthält den Namen des Metadatums, in dem die ACTApro ID steht
`requiredField`         | Enthält eine Liste aller Pflichtfelder. Im Attribut `type` kann angegeben werden, ob es sich um eine Eigenschaft (`property`) oder um ein Metadatum (`metadata`) handelt. 
`field`                 | Enthält eine Felddefinition, die im ACTApro Dokument überschrieben oder hinzugefügt werden soll. Das Attribut `value` enthält den zu schreibenden Wert. Hierbei kann auf den VariableReplacer zugegriffen werden. In `type` steht der Feldname. Falls es sich um ein untergeordnetes Feld eines anderen Feldes handelt, kann das Hauptfeld in `groupType` angegeben werden.