---
description: >-
  Step Plugin for automatically updating values in METS files
---

# Update Metadata Fields

## Overview

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_step_metadata_update_field
Repository               | [https://github.com/intranda/goobi-plugin-step-metadata-update-field](https://github.com/intranda/goobi-plugin-step-metadata-update-field)
Licence              | GPL 2.0 or newer 
Last change    | 04.02.2025 11:00:59


## Introduction
This Step plugin for Goobi workflow allows to automatically create or update specific metadata fields inside of METS files. To do so it can use the Variable Replacer or neighbor metadata fields to write metadata to logical elements on all hiearchical levels.

## Installation
To be able to use the plugin, the following files must be installed:

```bash
/opt/digiverso/goobi/plugins/step/plugin-step-metadata-update-field-base.jar
/opt/digiverso/goobi/config/plugin_intranda_step_metadata_update_field.xml
```

After installing the plugin, it can be selected within the workflow for the respective work steps and thus executed automatically.

To use the plugin, it must be selected in a work step:

![Configuration of the workflow step for using the plugin](screen1_en.png)


## Overview and functionality
First, the values that the plugin is supposed to update must be defined in the configuration file. When the plugin is executed, it collects all relevant structural elements of the METS file. It then checks whether and how the specified values should be updated. If the conditions are met, either new values are inserted into empty fields, or existing values are overwritten if this is forced.


## Configuration
The plugin is configured in the file `plugin_intranda_step_metadata_update_field.xml` as shown here:

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
        
		<!-- multiple updates can happen within one call. 
        	Repeat the update blocks for each additional change -->
		<update>
			<!-- define for which field inside of the METS file the content shall be generated -->
			<field>TitleDocMain</field>
			
			<!-- for which structure elements shall the content be updated? 
				Multiple 'element' can be listed here. 
				Use '*' to match all structure element types. -->
			<element>Monograph</element>
			
			<!-- define if the content shall be overwritten if the field is not empty -->
			<forceUpdate>true</forceUpdate>
	
            <!-- optional: configure a condition. If enabled, the condition must match, otherwise the update will be skipped -->
            <!--
            <condition field="PublicationYear" value="\d[4}]" />
            -->
    
			<!-- define a list of content here to be used for the field as metadata value
				variable: this content gets analyzed and replaced by the variable replacer 
				metadata: value of the metadata field with the given name inside of the same docstruct element
				static: a static string
				random: a random number with a defined length 
				uuid: a UUID with 36 characters
				timestamp: a numeric timestamp 
                groupcounter: a separate counter for each value of 'groupField' -->
			<content type="variable">{meta.CatalogIDDigital}</content>
			<content type="metadata">DocLanguage</content>
			<content type="static">_</content>
			<content type="random">9</content>
			<content type="uuid" />
			<content type="timestamp" />
			<content type="counter">%03d</content>
            <content groupField="{meta.PublicationYear}" type="groupcounter">%03d</content>
            
            <!-- replace some texts with other texts. For leading, trailing or single whitespaces, use '\u0020' -->
            <!-- field is repeatable to allow multiple changes. The individual replacements are processed from top to bottom -->
           <replace value="/" replacement="-" />
		</update>

		<update>
			<field>DocLanguage</field>
			<element>Chapter</element>
			<forceUpdate>false</forceUpdate>
			<content type="variable">{meta.DocLanguage}</content>
            
		</update>

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
`<field>`      | Defines the field within the METS file for which the content should be generated. |
`<element>`    | Specifies the structural elements for which the content should be updated. Multiple element entries can be listed here. Use `*` to match all structural element types. |
`<forceUpdate>`| Indicates whether the content should be overwritten if the field is not empty. |
`variable`     | Analyzed and replaced by the variable replacer. |
`metadata`     | Uses the value of the metadata field with the given name inside the same structural element. |
`static`       | Uses a static string. |
`random`       | Generates a random number with a defined length. |
`uuid`         | Uses a UUID (Universally Unique Identifier) with 36 characters. |
`timestamp`    | Generates a numeric timestamp. |
`counter`      | Generates a sequential number that is automatically incremented. For example, using `%03d` will count as follows: `001`, `002`, `003`, etc. |
`groupcounter` | A separate counter for each value of the `groupField` is used as the content.  |
`<replace>`    | Allows text replacement, where specified texts are replaced with others. |
`<condition>`  | If this field exists, the replacement is only executed if the metadata configured there exists and corresponds to the expression specified in `value`. |