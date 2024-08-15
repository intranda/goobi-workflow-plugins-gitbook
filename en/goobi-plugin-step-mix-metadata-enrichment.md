---
description: >-
  This Step plugin for Goobi workflow uses JHove to extract technical metadata from files and save the results into the METS file of a Goobi process.
---

# MIX Metadata Enrichment Plugin

## Overview

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_step_mix_metadata_enrichment
Repository               | [https://github.com/intranda/goobi-plugin-step-mix-metadata-enrichment](https://github.com/intranda/goobi-plugin-step-mix-metadata-enrichment)
Licence              | GPL 2.0 or newer 
Last change    | 15.08.2024 17:54:49


## Introduction
This documentation explains the plugin for enriching MIX Metadata.

## Installation
To be able to use the plugin, the following files must be installed:

```bash
/opt/digiverso/goobi/plugins/step/plugin-step-mix-metadata-enrichment-base.jar
/opt/digiverso/goobi/config/plugin_intranda_step_mix_metadata_enrichment.xml
/opt/digiverso/goobi/config/jhove/jhove.conf
```

Once the plugin has been installed, it can be selected within the workflow for the respective work steps and thus executed automatically. A workflow could look like the following example:

![Example of a workflow structure](images/goobi-plugin-step-mix-metadata-enrichment_screen1_en.png)

To use the plugin, it must be selected in a workflow step:

![Configuration of the workflow step for using the plugin](images/goobi-plugin-step-mix-metadata-enrichment_screen2_en.png)


## Overview and functionality
When the plugin is executed, all image files in the configured folders are analyzed with JHove and the technical metadata is extracted in MIX format.
This technical metadata is then added to the Mets file of the process and linked there to the respective image files.


## Configuration
The plugin is configured in the file `plugin_intranda_step_mix_metadata_enrichment.xml` as shown here:

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
        <folder>*</folder>

        <!-- jhove configuration file path -->
        <jhoveConfig>/opt/digiverso/goobi/config/jhove/jhove.conf</jhoveConfig>
    </config>

    <config>
        <project>*</project>
        <step>Custom Step</step>
        <folder>custom_folder</folder>

        <jhoveConfig>/opt/digiverso/goobi/config/jhove/jhove.conf</jhoveConfig>
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
`folder`                | Specifies the folder to be analyzed by JHove to extract technical metadata. <br /><br />Multiple folders can be specified by repeating the `<folder>` element. The value `*` can be used to select all default folders.
`jhoveConfig`           | The path to the JHove configuration file. A sample configuration is included with the plugin.