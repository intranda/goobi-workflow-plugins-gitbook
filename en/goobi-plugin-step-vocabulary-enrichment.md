---
description: >-
  Step Plugin for the Automatic Enrichment of Vocabulary
---

# Vocabulary enrichment

## Overview

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_step_vocabulary_enrichment
Repository               | [https://github.com/intranda/goobi-plugin-step-vocabulary-enrichment](https://github.com/intranda/goobi-plugin-step-vocabulary-enrichment)
Licence              | GPL 2.0 or newer 
Last change    | 04.09.2024 09:09:12


## Introduction
This documentation provides a brief overview of this plugin, which is used to enrich vocabularies based on metadata found in METS-MODS files within Goobi workflows.

## Installation
To be able to use the plugin, the following files must be installed:

```bash
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_vocabulary_enrichment.jar
/opt/digiverso/goobi/config/plugin_intranda_step_vocabulary_enrichment.xml
```

Once the plugin has been installed, it can be selected within the workflow for the respective work steps and thus executed automatically. 

## Overview and functionality
The program examines the metadata fields of a METS-MODS file from a Goobi process. For each `<item>` defined in the configuration file, the plugin will search the METS-MODS file for the specified metadata. The search can be restricted to the top struct of a document using the form `meta.topstruct.metadataName`. If one or more metadata are found, then the named vocabulary is searched. If the vocabulary does not contain an entry with field specifed by `target` which has a value equal to the value of the metadata found, then a new vocabulary entry is created with the `target` field given the value of the found metadata. If there are `generate` fields in the `<item>`, then the fields of the new vocabulary entry specified there are given the values defined in the `content` attributes.


## Configuration
The plugin is configured in the file `plugin_intranda_step_vocabulary_enrichment.xml` as shown here:

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

### General parameters 
The `<config>` block can occur repeatedly for different projects or work steps in order to be able to perform different actions within different workflows. The other parameters within this configuration file have the following meanings: 

| Parameter | Explanation | 
| :-------- | :---------- | 
| `project` | This parameter defines which project the current block `<config>` should apply to. The name of the project is used here. This parameter can occur several times per `<config>` block. | 
| `step` | This parameter controls which work steps the `<config>` block should apply to. The name of the work step is used here. This parameter can occur several times per `<config>` block. | 


### Further parameters 
In addition to these general parameters, the following parameters are available for further configuration: 


Parameter               | Explanation
------------------------|------------------------------------
|`item`| Each metadata which is to be added to a vocabulary is defined in an item. |
|`source`   |This defines the metadata field from which to take the vocabulary entry.    |
|`vocabulary`   | The name of the vocabulary to enrich.  |
|` target`  | The name of the field in the vocabulary which is to be given the value in the source. |
|`generate`   | These are optional fields: if they are specified, then each field in the vocabulary entry which is generated which specifed by the attribute `field` is given the value specified by the attribute `content`.  |