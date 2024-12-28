---
description: >-
  This step plugin allows you to validate images using JHove and customize the workflow
---

# Tif-Validation

## Overview

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_step_jhove-validation
Repository               | [https://github.com/intranda/goobi-plugin-step-tif-validation](https://github.com/intranda/goobi-plugin-step-tif-validation)
Licence              | GPL 2.0 or newer 
Last change    | 04.12.2024 10:01:16


## Introduction
This plugin is used to validate images in `TIF` format within defined directories. The validation is done with the help of the [open source software library JHove](https://jhove.openpreservation.org/) and is extensively configurable.


## Installation
To install the plugin, the following file must be installed first:

```bash
/opt/digiverso/goobi/plugins/step/plugin_intranda_step_tif_validation-base.jar
```

To configure how the plugin should behave, different values can be adjusted in the configuration file. The central configuration file is usually located here:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_tif_validation.xml
```

Within this configuration file the path to the JHove configuration is named. In the case of the example below, the following path is given:

```xml
<jhoveConfiguration>/opt/digiverso/goobi/config/jhove/jhove.conf</jhoveConfiguration>
```

Accordingly, the following two files must also be installed under this path:

```xml
/opt/digiverso/goobi/config/jhove/jhove.conf
/opt/digiverso/goobi/config/jhove/jhoveConfig.xsd
```


## Overview and functionality
The plugin is usually executed fully automatically within the workflow. It first determines whether there is a block within the configuration file that has been configured for the current workflow with respect to project name and work step. If this is the case, the other parameters are evaluated and the checks are started. If one of the configured checks is not successful, the configured or alternatively the previous work step is set to an error status and the validation message is written to the process log. If the work steps between the validation step and the notified step are to be set in the status to closed, these steps are also provided with the correction message for the agents and thus allow the problem case to be traced.

This plugin is integrated into the workflow in such a way that it is executed automatically. Manual interaction with the plugin is not necessary. For use within a step of the workflow it should be configured as shown in the following screenshot.

![Integration of the plugin into the workflow](images/goobi-plugin-step-tif-validation_screen1.png)


## Configuration
The configuration for the plugin is done within the central configuration file. It looks like the following example:

```xml
<config_plugin>
    <config>
        <project>*</project>
        <step>*</step>
        <!-- folders to validate, can be multiple one (e.g. master, main etc. -->
        <folder>master</folder>
        <openStepOnError>Scanning</openStepOnError>
        <lockAllStepsBetween>true</lockAllStepsBetween>
        <jhoveConfiguration>/opt/digiverso/goobi/config/jhove/jhove.conf</jhoveConfiguration>
        <namespace uri="http://www.loc.gov/mix/v20" name="mix" />
        <namespace uri="http://schema.openpreservation.org/ois/xml/ns/jhove" name="jhove" />

        <!-- format is 'TIFF' -->
        <check>
            <xpath>string(//jhove:repInfo/jhove:format)</xpath>
            <wanted>TIFF</wanted>
            <error_message> Check format for "${image}": Expected value "${expected}", but found value "${found}".</error_message>
        </check>

        <!-- image is well-formed and valid-->
        <check>
            <xpath>//jhove:repInfo/jhove:status</xpath>
            <wanted value="Well-Formed and valid"></wanted>
            <error_message> Check status for "${image}": Expected value "${expected}", but found value "${found}".</error_message>
        </check> 
        
        <!-- compression type is 'Uncompressed' -->
        <check>
            <xpath>//mix:Compression/mix:compressionScheme</xpath>
            <wanted>Uncompressed</wanted>
            <error_message>Check compression scheme for "${image}": Expected value "${expected}", but found value "${found}".</error_message>
        </check>

        <!-- planar configuration -->
        <check>
            <xpath>//jhove:property[jhove:name='PlanarConfiguration']/jhove:values/jhove:value</xpath>
            <wanted>1</wanted>
            <error_message>Check planar configuration for "${image}": Expected value "${expected}", but found value "${found}".</error_message>
        </check>

        <!-- byte order -->
        <check>
            <xpath>//jhove:property[jhove:name='ByteOrder']/jhove:values/jhove:value</xpath>
            <wanted>little-endian</wanted>
            <error_message>Check compression scheme for "${image}": Expected value "${expected}", but found value "${found}".</error_message>
        </check>
        
        <!-- bit order is MSB-to-LSB  -->
        <check>
            <xpath>//jhove:property[jhove:name='FillOrder']/jhove:values/jhove:value</xpath>
            <wanted>1</wanted>
            <error_message>Check compression scheme for "${image}": Expected value "${expected}", but found value "${found}".</error_message>
        </check>

        <!-- color space -->
        <check>
            <xpath>//mix:PhotometricInterpretation/mix:colorSpace</xpath>
            <wanted>RGB</wanted>
            <error_message>Check compression scheme for "${image}": Expected value "${expected}", but found value "${found}".</error_message>
        </check>

        <!--Check color depth, allowed is 8,8,8 or 16,16,16-->
        <check>
            <xpath>string(//mix:samplesPerPixel[1])</xpath>
            <wanted>3</wanted>
            <error_message> Check color depth for "${image}": Expected value "${expected}", but found value "${found}".</error_message>
        </check>
        <check>
            <xpath>string(//mix:bitsPerSampleValue[1])</xpath>
            <wanted>8|16</wanted>
            <error_message> Check color depth for "${image}": Expected value is 8 or 16 but found value "${found}".</error_message>
        </check>
        <check>
            <xpath>string(//mix:bitsPerSampleValue[2])</xpath>
            <wanted>8|16</wanted>
            <error_message> Check color depth for "${image}": Expected value is 8 or 16 but found value "${found}".</error_message>
        </check>
        <check>
            <xpath>string(//mix:bitsPerSampleValue[2])</xpath>
            <wanted>8|16</wanted>
            <error_message> Check color depth for "${image}": Expected value is 8 or 16 but found value "${found}".</error_message>
        </check>

        <!-- Resolution checks -->

        <!-- values exist -->
        <check>
            <xpath>//mix:ImageAssessmentMetadata//mix:SpatialMetrics/mix:xSamplingFrequency/mix:numerator</xpath>
            <wanted>exists</wanted>
            <error_message>Check x-axis resolution": value not found".</error_message>
            <checkType>exists</checkType>
        </check>

        <check>
            <xpath>//mix:ImageAssessmentMetadata//mix:SpatialMetrics/mix:ySamplingFrequency/mix:numerator</xpath>
            <wanted>exists</wanted>
            <error_message>Check y-axis resolution": value not found".</error_message>
            <checkType>exists</checkType>
        </check>
        <check>
            <xpath>//mix:ImageAssessmentMetadata//mix:SpatialMetrics/mix:samplingFrequencyUnit</xpath>
            <wanted>exists</wanted>
            <error_message>Check resolution unit": value not found".</error_message>
            <checkType>exists</checkType>
        </check>        
        <!-- values are identical -->
        <check>
            <xpath>//mix:ImageAssessmentMetadata//mix:SpatialMetrics/mix:xSamplingFrequency/mix:numerator</xpath>
            <wanted></wanted>
            <error_message> Check resolution for "${image}": X- and Y-resolution differ.</error_message>
            <checkType>same</checkType>
            <otherXpath>//mix:ImageAssessmentMetadata//mix:SpatialMetrics/mix:ySamplingFrequency/mix:numerator</otherXpath>
        </check>
        <!-- at least 300 dpi -->
        <check>
            <xpath>//mix:ImageAssessmentMetadata//mix:SpatialMetrics/mix:xSamplingFrequency/mix:numerator</xpath>
            <wanted>300</wanted>
            <error_message>Check x-axis resolution": Expected value at least "${expected}", but found value "${found}".</error_message>
            <checkType>greater</checkType>
        </check>
        <check>
            <xpath>//mix:ImageAssessmentMetadata//mix:SpatialMetrics/mix:ySamplingFrequency/mix:numerator</xpath>
            <wanted>300</wanted>
            <error_message>Check y-axis resolution": Expected value at least "${expected}", but found value "${found}"".</error_message>
            <checkType>greater</checkType>
        </check>

        <!-- get expected resolution from process property -->
        <integrated_check name="resolution_check">
            <mix_uri>http://www.loc.gov/mix/v20</mix_uri>
            <!-- get the value from the property 'Resolution' -->
            <wanted value="{process.Resolution}">
            <!-- only when 'Resolution' contains a numeric value -->
                <condition value="{process.Resolution}" matches="^\d+" />
            </wanted>
            <!-- get the value from a different field, if 'Resolution' contains the value 'other' --> 
            <wanted value="{process.Special Resolution}">
                <condition value="{process.Resolution}" matches="other" />
            </wanted>
            <!-- default configuration that is used if none of the above conditions apply. 
            If no default configuration exists and no condition applies, the check is not executed.
            Can be a fixed value or a range. -->
            <wanted>100.0-899.23</wanted>
            <error_message> Check resolution for "${image}": Expected value "${expected}", but found value "${found}".</error_message>
        </integrated_check>

        <!-- resolution values for x-axis and y-axis are the same-->
        <integrated_check name="resolution_check">
            <mix_uri>http://www.loc.gov/mix/v20</mix_uri>
            <wanted>1</wanted>
            <error_message> Check resolution for "${image}": x-axis and y-axis differ.</error_message>
            <checkType>same</checkType>
        </integrated_check>

        <!-- resolution is at least 300 dpi-->
        <integrated_check name="resolution_check">
            <mix_uri>http://www.loc.gov/mix/v20</mix_uri>
            <wanted>300</wanted>
            <error_message> Check resolution for "${image}": must be at least "${expected}" dpi, but found value "${found}".</error_message>
            <checkType>greater</checkType>
        </integrated_check>

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


Parameter         | Explanation
------------------|----------------------------------------
`folder` | This parameter can be used to specify directories whose contents are to be validated. This parameter can occur repeatedly. Possible values are `master`, `media` or individual folders like `photos` and `scans`.
`openStepOnError` | This parameter determines which step of the workflow should be reopened if an error occurs within the validation. If this parameter is not used, the plugin simply activates the previous step of the validation step instead.
`lockAllStepsBetween` | This parameter is used to determine whether the work steps of the workflow between the validation step and the one specified within the parameter openStepOnError are to be set back to the status locked so that these work steps have to be run through again (`true`). If, on the other hand, the value is set to `false`, the status of the steps in between is not changed, so that these steps are not run through again.
`jhoveConfiguration` | This parameter specifies where the configuration file for JHove is located. 
`check` | Within each element check is defined what exactly JHove should validate. For example, here you define which file format is expected. For the expected value, a direct input can also be entered as a range within the `<wanted>` element. It is also possible to use a variable here, which is replaced by the variable replacer (e.g. `{process.Resolution}`. Also included is which error message should be issued in case of an incorrect validation. The following variables can be used in the error message: `${wanted}` for the exact content from the `<wanted>` field, `${expected}` for the resolved expected value, `${found}` for the found value and `${image}` for the file name. The field `<wanted>` can be repeated and can contain the sub-element `<condition>`. The check is then only executed if the configured condition applies. The `<checkType>` field can be used to specify how exactly the check is to be carried out. The default value is `equals`, the value found must match the configured value exactly. The `multiple` option checks whether the value found is a multiple of the configured number. With `lesser` or `greater`, the value found must be greater or smaller than the configured value, `exists` and `not exists` checks for the existence of any value and `same` checks whether the value matches another value. To do this, the other value must be specified in `otherXpath`.