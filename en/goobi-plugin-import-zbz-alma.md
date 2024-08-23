---
description: >-
  This import plugin for Goobi workflow allows data to be imported with a subsequent catalogue query from ALMA, as required for the Zentralbibliothek Zürich.
---

# Data import with ALMA catalogue query for Zurich Central Library

## Overview

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_import_zbz_alma
Repository               | [https://github.com/intranda/goobi-plugin-import-zbz-alma](https://github.com/intranda/goobi-plugin-import-zbz-alma)
Licence              | GPL 2.0 or newer 
Last change    | 23.08.2024 11:12:32


## Introduction
This import plugin allows you to import data with an ALMA catalogue query. Data that was previously copied from an Excel file is inserted into the user interface.

## Installation
To be able to use the plugin, the following files must be installed:

```bash
/opt/digiverso/goobi/plugins/import/plugin-import-zbz-alma-base.jar
/opt/digiverso/goobi/config/plugin_intranda_import_zbz_alma.xml
```

Once the plugin has been installed, it can be accessed from the overview of production templates by using the second blue button next to the selected production template.

![Production template with additional blue button for mass import](images/goobi-plugin-import-zbz-alma_screen1_en.png)

Once the plugin has been entered, a user interface is available in which the data to be imported can be selected or uploaded.

![User interface of the import plugin](images/goobi-plugin-import-zbz-alma_screen2_en.png)


## Overview and functionality
After selecting the correct plugin, the data, which is either available as TAB-separated CSV data or copied from an Excel file, can be inserted into the `Records` field in the user interface. The data has the following structure:

Column    | Metadata        | Explanation
----------|-----------------|-------------------------
`1`       | `MMS-ID`        | If this contains an underscore, a multi-volume work is created, otherwise a monograph. This is a mandatory entry.

Immediately after inserting the data and clicking on `Save`, the creation of the processes starts without a catalogue being requested.


## Configuration
The plugin is configured in the file `plugin_intranda_import_zbz_alma.xml` as shown here:

```xml
<config_plugin>
	<config>

		<!-- which workflow template shall be used -->
		<template>*</template>

		<!-- define if import shall use GoobiScript to run in the background -->
		<runAsGoobiScript>true</runAsGoobiScript>
		
		<!-- name of the catalogue -->
		<catalogue>ZB-ALMA</catalogue>
		<searchField>alma.mms_id</searchField>

	</config>
</config_plugin>

```

The following table contains a summary of the parameters and their descriptions:

Parameter               | Explanation
------------------------|------------------------------------
`template`              | This can be used to define which production template the respective `config` block should apply to. 
`runAsGoobiScript`      | This parameter can be used to specify whether the import should take place as GoobiScript in the background.
`catalogue`             | The catalogue to be used for the query is defined here. This must be defined within the configuration file `goobi_opac.xml`.
`searchField`           | This parameter defines in which field of the catalogue the search for the identifier should take place.