---
description: >-
  Step Plugin for reversing the order of the images
---

# Reverse Image Order

## Overview

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_step_reverse_image-order
Repository               | [https://github.com/intranda/goobi-plugin-step-reverse-image-order](https://github.com/intranda/goobi-plugin-step-reverse-image-order)
Licence              | GPL 2.0 or newer 
Last change    | 22.08.2024 12:00:30


## Introduction
This step plugin for the Goobi workflow reverses the file order of images in the Goobi workflow based on the metadata field `DocLanguage` .

## Installation
To be able to use the plugin, the following files must be installed:

```bash
/opt/digiverso/goobi/plugins/step/plugin-step-reverse-image-order-base.jar
/opt/digiverso/goobi/config/plugin_intranda_step_reverseImageOrder.xml
```

To use the plugin, it must be selected in a workflow step:

![Configuration of the workflow step for using the plugin](images/goobi-plugin-step-reverse-image-order_screen1_en.png)


## Overview and functionality
The plugin compares all languages present in the document with the languages specified in the configuration file. If a match is found, the images are reversed in order.

## Configuration
The plugin is configured in the file `plugin_intranda_step_reverseImageOrderPlugin.xml` as shown here:

```xml
<!-- * This file is part of a plugin for Goobi - a Workflow tool for the 
	support of mass digitization. * * Visit the websites for more information. 
	* - https://goobi.io * - https://www.intranda.com * - https://github.com/intranda/goobi 
	* * This program is free software; you can redistribute it and/or modify 
	it under the terms of the GNU General Public License as published by the 
	Free * Software Foundation; either version 2 of the License, or (at your 
	option) any later version. * * This program is distributed in the hope that 
	it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty 
	of MERCHANTABILITY or * FITNESS FOR A PARTICULAR PURPOSE. See the GNU General 
	Public License for more details. * * You should have received a copy of the 
	GNU General Public License along with this program; if not, write to the 
	Free Software Foundation, Inc., 59 * Temple Place, Suite 330, Boston, MA 
	02111-1307 USA * -->
<config_plugin>
	<!-- order of configuration is: 1.) project name and step name matches 2.) 
		step name matches and project is * 3.) project name matches and step name 
		is * 4.) project name and step name are * -->
	<config>
		<!-- which projects to use for (can be more then one, otherwise use *) -->
		<project>*</project>
		<step>*</step>

		<language>heb</language>
		<language>yid</language>
		<language>lad</language>
		<language>jpr</language>
		<language>jbe</language>
		<language>jrb</language>
		<language>jge</language>
		<language>itk</language>
		<language>yhd</language>
		<language>aju</language>
		<language>yud</language>
		<language>ajt</language>
		<language>jye</language>
		<language>jdt</language>
		<language>jtgk</language>
		<language>jger</language>
		<language>jgrc</language>
		<language>narc</language>
	</config>

	<config>
		<!-- which projects to use for (can be more then one, otherwise use *) -->
		<project>My special project</project>
		<project>Archive_Project</project>
		<step>OCR</step>

		<language>heb</language>
		<language>yid</language>
		<language>lad</language>
		<language>jpr</language>
		<language>jbe</language>
		<language>jrb</language>
		<language>jge</language>
		<language>itk</language>
		<language>yhd</language>
		<language>aju</language>
		<language>yud</language>
		<language>ajt</language>

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
`language`              | This parameter specifies a list of languages for which the image order in the document should be reversed. The languages are given as ISO language codes (e.g., "he" for Hebrew, "ar" for Arabic). |