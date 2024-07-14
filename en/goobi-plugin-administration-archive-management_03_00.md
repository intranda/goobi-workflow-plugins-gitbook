# Configuration of the plugin

## Overview

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_administration_archive_management
Repository               | [https://github.com/intranda/goobi-plugin-administration-archive-management](https://github.com/intranda/goobi-plugin-administration-archive-management)
Licence              | GPL 2.0 or newer 
Last change    | 14.07.2024 11:04:38


After installing the plugin and the associated database, the plugin must also be configured. This takes place within the configuration file `plugin_intranda_administration_archive_management.xml` and is structured as follows:

```xml
<config_plugin>

    <basexUrl>http://localhost:8984/</basexUrl>
    <eadExportFolder>/opt/digiverso/basex/import</eadExportFolder>
    <config>
        <!-- define the name(s) of all archives for the plugin -->
        <archive>*</archive>
        <!-- default title for a new node -->
        <nodeDefaultTitle>Document</nodeDefaultTitle>
        
        <!-- configurations for generating process titles -->
        
        <!-- maximum length of the body token that will be used to generate a new process title -->
        <!-- the specifically set HEAD token and TAIL token will not be affected by this limit -->
        <!-- if the limit is positively configured, then CAMEL_CASE_LENGTH_LIMITED will be applied upon every body token, otherwise CAMEL_CASE will be applied -->
        <lengthLimit>0</lengthLimit>
        <!-- separator string that will be used to combine the tokens -->
        <separator>_</separator>
        
        <!-- use id from parent node instead of id from node -->
        <useIdFromParent>false</useIdFromParent>
        
        <!-- Optional title generation, if nothing is configured, process titles are built based on the node ID -->
        <!-- attribute value: contains a static text. If it is empty, it is assumed that a metadata value is being searched for -->
        <!-- attribute name: contains the metadata name to use -->
        <!-- attribute type: can be NORMAL (use text as it is), CAMEL_CASE (each word begins with an upper case letter), 
             AFTER_LAST_SEPARATOR (insert at the end), BEFORE_FIRST_SEPARATOR (insert in front) -->
        
        <!-- 
        <title name="shelfmarksource" type="NORMAL"    />
        <title name="static" type="CAMEL_CASE" value="STATIC TEXT"  />
        <title name="CatalogIDDigital" type="AFTER_LAST_SEPARATOR"    />
        -->
        
        <!-- // configurations for generating process titles -->
        
        <!-- define metadata fields. All fields are displayed on the UI based on the level and the order within this file.
                - @name: contains the internal name of the field. The value can be used to translate the field in the messages files. The field must start with a letter and can not contain any white spaces.
                - @level: metadata level, allowed values are 1-7:
                    * 1: metadata for Identity Statement Area 
                    * 2: Context Area 
                    * 3: Content and Structure Area
                    * 4: Condition of Access and Use Area
                    * 5: Allied Materials Area
                    * 6: Note Area
                    * 7: Description Control Area
                - @xpath: contains a relative path to the ead value. The root of the xpath is either the <ead> element or the <c> element
                - @xpathType: type of the xpath return value, can be text, attribute, element (default)
                - @repeatable: defines if the field can exist once or multiple times, values can be true/false, default is false
                - @visible: defines if the field is displayed on the UI, values can be true/false, default is true
                - @showField: defines if the field is displayed as input field (true) or badge (false, default), affects only visible metadata
                - @fieldType: defines the type of the input field. Posible values are input (default), textarea, dropdown, multiselect, vocabulary
                - @rulesetName: internal name of the metadata in ruleset. If missing or empty, field is not imported into process metadata
                - @importMetadataInChild: defines if the field is imported or skipped in processes for child elements 
                - @validationType: defines a validation rule, allowed values are unique, required, unique+required, regex, regex+required, regex+unique, regex+unique+required
                - @regularExpression defines a regular expression that gets used for validation type regex
                - validationError: message to display in case of validation errors
                - value: list of possible values for dropdown and multiselect lists
                - vocabulary: name of the vocabulary
                - searchParameter: distinct the vocabulary list by the given condition. Syntax is fieldname=value, field is repeatable
         -->

        <!-- internal fields, not visible on the UI -->
        <metadata xpath="./ead:eadheader[@countryencoding='iso3166-1'][@dateencoding='iso8601'][@langencoding='iso639-2b'][@repositoryencoding='iso15511'][@scriptencoding='iso15924']/ead:eadid/@mainagencycode" xpathType="attribute" name="mainagencycode" level="1" repeatable="false" visible="false"/>
        <metadata xpath="./ead:eadheader/ead:profiledesc/ead:creation/@normal" xpathType="attribute" name="normalcreationdate" level="1" repeatable="false" visible="false"/>
        <metadata xpath="./ead:eadheader/ead:profiledesc/ead:creation" xpathType="element" name="creationdate" level="1" repeatable="false" visible="false"/>

        <!--  Identity Statement Area -->
        <metadata xpath="./ead:control/ead:maintenanceagency/ead:agencycode" xpathType="element" name="agencycode" level="1" repeatable="false" fieldType="input"/>

        <metadata xpath="./ead:eadheader/ead:eadid" xpathType="element" name="eadid" level="1" repeatable="false" showField="false" fieldType="input" rulesetName="EADID"/>
 
        <metadata xpath="./ead:control/ead:recordid" xpathType="element" name="recordid" level="1" repeatable="false" fieldType="input" rulesetName="RecordID"/>
        <metadata xpath="(./ead:archdesc/ead:did/ead:unitid[not(@type)] | ./ead:did/ead:unitid[not(@type)])[1]" xpathType="element" name="unitid" level="1" repeatable="false" showField="false" fieldType="input" rulesetName="UnitID"/>

        <metadata xpath="./ead:did/ead:unitid[@type='Vorl. Nr.']" xpathType="element" name="Number" level="1" repeatable="true" />
        <metadata xpath="./ead:did/ead:unitid[@type='Altsignatur']" xpathType="element" name="Shelfmark" level="1" repeatable="true" rulesetName="shelfmarksource" validationType="unique">
            <validationError>Der Wert wurde an anderer Stelle bereits verwendet</validationError>
        </metadata>
        <metadata xpath="(./ead:archdesc/ead:did/ead:unittitle | ./ead:did/ead:unittitle)[1]" xpathType="element" name="unittitle" level="1" repeatable="false" fieldType="textarea" rulesetName="TitleDocMain" importMetadataInChild="false" />
        <metadata xpath="(./ead:archdesc/ead:did/ead:unitdate | ./ead:did/ead:unitdate)[1]" xpathType="element" name="unitdate" level="1" repeatable="false" rulesetName="PublicationYear" importMetadataInChild="false" regularExpression="\\d{4}" validationType="regex">
            <validationError>Der Wert ist keine vierstellige Jahreszahl</validationError>
        </metadata>
        <metadata xpath="(./ead:archdesc/ead:did/ead:unitdatestructured | ./ead:did/ead:unitdatestructured)[1]" xpathType="element" name="unitdatestructured" level="1" repeatable="false"  rulesetName="DateOfOrigin"/>
        <metadata xpath="(./ead:archdesc/@level | ./@level)[1]" xpathType="attribute" name="descriptionLevel" level="1" repeatable="false" fieldType="dropdown">
            <value>collection</value>
            <value>fonds</value>
            <value>class</value>
            <value>recordgrp</value>
            <value>series</value>
            <value>subfonds</value>
            <value>subgrp</value>
            <value>subseries</value>
            <value>file</value>
            <value>item</value>
            <value>otherlevel</value>
        </metadata>


        <metadata xpath="(./ead:archdesc/ead:did/ead:physdesc | ./ead:did/ead:physdesc)[1]" xpathType="element" name="physdesc" level="1" repeatable="false" rulesetName="Format" />
        <metadata xpath="(./ead:archdesc/ead:did/ead:physdescstructured | ./ead:did/ead:physdescstructured)[1]" xpathType="element" name="physdescstructured" level="1" repeatable="false" rulesetName="physicalDescriptionExtent" />

        <!-- Context Area -->
        <metadata xpath="(./ead:archdesc/ead:did/ead:origination | ./ead:did/ead:origination)[1]" xpathType="element" name="origination" level="2" repeatable="true" rulesetName="Provenience"/>
        <metadata xpath="(./ead:archdesc/ead:odd/ead:head | ./ead:odd/ead:head)[1]" xpathType="element" name="role" level="2" repeatable="false" fieldType="vocabulary">
            <vocabulary>Rollen</vocabulary>
            <!--<searchParameter>type=visible</searchParameter>-->
        </metadata>
        <metadata xpath="(./ead:archdesc/ead:odd/ead:p | ./ead:odd/ead:p)[1]" xpathType="element" name="person" level="2" repeatable="false"/>

        <metadata xpath="(./ead:archdesc/ead:dsc/ead:bioghist | ./ead:dsc/ead:bioghist)[1]" xpathType="element" name="bioghist" level="2" repeatable="true" fieldType="textarea" rulesetName="BiographicalInformation" />
        <metadata xpath="(./ead:archdesc/ead:dsc/ead:custodhist | ./ead:dsc/ead:custodhist)[1]" xpathType="element" name="custodhist" level="2" repeatable="false" fieldType="textarea" rulesetName="InventoryHistory"/>
        <metadata xpath="(./ead:archdesc/ead:dsc/ead:acqinfo | ./ead:dsc/ead:acqinfo)[1]" xpathType="element" name="acqinfo" level="2" repeatable="false" fieldType="dropdown" rulesetName="AquisitionInformation" >
            <value>value 1</value>
            <value>value 2</value>
            <value>...</value>
        </metadata>

        <!-- Content and Structure Area -->
        <metadata xpath="(./ead:archdesc/ead:dsc/ead:scopecontent | ./ead:dsc/ead:scopecontent)[1]" xpathType="element" name="scopecontent" level="3" repeatable="false" fieldType="textarea" rulesetName="ContentDescription"/>
        <metadata xpath="(./ead:archdesc/ead:dsc/ead:appraisal | ./ead:dsc/ead:appraisal)[1]" xpathType="element" name="appraisal" level="3" repeatable="false" fieldType="textarea" rulesetName="AppraisalInformation"/>
        <metadata xpath="(./ead:archdesc/ead:dsc/ead:accruals | ./ead:dsc/ead:accruals)[1]" xpathType="element" name="accruals" level="3" repeatable="true" fieldType="textarea" rulesetName="Additions"/>
        <metadata xpath="(./ead:archdesc/ead:dsc/ead:arrangement | ./ead:dsc/ead:arrangement)[1]" xpathType="element" name="arrangement" level="3" repeatable="false" fieldType="textarea" rulesetName="Arrangement"/>

        <!-- Condition of Access and Use Area -->
        <metadata xpath="(./ead:archdesc/ead:dsc/ead:accessrestrict | ./ead:dsc/ead:accessrestrict)[1]" xpathType="element" name="accessrestrict" level="4" repeatable="false" fieldType="dropdown" rulesetName="RestrictionOnAccessLicense" importMetadataInChild="true">
            <value>open access</value>
            <value>restricted</value>
            <value>required registration</value>
        </metadata>

        <metadata xpath="(./ead:archdesc/ead:dsc/ead:userestrict | ./ead:dsc/ead:userestrict)[1]" xpathType="element" name="userestrict" level="4" repeatable="false" fieldType="dropdown" importMetadataInChild="true" rulesetName="UseRestriction">
            <value>damaged</value>
            <value>good condition</value>
        </metadata>

        <metadata xpath="(./ead:archdesc/ead:did/ead:langmaterial[@label='Language']/ead:language | ./ead:did/ead:langmaterial[@label='Language']/ead:language)[1]" xpathType="element" name="langmaterial" level="4" repeatable="false" fieldType="multiselect" rulesetName="DocLanguage" importMetadataInChild="false">
            <value>eng</value>
            <value>ger</value>
            <value>dut</value>
            <value>fre</value>
            <value>esp</value>
            <value>ita</value>
            <value>lat</value>
            <value>pol</value>
            <value>rus</value>
            <value>swe</value>
        </metadata>

        <metadata xpath="(./ead:archdesc/ead:did/ead:langmaterial[@label='font'] | ./ead:did/ead:langmaterial[@label='font'])[1]" xpathType="element" name="font" level="4" repeatable="false" fieldType="multiselect" rulesetName="FontType" importMetadataInChild="false">
            <value>antiqua</value>
            <value>fracture</value>
            <value>handwritten</value>
            <value>mixed</value>
            <value>no text</value>
        </metadata>

        <metadata xpath="(./ead:archdesc/ead:dsc/ead:phystech | ./ead:dsc/ead:phystech)[1]" xpathType="element" name="phystech" level="4" repeatable="false" fieldType="textarea" rulesetName="PhysTech" />
        <metadata xpath="(./ead:archdesc/ead:dsc/ead:otherfindaid | ./ead:dsc/ead:otherfindaid)[1]" xpathType="element" name="otherfindaid" level="4" repeatable="false" fieldType="textarea" rulesetName="OtherFindAid"/>

        <!-- Allied Materials Area -->
        <metadata xpath="(./ead:archdesc/ead:dsc/ead:originalsloc | ./ead:dsc/ead:originalsloc)[1]" xpathType="element" name="originalsloc" level="5" repeatable="false" fieldType="dropdown" rulesetName="OriginalsLocation">
            <value>value 1</value>
            <value>value 2</value>
        </metadata>
        <metadata xpath="(./ead:archdesc/ead:dsc/ead:altformavail | ./ead:dsc/ead:altformavail)[1]" xpathType="element" name="altformavail" level="5" repeatable="false" fieldType="dropdown" rulesetName="AlternativeFormAvailable">
            <value>value 1</value>
            <value>value 2</value>
        </metadata>
        <metadata xpath="(./ead:archdesc/ead:dsc/ead:relatedmaterial/ead:separatedmaterial | ./ead:dsc/ead:relatedmaterial/ead:separatedmaterial)[1]" xpathType="element" name="separatedmaterial" level="5" repeatable="false" rulesetName="SeparatedMaterial"/>
        <metadata xpath="(./ead:archdesc/ead:dsc/ead:bibliography | ./ead:dsc/ead:bibliography)[1]" xpathType="element" name="bibliography" level="5" repeatable="false" rulesetName="BibliographicCitation"/>


        <!-- Note Area -->
        <metadata xpath="(./ead:archdesc/ead:did/ead:didnote | ./ead:did/ead:didnote)[1]" xpathType="element" name="didnote" level="6" repeatable="false" fieldType="textarea" rulesetName="DidNote"/>
        <metadata xpath="(./ead:archdesc/ead:dsc/ead:odd | ./ead:dsc/ead:odd)[1]" xpathType="element" name="oddnote" level="6" repeatable="false" fieldType="textarea" rulesetName="Odd" />

        <!-- Description Control Area -->
        <metadata xpath="./ead:control/ead:conventiondeclaration" xpathType="element" name="conventiondeclaration" level="7" repeatable="false" fieldType="multiselect" rulesetName="ConventionDeclaration">
            <value>val 1</value>
            <value>val 2</value>
            <value>val 3</value>
            <value>val 4</value>
        </metadata>

    	<node name="file" ruleset="File" icon="fa fa-file-text-o" processTemplateId="309544" />
    	<node name="folder" ruleset="Folder" icon="fa fa-folder-open-o" processTemplateId="309544"/>
    	<node name="image" ruleset="Picture" icon="fa fa-file-image-o" processTemplateId="309544" />
    	<node name="audio" ruleset="Audio" icon="fa fa-file-audio-o" processTemplateId="309544"/>
    	<node name="video" ruleset="Video" icon="fa fa-file-video-o" processTemplateId="309544"/>
    	<node name="other" ruleset="Other" icon="fa fa-file-o" processTemplateId="309544" />
        
        <!-- configured fields are used to find identifying metadata in processes and ead nodes  -->
        <processIdentifierField>NodeId</processIdentifierField>
        <nodeIdentifierField>id</nodeIdentifierField>
        
    </config>
</config_plugin>
```

## General configuration

The two parameters `<basexUrl>` and `<eadExportFolder>` configure the connection to the BaseX XML database. The URL to the REST API of the XML database is configured via `basexUrl`, `eadExportFolder` contains the folder name into which the plugin exports the EAD files. This folder is monitored by the XML database.

This is followed by a repeatable `<config>` block. The repeatable element `<archive>` can be used to specify for which files the `<config>` block should apply. If there should be a default block that should apply to all documents, `*` can be used.

The `<processTemplateId>` is used to specify which process template should be used as the basis for the Goobi processes created.

## Configuration of the generation of process titles
The parameters `<lengthLimit>` `<separator>` `<useIdFromParent>` and `<useSignature>` are used to configure the naming of the process to be generated:

* The `<lengthLimit>` value sets a length limit for all tokens except the manually set prefix and suffix. The default setting is `0`, i.e. it does not limit the length.
* The parameter `<separator>` defines the separator to be used to combine all separate tokens. The default setting is `_`.
- The parameter `<useIdFromParent>` configures whose ID should be used to create the process title. If it is set to `true`, the ID of the parent node is used. Otherwise, the ID of the current node is used.
- The parameter `<useShelfmarkAsId>` configures whether the shelfmark should preferably be used to generate the process title. If it is set to `true` and a shelfmark actually exists in a predecessor node, this shelfmark is used. Otherwise, the ID of the current node or the parent node is used, depending on the configuration of the parameter `<useIdFromParent>`. 


## Configuring the metadata fields

This is followed by a list of `<metadata>` elements. This controls which fields are displayed, can be imported, how they should behave and whether there are validation rules.

### Mandatory fields

Each metadata field consists of at least three mandatory fields:

| Value | Description |
| :--- | :--- |
| `name` | This value identifies the field. It must therefore contain a unique designation. If the value has not been configured separately in the messages files, it is also used as the label of the field. |
| `level` | This defines the area in which the metadatum is inserted. The numbers 1-7 are allowed: `1. Identity Statement Area`, `2. Context Area`, `3. Content and Structure Area`, `4. Condition of Access and Use Area`, `5. Allied Materials Area`, `6. Note Area`, `7. Description Control Area` |
| `xpath` | Defines an XPath expression that is used both to read from existing EAD files and to write to the EAD file. The path is relative to the `<ead>` element in the case of the main element, and always relative to the `<c>` element for all other nodes. |

### Optional information

There are also a number of other optional specifications:

| Value | Description |
| :--- | :--- |
| `@xpathType` | This defines whether the XPath expression returns an `element` \(default\), an `attribute` or a `text`. |
| `@visible` | This can be used to control whether the metadata is displayed in the mask or is hidden. The field may contain the two values `true` \(default\) and `false`. |
| `@repeatable` | Defines whether the field is repeatable. The field may contain the two values `true` and `false` \(default\). |
| `@showField` | Defines whether the field is displayed open in the detail display, even if no value is available yet. The field may contain the two values `true` and `false` \(default\). |
| `@rulesetName` | A metadata from the rule set can be specified here. When a Goobi process is created for the node, the configured metadata is created. |
| `@importMetadataInChild` | This can be used to control whether the metadate should also be created in Goobi processes of child nodes. The field may contain the two values `true` and `false` \(default\). |
| `@fieldType` | Controls the behaviour of the field. Possible are `input` \(default\) , `textarea`, `dropdown`, `multiselect`, `vocabulary`. |
| `value` | This sub-element is only used with the two types `dropdown` and `multiselect` and contains the possible values that are to be available for selection. |
| `vocabulary` | This subelement contains the name of the vocabulary to be used. It is only evaluated if `vocabulary` is set in the type of the field. The selection list contains the main value of each record. |
| `searchParameter` | This repeatable subfield can be used to define search parameters with which to filter the vocabulary, the syntax is `fieldname=value`. |
| `@validationType` | Here you can set whether the field should be validated. Three different rules are possible, which can be combined. `unique` checks that the content of the field has not been used again in another place, `required` ensures that the field contains a value. The type `regex` can be used to check whether the filled value corresponds to a regular expression. With `unique+required`, `regex+required`, `regex+unique` and `regex+unique+required` several validation rules can be applied. |
| `@regularExpression` | The regular expression to be used for `regex` validation is specified here. IMPORTANT: the backslash must be masked by a second backslash. A class is therefore not defined by `\w` but by `\\w`. |
| `validationError` | This subfield contains a text that is displayed if the field content violates the validation rules. |

## Examples of different field configurations

### Simple input field

```xml
<metadata xpath="./ead:control/ead:maintenanceagency/ead:agencycode" xpathType="element" name="agencycode" level="1" repeatable="false" fieldType="input"/>
```

### Text area

```xml
<metadata xpath="(./ead:archdesc/ead:did/ead:unittitle | ./ead:did/ead:unittitle)[1]" xpathType="element" name="unittitle" level="1" repeatable="false" fieldType="textarea" rulesetName="TitleDocMain" importMetadataInChild="false" />
```

### Selection list

```xml
<metadata xpath="(./ead:archdesc/@level | ./@level)[1]" xpathType="attribute" name="descriptionLevel" level="1" repeatable="false" fieldType="dropdown">
    <value>collection</value>
    <value>fonds</value>
    <value>class</value>
    <value>recordgrp</value>
    <value>series</value>
    <value>subfonds</value>
    <value>subgrp</value>
    <value>subseries</value>
    <value>file</value>
    <value>item</value>
    <value>otherlevel</value>
</metadata>
```

### Multiple selection

```xml
<metadata xpath="(./ead:archdesc/ead:did/ead:langmaterial[@label='Language']/ead:language | ./ead:did/ead:langmaterial[@label='Language']/ead:language)[1]" xpathType="element" name="langmaterial" level="4" repeatable="false" fieldType="multiselect" rulesetName="DocLanguage" importMetadataInChild="false">
    <value>eng</value>
    <value>ger</value>
    <value>dut</value>
    <value>fre</value>
    <value>esp</value>
    <value>ita</value>
    <value>lat</value>
    <value>pol</value>
    <value>rus</value>
    <value>swe</value>
</metadata>
```

### Validation of dates

```xml
<metadata xpath="(./ead:archdesc/ead:did/ead:unitdate | ./ead:did/ead:unitdate)[1]" xpathType="element" name="unitdate" level="1" repeatable="false" rulesetName="PublicationYear" importMetadataInChild="false" regularExpression="^([0-9]{4}\\-[0-9]{2}\\-[0-9]{2}|[0-9]{4})(\\s?\\-\s?([0-9]{4}\\-[0-9]{2}\\-[0-9]{2}|[0-9]{4}))?$" validationType="regex">
  <validationError>The value is not a date specification. Permitted values are either years (YYYY), exact dates (YYYY-MM-DD) or time periods (YYYY - YYYY, YYYY-MM-DD-YYYY-MM-DD).</validationError>
</metadata>
```

### Connection of a controlled vocabulary

```xml
<metadata xpath="(./ead:archdesc/ead:dsc/ead:acqinfo | ./ead:dsc/ead:acqinfo)[1]" xpathType="element" name="acqinfo" level="2" repeatable="false" fieldType="vocabulary" rulesetName="AquisitionInformation" >
  <vocabulary>Aquisition</vocabulary>
  <searchParameter>type=visible</searchParameter>
  <searchParameter>active=true</searchParameter>
</metadata>
```