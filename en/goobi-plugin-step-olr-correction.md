---
description: >-
  Step plug-in for the correction of recognised tables of contents and the generation of PICA data records
---

# Correction of tables of contents after an OLR

## Overview

Name                     | Wert
-------------------------|-----------
Identifier               | plugin_intranda_step_olr-correction
Repository               | [https://github.com/intranda/goobi-plugin-step-olr-correction](https://github.com/intranda/goobi-plugin-step-olr-correction)
Licence              | GPL 2.0 or newer 
Last change    | 22.08.2024 07:32:36


## Introduction
This documentation describes the installation, configuration and use of the plugin.

## Installation

The program consists of these files:

``` bash
plugin_intranda_step_olr-correction.jar
plugin_intranda_step_olr-correction.xml
```

The file `plugin_intranda_step_olr-correction.jar` contains the program logic, and should be copied to this path: `/opt/digiverso/goobi/plugins/step`.

The file `plugin_intranda_step_olr-correction.xml` is the config file, and should be copied to the folder `/opt/digiverso/goobi/config/`.

fter the installation of the plugin, it can be selected in a work step.

![Configuration of the work step for using the plugin](images/goobi-plugin-step-olr-correction_screen1_en.png)

## Configuration
The configuration is done via the configuration file `plugin_intranda_step_olr-correction.xml` and can be adapted during operation. It is structured as follows:

```xml
<config_plugin>
    <config>
        <!-- which projects to use for (can be more then one, otherwise use *) -->
        <project>*</project>
        <step>*</step>
        <!-- which images to use -->
        <useOrigFolder>true</useOrigFolder>
        
         <!-- Are the files originally digital rather than scanned? -->
        <bornDigital>true</bornDigital>      
        
        <!-- which image sizes to use for the big image -->
        <imagesize>800</imagesize>
        <imagesize>3000</imagesize>
        <!-- which image format to generate -->
        <imageFormat>jpg</imageFormat>
        
        <!-- colors for different groups -->
        <class>
            <type>pagenum</type>
            <color>#00FFFA</color>
        </class>
        <class>
            <type>title</type>
            <color>#FF5D15</color>
        </class>
        <class>
            <type>author</type>
            <color>blue</color>
        </class>
        <class>
            <type>institution</type>
            <color>#2F8C3C</color>
        </class>
        <class>
            <type>entry</type>
            <color>grey</color>
        </class>        
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


Parameter               | Erläuterung
------------------------|------------------------------------
 `bornDigital`          | If this is true, the files are assumed to be born digital and not scanned files.|