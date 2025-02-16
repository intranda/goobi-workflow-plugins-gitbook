---
description: >-
  Step Plugin for the automatic enrichment of METS files with the associated media files 
---

# Enrich METS file

## Overview

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_step_mets_enhancer
Repository               | [https://github.com/intranda/goobi-plugin-step-mets-enhancer](https://github.com/intranda/goobi-plugin-step-mets-enhancer)
Licence              | GPL 2.0 or newer 
Last change    | 16.02.2025 19:52:46


## Introduction
This documentation explains the Step plugin for Goobi workflow for the automatic enrichment of METS files with the associated media files. Depending on the configuration, pagination can also be generated automatically and additional metadata added.

## Installation
To use the plugin, the following files must be installed:

```bash
/opt/digiverso/goobi/plugins/plugin-step-mets-enhancer-base.jar
/opt/digiverso/goobi/config/plugin-step-mets-enhancer-base.xml
```

Once the plugin has been installed, it can be selected within the workflow for the respective workflow steps and thus executed automatically.

To use the plugin, it must be selected in a workflow step as follows:

![Configuration of the workflow step for using the plugin](images/goobi-plugin-step-mets-enhancer_screen2_en.png)


## Overview and functionality
The plugin is usually used if it is not intended to work manually on the METS file within the workflow and to publish it automatically. In this case, a workflow could look like this, for example:

![Automatic enrichment of the METS file in the workflow before the first export](images/goobi-plugin-step-mets-enhancer_screen1_en.png)

In this case, the METS file is automatically opened by the plugin and enriched as configured. Firstly, all media files from the file system of the process are reassigned to the topmost structural element. Depending on the configuration, different types of pagination can also be created if required. The configuration can also be used to automatically add further metadata for the top-level structure element.

## Configuration
The plugin is configured in the file `plugin_intranda_step_mets_enhancer.xml` as shown here:

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

		<!-- if a pagination shall be automatically created activate this and use one of these types:
        	- uncounted
        	- ROMAN
        	- roman
        	- arabic
        -->
		<createPagination type="arabic">true</createPagination>

		<!-- list of metadata to be added to the topstruct element -->
		<addMetadata type="singleDigCollection" value="Maps" />

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
| `<createPagination>`   | The value ‘true’ can be used here to specify whether pagination should be created automatically. This can also be defined using the ‘type’ attribute. The following types are available for pagination: `uncounted`, `roman`, `ROMAN` or `arabic` |
| `<addMetadata>`        | Further metadata to be added to the top-level structural element can be defined in the configuration here. The `type` attribute defines the desired metadata type as it is available in the rule set, and the `value` attribute is used to specify the value to be used. This element is repeatable. |