Nach der Durchführung der Installation des Plugins und der zugehörigen Datenbank muss ebenfalls noch eine Konfiguration des Plugins erfolgen. Diese findet innerhalb der Konfigurationsdatei `plugin_intranda_administration_archive_management.xml` statt und ist beispielhaft wie folgt aufgebaut:

```xml
<config_plugin>
    <export>
        <!-- configure export for a specific inventory -->
        <file name="sample.xml">
            <folder>/opt/digiverso/viewer/hotfolder/</folder>
        </file>
        
        <!-- default export for all inventories without a specific  -->
        <file name="*">
            <folder>/opt/digiverso/viewer/hotfolder/</folder>
        </file>
    </export>

    <backup>
        <!-- backup folder -->
        <folder>CHANGEME</folder>
        <!-- number of backups for each inventory -->
        <numberOfFiles>10</numberOfFiles>
        <!-- tool to create the backup files -->
        <tool>/usr/bin/mysqldump</tool>
        <!-- database password. The user name, database name, tables etc. can be recognized automatically, but the password must be entered.-->
        <!-- Leave it empty if access is possible without authentication (e.g. configured in ~/.my.cnf)  -->
        <password></password>
    </backup>


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

        <!-- // configurations for generating process titles // -->

        <!-- configuration for node/process mapping -->
        <!-- it will be used during ead import or when the button to update references is used -->
        
        <!--
            nodeIdentifierField contains the identifying value of the node 
            processIdentifierField contains the name of the metadata where the node value is stored
            If both fields contain the same value, a link between node and process is created
        --> 
        <!-- 
        <nodeIdentifierField>id</nodeIdentifierField>
        <processIdentifierField>NodeId</processIdentifierField>
        -->
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
                - @fieldType: defines the type of the input field. Posible values are input (default), textarea, dropdown, multiselect, vocabulary, nodelink, gnd, geonames, viaf
                - @rulesetName: internal name of the metadata in ruleset. If missing or empty, field is not imported into process metadata
                - @importMetadataInChild: defines if the field is imported or skipped in processes for child elements 
                - @validationType: defines a validation rule, allowed values are unique, required, regex, date, list or any combined values (e.g. date+required)
                - @regularExpression defines a regular expression that gets used for validation type regex
                - validationError: message to display in case of validation errors
                - value: list of possible values for dropdown and multiselect lists
                - vocabulary: name of the vocabulary
                - searchParameter: distinct the vocabulary list by the given condition. Syntax is fieldname=value, field is repeatable
         -->

        <!-- internal fields, not visible on the UI -->

        <metadata xpath="./ead:control/ead:maintenancestatus/@value" xpathType="attribute" name="maintenancestatus" level="1" repeatable="false" visible="false" />
        
        <metadata xpath="./ead:control/ead:maintenanceagency/ead:agencyname" xpathType="element" name="agencycode" level="1" repeatable="false" fieldType="input" />

        <!-- repository data group -->
        <metadata xpath="./ead:archdesc/ead:did/ead:repository" group="true" name="repository" level="1" repeatable="true" visible="false" fieldType="group" rulesetName="Repository">
            <metadata xpath="@label" xpathType="attribute" name="repositoryLabel" level="1" repeatable="false" visible="true" rulesetName="RepositoryLabel" />
            <metadata xpath="ead:address/ead:addressline" xpathType="element" name="repositoryaddressline" level="1" repeatable="true" visible="true" rulesetName="RepositoryAddress" />
            <metadata xpath="ead:extref/@href" xpathType="attribute" name="extrefhref" level="1" repeatable="true" visible="true" rulesetName="RepositoryLink" />
            <metadata xpath="ead:extref" xpathType="element" name="extref" level="1" repeatable="true" visible="true" rulesetName="RepositoryLinkName" />
        </metadata>


        <!--  Identity Statement Area -->
        <metadata xpath="./ead:control/ead:recordid" xpathType="element" name="recordid" level="1" repeatable="false" fieldType="input" rulesetName="RecordID" />
        
        <metadata xpath="./ead:control/ead:filedesc/ead:titlestmt/ead:titleproper" xpathType="element" name="titleproper" level="1" repeatable="false" visible="true" />

        <metadata xpath="./ead:control/ead:eadid" xpathType="element" name="eadid" level="1" repeatable="false" showField="false" fieldType="input" rulesetName="EADID" />

        <metadata xpath="(./ead:archdesc/ead:did/ead:unitid[not(@type)] | ./ead:did/ead:unitid[not(@type)])[1]" xpathType="element" name="unitid" level="1" repeatable="false" showField="false" fieldType="input" rulesetName="UnitID" />

        <metadata xpath="./ead:did/ead:unitid[@type='shelfmark']" xpathType="element" name="Shelfmark" level="1" repeatable="true" rulesetName="shelfmarksource" validationType="unique">
            <validationError>The value has already been used.</validationError>
        </metadata>
        
        <metadata xpath="(./ead:archdesc/ead:did/ead:unittitle | ./ead:did/ead:unittitle)[1]" xpathType="element" name="unittitle" level="1" repeatable="false" fieldType="textarea" rulesetName="TitleDocMain" importMetadataInChild="false" searchable="true" showField="true" />
        
        <metadata xpath="(./ead:archdesc/ead:did/ead:unitdate | ./ead:did/ead:unitdate)" xpathType="element" name="unitdate" level="1" repeatable="false" rulesetName="PublicationYear" importMetadataInChild="false" validationType="date" searchable="true" showField="true">
            <validationError>The value must be entered either as a year in the format YYYY or as a date in the format YYYY-MM-DD.</validationError>
        </metadata>
        
        <metadata xpath="(./ead:archdesc/ead:did/ead:unitdatestructured | ./ead:did/ead:unitdatestructured)[1]" xpathType="element" name="unitdatestructured" level="1" repeatable="false" validationType="date" rulesetName="DateOfOrigin" showField="true">
            <validationError>The value must be entered either as a year in the format YYYY or as a date in the format YYYY-MM-DD.</validationError>
        </metadata>

        <metadata xpath="(./ead:archdesc/@level | ./@level)[1]" xpathType="attribute" name="descriptionLevel" level="1" repeatable="false" fieldType="dropdown" validationType="list">
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

        <metadata xpath="(./ead:archdesc/ead:did/ead:physdescstructured|./ead:did/ead:physdescstructured)" xpathType="element" name="physdescstructured" level="1" repeatable="true"
            rulesetName="physdesc" group="true" fieldType="group" visible="true">
            <metadata xpath="ead:quantity" xpathType="element" name="physdescquantity" level="1" repeatable="false" rulesetName="Quantity" visible="true" />
            <metadata xpath="ead:unittype" xpathType="element" name="physdescunittype" level="1" repeatable="false" rulesetName="Unittype" visible="true" />
        </metadata>

        <metadata xpath="(./ead:archdesc/ead:did/ead:physdesc/ead:extent|./ead:did/ead:physdesc/ead:extent)" xpathType="element" name="physdesc" level="1" repeatable="false"
            rulesetName="physicalDescriptionExtent" />

        <!-- Context Area -->
        <metadata xpath="(./ead:archdesc/ead:did/ead:origination[@label='Creator']/ead:persname|./ead:did/ead:origination[@label='Creator']/ead:persname)[1]" xpathType="element" name="origination" level="2" repeatable="true" rulesetName="Provenience" />
        
        <metadata xpath="(./ead:archdesc/ead:did/ead:origination[@label='Creator']/ead:corpname|./ead:did/ead:origination[@label='Creator']/ead:corpname)[1]" xpathType="element" name="originationcorpname" level="2" repeatable="true" rulesetName="Provenience" />

        <metadata xpath="(./ead:archdesc/ead:odd|./ead:odd)" xpathType="element" name="oddnote" level="2" repeatable="true" visible="true" group="true" fieldType="group">
            <metadata xpath="ead:head" xpathType="element" name="role" level="2" repeatable="false" visible="true" fieldType="input" />
            <metadata xpath="ead:p" xpathType="element" name="person" level="2" visible="true" repeatable="false" fieldType="input" />
        </metadata>

        <metadata xpath="(./ead:archdesc/ead:bioghist/ead:p | ./ead:bioghist/ead:p)[1]" xpathType="element" name="bioghist" level="2" repeatable="true" fieldType="textarea" rulesetName="BiographicalInformation" />

        <metadata xpath="(./ead:archdesc/ead:custodhist/ead:p|./ead:custodhist/ead:p)" group="true" name="custodhist" level="2" repeatable="true" visible="true" fieldType="group" rulesetName="InventoryHistoryGroup">
            <metadata xpath="ead:head" xpathType="element" name="AcquisitionMethod" level="2" repeatable="false" visible="false" fieldType="input" rulesetName="AcquisitionMethod" />
            <metadata xpath="ead:list/ead:item" xpathType="element" name="AcquisitionAgent" level="2" repeatable="false" visible="false" fieldType="input" rulesetName="AcquisitionAgent" />
            <metadata xpath="ead:p" xpathType="element" name="AcquisitionNotes" level="2" repeatable="false" visible="false" fieldType="textarea" rulesetName="AcquisitionNotes" />
        </metadata>

        <!-- Content and Structure Area -->
        <metadata xpath="(./ead:archdesc/ead:scopecontent/ead:p | ./ead:scopecontent/ead:p)[1]" xpathType="element" name="scopecontent" level="3" repeatable="false" fieldType="textarea" rulesetName="ContentDescription" />
        
        <metadata xpath="(./ead:archdesc/ead:appraisal/ead:p | ./ead:appraisal/ead:p)[1]" xpathType="element" name="appraisal" level="3" repeatable="false" fieldType="textarea" rulesetName="AppraisalInformation" />
        
        <metadata xpath="(./ead:archdesc/ead:arrangement/ead:p | ./ead:arrangement/ead:p)[1]" xpathType="element" name="arrangement" level="3" repeatable="false" fieldType="textarea" rulesetName="Arrangement" />

        <!-- accruals group-->
        <metadata xpath="(./ead:archdesc/ead:accruals|./ead:accruals)" group="true" name="accruals" level="3" repeatable="true" visible="true" fieldType="group" rulesetName="AccrualsGroup">
            <metadata xpath="ead:head" xpathType="element" name="accruals_head" level="3" repeatable="false" visible="true" rulesetName="Title" />
            <metadata xpath="ead:p" xpathType="element" name="accruals_p" level="3" repeatable="false" visible="true" rulesetName="Description" />
            <metadata xpath="ead:chronlist/ead:chronitem/ead:datesingle" xpathType="element" name="accruals_date" level="3" repeatable="false" visible="true" rulesetName="Date" validationType="date" />
        </metadata>

        <!-- Condition of Access and Use Area -->
        <metadata xpath="(./ead:archdesc/ead:accessrestrict|./ead:accessrestrict)" group="true" name="accessrestrict" level="4" repeatable="true" visible="true" fieldType="group" rulesetName="AccessRestrictGroup">
            <metadata xpath="(ead:p|ead:p)" xpathType="element" name="accessrestrict_value" level="4" repeatable="false" fieldType="dropdown" rulesetName="RestrictionOnAccessLicense" importMetadataInChild="true">
                <value>open access</value>
                <value>restricted</value>
            </metadata>
            <metadata xpath="ead:chronlist/ead:chronitem/ead:datesingle" xpathType="element" name="accessrestrict_date" level="3" repeatable="false" visible="true" rulesetName="Date" validationType="date" />
        </metadata>

        <metadata xpath="(./ead:archdesc/ead:userestrict/ead:p | ./ead:userestrict/ead:p)[1]" xpathType="element" name="userestrict" level="4" repeatable="false" fieldType="dropdown" importMetadataInChild="true" rulesetName="UseRestriction">
            <value>CC0 1.0</value>
            <value>CC BY 4.0</value>
            <value>CC BY-SA 4.0</value>
            <value>CC BY-ND 4.0</value>
            <value>CC BY-NC 4.0</value>
            <value>CC BY-NC-SA 4.0</value>
            <value>CC BY-NC-ND 4.0</value>
        </metadata>

        <metadata xpath="./ead:did/ead:langmaterial/ead:language" xpathType="element" name="langmaterial" level="4" repeatable="true" fieldType="textarea" rulesetName="DocLanguage" importMetadataInChild="false">
            <value>ger</value>
            <value>eng</value>
            <value>fre</value>
            <value>ita</value>
            <value>lat</value>
            <value>spa</value>
            <value>ara</value>
            <value>heb</value>
        </metadata>

        <metadata xpath="(./ead:archdesc/ead:did/ead:langmaterial[@label='font'] | ./ead:did/ead:langmaterial[@label='font'])[1]" xpathType="element" name="font" level="4" repeatable="false" fieldType="multiselect" rulesetName="FontType" importMetadataInChild="false">
            <value>antiqua</value>
            <value>fracture</value>
            <value>handwritten</value>
            <value>mixed</value>
            <value>no text</value>
        </metadata>

        <metadata xpath="(./ead:archdesc/ead:phystech/ead:p | ./ead:phystech/ead:p)" xpathType="element" name="phystech" level="4" repeatable="false" fieldType="textarea" rulesetName="PhysTech" />
        
        <metadata xpath="(./ead:archdesc/ead:otherfindaid|./ead:otherfindaid)" group="true" name="otherfindaid" level="4" repeatable="true" visible="true" fieldType="group" rulesetName="OtherFindAidGroup">
            <metadata xpath="(ead:head|ead:head)" xpathType="element" name="otherfindaid_type" level="4" repeatable="false" fieldType="input" rulesetName="Type" />
            <metadata xpath="ead:p/ead:ref" xpathType="element" name="otherfindaid_link" level="4" repeatable="false" fieldType="input" rulesetName="OtherFindAid" />
            <metadata xpath="ead:p/ead:ptr" xpathType="element" name="otherfindaid_node" level="4" repeatable="false" fieldType="nodelink" rulesetName="OtherFindAid" />
        </metadata>

        <!-- Allied Materials Area -->

        <!-- Location of Originals group-->
        <metadata xpath="(./ead:archdesc/ead:originalsloc|./ead:originalsloc)" group="true" name="originalsloc" level="5" repeatable="true" visible="true" fieldType="group" rulesetName="OriginalsLocationGroup">
            <metadata xpath="ead:p/ead:name" xpathType="element" name="originalsloc_person" level="5" repeatable="false" visible="true" rulesetName="Person" fieldType="input" />
            <metadata xpath="ead:p/ead:subject" xpathType="element" name="originalsloc_shelfmark" level="5" repeatable="false" visible="true" rulesetName="shelfmarksource" />
            <metadata xpath="ead:p/ead:ref" xpathType="element" name="originalsloc_link" level="4" repeatable="false" fieldType="input" rulesetName="RepositoryLink" />
        </metadata>

        <!-- Alternative Form Available group-->
        <metadata xpath="(./ead:archdesc/ead:relatedmaterial|./ead:relatedmaterial)" group="true" name="altformavail" level="5" repeatable="true" visible="true" fieldType="group" rulesetName="CopyLocationGroup">
            <metadata xpath="ead:p/ead:name" xpathType="element" name="altformavail_person" level="5" repeatable="false" visible="true" rulesetName="Person" fieldType="input" />
            <metadata xpath="ead:p/ead:subject" xpathType="element" name="altformavail_shelfmark" level="5" repeatable="false" visible="true" rulesetName="shelfmarksource" />
            <metadata xpath="ead:p/ead:ref" xpathType="element" name="altformavail_link" level="4" repeatable="false" fieldType="input" rulesetName="RepositoryLink" />
        </metadata>

        <metadata xpath="(./ead:archdesc/ead:separatedmaterial/ead:p|./ead:separatedmaterial/ead:p)[1]" xpathType="element" name="separatedmaterial" level="5" repeatable="true" rulesetName="SeparatedMaterial" fieldType="nodelink" />
        
        <metadata xpath="(./ead:archdesc/ead:bibliography|./ead:bibliography)[1]" xpathType="element" name="bibliography" level="5" repeatable="false" rulesetName="BibliographicCitation" />

        <!-- Note Area -->
        <metadata xpath="(./ead:archdesc/ead:note/ead:p|./ead:note/ead:p)[1]" xpathType="element" name="didnote" level="6" repeatable="true" fieldType="textarea" rulesetName="DidNote" />
       
        <metadata xpath="./ead:control/ead:localtypedeclaration" xpathType="element" name="Conventions" level="6" repeatable="false" fieldType="textarea" rulesetName="ConventionDeclaration" />
        
        <metadata xpath="./ead:processinfo/ead:chronlist/ead:chronitem/ead:datesingle" xpathType="element" name="DescriptionDates" level="6" repeatable="false" fieldType="textarea" rulesetName="DescriptionDates" />


        <!-- Description Control Area -->
        <metadata xpath="./ead:archdesc/ead:processinfo/ead:list/ead:item" xpathType="element" name="editorName" level="7" repeatable="true" fieldType="textarea" visible="false" />

        <metadata xpath="./ead:processinfo/ead:p" xpathType="element" name="ArchivistNote" level="7" repeatable="false" fieldType="textarea" rulesetName="ArchivistNote" />

        <metadata xpath="./ead:control/ead:conventiondeclaration/ead:abbr" xpathType="element" name="conventiondeclaration" level="7" repeatable="false" fieldType="multiselect">
            <value>ISAD(G)</value>
            <value>NCARules</value>
            <value>ISO 8601</value>
            <value>DACS</value>
        </metadata>

        <metadata xpath="./ead:control/ead:maintenancehistory/ead:maintenanceevent" group="true" name="maintenancehistory" level="7" repeatable="true" visible="false" fieldType="group">
            <metadata xpath="ead:eventtype" xpathType="element" name="eventtype" level="1" repeatable="false" visible="false" />
            <metadata xpath="ead:agent" xpathType="element" name="agent" level="1" repeatable="false" visible="false" />
            <metadata xpath="ead:eventdescription" xpathType="element" name="eventdescription" level="1" repeatable="false" visible="false" />
            <metadata xpath="ead:eventdatetime" xpathType="element" name="eventdatetime" level="1" repeatable="false" visible="false" />
        </metadata>

        <!-- viaf sample
        <metadata xpath="./ead:archdesc/ead:index/ead:indexentry/ead:corpname/ead:part" xpathType="element" name="Corporate" level="7" repeatable="true" searchable="true" showField="true" fieldType="viaf"
        searchFields="210__a; 111__a; 100__a; 110__a; 150__a; 151__a;" displayFields="001=NORM_IDENTIFIER; 0247_a=URI; 1001_a=NORM_NAME; 1001_d=NORM_LIFEPERIOD; 1001_q=NORM_SEX; 375__a=NORM_SEX;" />
        -->

        <!-- geonames sample
        <metadata xpath="./ead:archdesc/ead:index/ead:indexentry/ead:geogname/ead:part[@localtype='place']" xpathType="element" name="Place" level="7" repeatable="true" fieldType="geonames" visible="true" />
        -->
        <!-- gnd sample
        <metadata xpath="./ead:archdesc/ead:index/ead:indexentry/ead:persname/ead:part" xpathType="element" name="Person" level="7" repeatable="true" fieldType="gnd" visible="true" />
        -->

        <!-- extend configured areas -->
        <showGroup level="1" />

        <!--
        <showGroup level="2" />
        <showGroup level="3" />
        <showGroup level="4" />
        <showGroup level="5" />
        <showGroup level="6" />
        <showGroup level="7" />

        -->

        <treeView>
            <!-- tree view: display/hide node id-->
            <showNodeId>false</showNodeId>
        </treeView>

        <!-- enables template and project name selection in process creation area -->
        <showProjectSelection>false</showProjectSelection>
        <!-- 
        possible namespaces: 
            ead2: urn:isbn:1-931666-22-9
            ead3: http://ead3.archivists.org/schema/
            ead4: https://archivists.org/ns/ead/v4 
        -->
        <eadNamespaceRead>http://ead3.archivists.org/schema/</eadNamespaceRead>
        <eadNamespaceWrite>http://ead3.archivists.org/schema/</eadNamespaceWrite>

        <node name="file" ruleset="File" icon="fa fa-file-text-o" processTemplateId="456" />
        <node name="folder" ruleset="Folder" icon="fa fa-folder-open-o" processTemplateId="456" />
        <node name="image" ruleset="Picture" icon="fa fa-file-image-o" processTemplateId="456" />
        <node name="audio" ruleset="Audio" icon="fa fa-file-audio-o" processTemplateId="456" />
        <node name="video" ruleset="Video" icon="fa fa-file-video-o" processTemplateId="456" />
        <node name="other" ruleset="Other" icon="fa fa-file-o" processTemplateId="456" />
    </config>
    
    
    <config>
        <archive>ead2 sample</archive>
        <processTemplateId>2</processTemplateId>
        <nodeDefaultTitle>Document</nodeDefaultTitle>
        
        <lengthLimit>25</lengthLimit>
        
        <separator>_</separator>
        
        <useIdFromParent>false</useIdFromParent>
        
        <useShelfmarkAsId>false</useShelfmarkAsId>

        <metadata xpath="./ead:eadheader[@countryencoding='iso3166-1'][@dateencoding='iso8601'][@langencoding='iso639-2b'][@repositoryencoding='iso15511'][@scriptencoding='iso15924']/ead:eadid/@mainagencycode" xpathType="attribute" name="mainagencycode" level="1" repeatable="false" visible="false"/>
        <metadata xpath="./ead:eadheader/ead:profiledesc/ead:creation/@normal" xpathType="attribute" name="normalcreationdate" level="1" repeatable="false" visible="false"/>
        <metadata xpath="./ead:eadheader/ead:profiledesc/ead:creation" xpathType="element" name="creationdate" level="1" repeatable="false" visible="false"/>
        <metadata xpath="./ead:eadheader/ead:filedesc/ead:titlestmt/ead:titleproper" xpathType="element" name="titlestmt" level="1" repeatable="false" visible="false"/>

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

        <eadNamespaceRead>urn:isbn:1-931666-22-9</eadNamespaceRead>
        <eadNamespaceWrite>urn:isbn:1-931666-22-9</eadNamespaceWrite>

        <!-- root nodes -->
        <node name="archive" icon="fa fa-archive" processTemplateId="456" rootNode="true" allowProcessCreation="false">
            <child>tectonics</child>
            <child>inventory</child>
            <child>folder</child>
        </node>

        <!-- folder level nodes -->
        <node name="inventory" icon="fa fa-sitemap" processTemplateId="456" rootNode="true" allowProcessCreation="false">
            <child>folder</child>
            <child>tectonics</child>
            <child>inventory</child>
        </node>
        <node name="tectonics" icon="fa fa-archive-o" processTemplateId="456" rootNode="true" allowProcessCreation="false">
            <child>tectonics</child>
            <child>folder</child>
        </node>

        <node name="folder" ruleset="Folder" icon="fa fa-folder-open-o" processTemplateId="456" allowProcessCreation="false">
            <child>folder</child>
            <child>file</child>
            <child>image</child>
            <child>audio</child>
            <child>video</child>
            <child>other</child>
        </node>

        <!-- item level nodes -->
        <node name="file" ruleset="File" icon="fa fa-file-text-o" processTemplateId="456" allowProcessCreation="true" />
        <node name="image" ruleset="Picture" icon="fa fa-file-image-o" processTemplateId="456" allowProcessCreation="true" />
        <node name="audio" ruleset="Audio" icon="fa fa-file-audio-o" processTemplateId="456" allowProcessCreation="true" />
        <node name="video" ruleset="Video" icon="fa fa-file-video-o" processTemplateId="456" allowProcessCreation="true" />
        <node name="other" ruleset="Other" icon="fa fa-file-o" processTemplateId="456" allowProcessCreation="true" />

    </config>
    
    
</config_plugin>
```


## Allgemeine Konfiguration
Im Bereich `<export>` wird die Anbindung an den Goobi viewer konfiguriert. Hier wird der Ort festgelegt, an den ein Export als EAD-XML erfolgt und welche Bestände exportiert werden sollen. Der Export erfolgt automatisch in regelmäßigen Abständen oder kann manuell aus der Nutzeroberfläche gestartet werden.

Im zweiten Bereich `<backup>` kann ein automatisches Backup der einzelnen Bestände konfiguriert werden. Dabei wird für jeden Bestand eine eigene Datei erzeugt. Es kann definiert werden, wie viele Backups vorgehalten werden sollen und welches Tool zum Erzeugen der Backups genutzt werden soll. Falls ein Passwort für den Datenbank-Zugriff benötigt wird, kann dies hier ebenfalls konfiguriert werden.

Anschließend folgt ein wiederholbarer `<config>` Block. Über das wiederholbare Element `<archive>` kann festgelegt werden, für welche Dateien der `<config>`-Block gelten soll. Soll es einen Default-Block geben, der für alle Dokumente gelten soll, kann `*` genutzt werden.

Mittels `<processTemplateId>` wird festgelegt, auf Basis welcher Produktionsvorlage die Goobi-Vorgänge erstellt werden sollen.


## Konfiguration der Generierung von Vorgangstiteln
Die Parameter `<lengthLimit>` `<separator>` `<useIdFromParent>` und `<title>` werden verwendet, um die Benennung des zu erzeugenden Vorgangs zu konfigurieren:

- Der Wert `<lengthLimit>` setzt ein Längenlimit für alle Tokens außer dem manuell gesetzten Präfix und Suffix. Die Voreinstellung ist `0`, begrenzt die Länge also nicht.
- Der Parameter `<separator>` definiert das Trennzeichen, das verwendet werden soll, um alle separaten Tokens zu kombinieren. Die Voreinstellung ist `_`.
- Der Parameter `<useIdFromParent>` konfiguriert, wessen ID für die Erstellung des Vorgangstitels verwendet werden soll. Wenn er auf `true` gesetzt ist, wird die ID des übergeordneten Knotens verwendet. Andernfalls wird die ID des aktuellen Knotens verwendet.
- Der Parameter `<title>` konfiguriert, welche Metadaten für die Titelgenerierung genutzt werden sollen. Dabei kann das Attribut `value` einen statischen Text oder das Attribut `name` den Namen eines Metadatenfeldes beinhalten. Mittels `type` wird gesteuert, was mit dem Wert geschehen soll `NORMAL` fügt das Feld unverändert ein, `CAMEL_CASE` ersetzt Leerzeichen und lässt jedes Wort mit einem Großbuchstaben starten, `AFTER_LAST_SEPARATOR` fügt das Feld immer am Ende an, `BEFORE_FIRST_SEPARATOR` fügt es immer am Anfang an. Wenn kein title konfiguriert wurde, wird der Vorgangstitel auf Basis der node ID gebildet.


## Konfiguration der Verknüpfung zwischen Knoten und Vorgang

Die beiden Parameter `<nodeIdentifierField>` und `<processIdentifierField>` dienen zur Verknüpfung zwischen Knoten und Vorgang. Im Feld `<nodeIdentifierField>` steht der Name des Feldes, das den Identifier des Knotens enthält. Dabei kann jedes konfigurierte Feld genutzt werden. Wenn nicht anders angegeben, wird `id` genutzt. In `<processIdentifierField>` steht das Metadatum, in dem der Identifier des Knotes gespeichert werden soll. Üblicherweise ist dies `NodeId`.

Wenn eine neue EAD Datei importiert wird oder der Button "Verweise zu Vorgängen aktualisieren" genutzt wird, wird in allen Vorgängen nach dem konfigurierten Metadatum gesucht. Anschließend wird verglichen, ob das Metadatum den Wert enthält, der im Feld in einem Knoten eingetragen ist. Wenn dies der Fall ist, wird eine Verknüpfung zwischen Knoten und Vorgang erstellt. Bei allen Knoten, zu denen kein Treffer gefunden wurde, werden eventuell vorhandene Verweise entfernt.


## Konfiguration der Metadatenfelder
Anschließend folgt eine Liste von `<metadata>` Elementen. Darüber wird gesteuert, welche Felder angezeigt werden, importiert werden können, wie sie sich verhalten sollen und ob es Validierungsregeln gibt.


### Pflichtangaben
Jedes Metadatenfeld besteht aus mindestens den folgenden Pflichtangaben:

| Wert | Beschreibung |
| :--- | :--- |
| `name` | Mit diesem Wert wird das Feld identifiziert. Es muss daher eine eindeutige Bezeichnung enthalten. Sofern der Wert nicht noch extra in den messages Dateien konfiguriert wurde, wird er auch als Label des Feldes genutzt. |
| `level` | Hiermit wird definiert, in welchem Bereich das Metadatum eingefügt wird. Dabei sind die Zahlen 1-7 erlaubt: `1. Identifikation`, `2. Kontext`, `3. Inhalt und innere Ordnung`, `4. Zugangs- und Benutzungsbedingungen`, `5. Sachverwandte Unterlagen`, `6. Anmerkungen`, `7. Verzeichnungskontrolle` |


### Optionale Angaben
Des weiteren gibt es noch eine Reihe weiterer optionaler Angaben:

| Wert | Beschreibung |
| :--- | :--- |
| `xpath` | Definiert einen XPath Ausdruck, der sowohl zum Lesen aus vorhanden EAD Dateien als auch zum Schreiben der EAD Datei genutzt wird. Der Pfad ist im Fall des Hauptelements relativ zum `<ead>` Element, bei allem anderen Knoten immer relativ zum `<c>` Element. |
| `@xpathType` | Hiermit wird definiert, ob der XPath Ausdruck ein `element` \(default\), ein `attribute` oder einen `text` zurückliefert. |
| `@visible` | Hierüber kann gesteuert werden, ob das Metadatum in der Maske angezeigt wird oder versteckt ist. Das Feld darf die beiden Werte `true` \(default\) und `false` enthalten. |
| `@repeatable` | Definiert, ob das Feld wiederholbar ist. Das Feld darf die beiden Werte `true` und `false` \(default\) enthalten. |
| `@showField` | Definiert, ob das Feld in der Detailanzeige geöffnet angezeigt wird, auch wenn noch kein Wert vohanden ist. Das Feld darf die beiden Werte `true` und `false` \(default\) enthalten. |
| `@rulesetName` | Hier kann ein Metadatum aus dem Regelsatz angegeben werden. Wenn für den Knoten ein Goobi-Vorgang erstellt wird, wird das konfigurierte Metadatum erstellt. |
| `@importMetadataInChild` | Hierüber kann gesteuert werden, ob das Metadatum auch in Goobi-Vorgängen von Kind-Knoten erstellt werden soll. Das Feld darf die beiden Werte `true` und `false` \(default\) enthalten. |
| `@fieldType` | Steuert das Verhalten des Feldes. Möglich sind `input` \(default\) ,  `textarea`,  `dropdown`, `multiselect`, `vocabulary`, `nodelink`, `gnd`, `geonames`, `viaf` |
| `value` | Dieses Unterelement wird nur bei den beiden Typen `dropdown` und  `multiselect` genutzt und enthält die möglichen Werte, die zur Auswahl stehen sollen. |
| `vocabulary` | Dieses Unterelement enthält den Namen des zu verwendenden Vokabulars. Es wird nur ausgewertet, wenn `vocabulary`, `dropdown` oder `multiselect` im Typ des Feldes gesetzt ist und keine `<value>` Elemente konfiguriert wurden. Die Auswahlliste enthält jeweils den Hauptwert der Records. |
| `searchParameter` | Mit diesem wiederholbaren Unterfeld können Suchparameter definiert werden, mit denen das Vokabular gefiltert wird, die Syntax ist `fieldname=value`. |
| `@validationType` | Hier kann eingestellt werden, ob das Feld validiert werden soll. Dabei sind unterschiedliche Regeln möglich, die sich kombinieren lassen. `unique` prüft, dass der Inhalt des Feldes nicht noch einmal an einer anderen Stelle genutzt wurde, mittels `required` wird sichergestellt, dass das Feld einen Wert enthhält. Mit dem Typ `regex` kann geprüft werden, ob der ausgefüllte Wert einem regulären Ausdruck entspricht, `date` prüft, ob der Wert einem EDTF Datum entspricht und `list` testet, ob der verwendete Wert in der konfigurierten Liste enthalten ist.
Es können auch mehrere Validierungsregeln kombiniert werden, zum Beispiel `unique+required`, `regex+required`, `regex+unique` oder `date+required`. In diesem Fall müssen alle angegeben Regeln erfüllt werden.|
| `@regularExpression` | Hier wird der reguläre Ausdruck angegeben, der bei der `regex` Validierung genutzt werden soll. WICHTIG: der Backslash muss dabei durch einen zweiten Backslash maskiert werden. Eine Klasse wird daher nicht durch `\w`, sondern durch `\\w` definiert. |
| `validationError` | Dieses Unterfeld enthält einen Text, der angezeigt wird, wenn der Feldinhalt gegen die Validierungsregeln verstößt. |
| `@searchable` | Hierüber kann gesteuert werden, ob das Metadatum in der erweiterten Suche als Feld angeboten werden soll. Das Feld darf die beiden Werte `true` und `false` \(default\) enthalten. |


## Beispiele für verschiedene Feld-Konfigurationen

### Einfaches Eingabefeld
```xml
<metadata xpath="./ead:control/ead:maintenanceagency/ead:agencycode" xpathType="element" name="agencycode" level="1" repeatable="false" fieldType="input"/>
```

![Eingabefeld](images/goobi-plugin-administration-archive-management_screen12_de.png)


### Textfeld
```xml
<metadata xpath="(./ead:archdesc/ead:did/ead:unittitle | ./ead:did/ead:unittitle)[1]" xpathType="element" name="unittitle" level="1" repeatable="false" fieldType="textarea" rulesetName="TitleDocMain" importMetadataInChild="false" />
```

![Textarea](images/goobi-plugin-administration-archive-management_screen13_de.png)


### Auswahlliste
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

![Auswahlliste](images/goobi-plugin-administration-archive-management_screen14_de.png)


### Mehrfachauswahl
```xml
        <metadata xpath="(./ead:archdesc/ead:did/ead:langmaterial[@label='font'] | ./ead:did/ead:langmaterial[@label='font'])[1]" xpathType="element" name="font" level="4" repeatable="false"
            fieldType="multiselect" rulesetName="FontType" importMetadataInChild="false">
            <value>antiqua</value>
            <value>fracture</value>
            <value>handwritten</value>
            <value>mixed</value>
            <value>no text</value>
        </metadata>
```

![Mehrfachauswahl](images/goobi-plugin-administration-archive-management_screen15_de.png)


### Validierung von Datumsangaben im ISO 8601 Format
```xml
<metadata xpath="(./ead:archdesc/ead:did/ead:unitdate | ./ead:did/ead:unitdate)[1]" xpathType="element" name="unitdate" level="1" repeatable="false" rulesetName="PublicationYear" importMetadataInChild="false" regularExpression="^([0-9]{4}\\-[0-9]{2}\\-[0-9]{2}|[0-9]{4})(\\s?\\-\s?([0-9]{4}\\-[0-9]{2}\\-[0-9]{2}|[0-9]{4}))?$" validationType="regex">
  <validationError>Der Wert ist keine Datumsangabe. Erlaubte Werte sind entweder Jahreszahlen (YYYY), exakte Datumsangaben (YYYY-MM-DD) oder Zeiträume (YYYY - YYYY, YYYY-MM-DD-YYYY-MM-DD)</validationError>
</metadata>
```


### Validierung von Datumsangaben im EDTF Format
```xml
<metadata xpath="(./ead:archdesc/ead:did/ead:unitdate | ./ead:did/ead:unitdate)[1]" xpathType="element" name="unitdate" level="1" repeatable="false" rulesetName="PublicationYear" importMetadataInChild="false" validationType="date">
  <validationError>Der Wert ist keine Datumsangabe</validationError>
</metadata>
```


### Anbindung eines kontrollierten Vokabulars
```xml
<metadata xpath="(./ead:archdesc/ead:dsc/ead:acqinfo | ./ead:dsc/ead:acqinfo)[1]" xpathType="element" name="acqinfo" level="2" repeatable="false" fieldType="vocabulary" rulesetName="AquisitionInformation" >
  <vocabulary>Aquisition</vocabulary>
  <searchParameter>type=visible</searchParameter>
  <searchParameter>active=true</searchParameter>
</metadata>
```


### Verknüpfung auf einen anderen Knoten innerhalb des Bestandes
```xml
        <metadata xpath="(./ead:archdesc/ead:relatedmaterial/ead:ref | ./ead:relatedmaterial/ead:ref)" xpathType="element" name="nodelink" fieldType="nodelink" level="5" repeatable="false" />
```

![Verknüpfung](images/goobi-plugin-administration-archive-management_screen16_de.png)

![Auswahl des Knotens](images/goobi-plugin-administration-archive-management_screen17_de.png)


### Suche in der GND
```xml
            <metadata xpath="./ead:archdesc/ead:index/ead:indexentry/ead:persname/ead:part" xpathType="element" name="Person" level="7" repeatable="true" fieldType="gnd" visible="true" />
```
![GND Feld](images/goobi-plugin-administration-archive-management_screen18_de.png)


## Suche in Geonames
```xml
            <metadata xpath="./ead:archdesc/ead:index/ead:indexentry/ead:geogname/ead:part[@localtype='place']" xpathType="element" name="Place" level="7" repeatable="true" fieldType="geonames" visible="true" />
```

![Geonames Feld](images/goobi-plugin-administration-archive-management_screen19_de.png)


## Suche in VIAF
```xml
            <metadata xpath="./ead:archdesc/ead:index/ead:indexentry/ead:corpname/ead:part" xpathType="element" name="Corporate" level="7" repeatable="true"
                searchable="true" showField="true" fieldType="viaf" searchFields="210__a; 111__a; 100__a; 110__a; 150__a; 151__a;"
                displayFields="001=NORM_IDENTIFIER; 0247_a=URI; 1001_a=NORM_NAME; 1001_d=NORM_LIFEPERIOD; 1001_q=NORM_SEX; 375__a=NORM_SEX;" />
```

![VIAF Feld](images/goobi-plugin-administration-archive-management_screen20_de.png)


## Konfiguration der Anzeige der Bereiche
In der Standardeinstellung sind die einzelnen Bereiche 1. `Identifikation`, 2. `Kontext`, 3. `Inhalt und innere Ordnung`, 4. `Zugangs- und Benutzungsbedingungen`, 5. `Sachverwandte Unterlagen`, 6. `Anmerkungen` und 7. `Verzeichnungskontrolle` aus Platzgründen eingeklappt und werden nicht angezeigt. Damit sie bereits beim auswählen eines Knotens ausgeklappt und angezeigt werdden, kann das Element `<showGroup level="1" />` verwendet werden. Über die Ordnungsnummer im Attribut level wird gesteuert, welcher Bereich ausgeklappt wird. Um auch unausgefüllte Metadaten gleich anzuzeigen, ohne sie mittels Badge hinzuzufügen, kann innerhalb der `<metadata>` Definition das Attribut `showField="true"` genutzt werden.


## Konfiguration des XML Namensraums
Die beiden Elemente `<eadNamespaceRead>` und `<eadNamespaceWrite>` legen fest, welche XML Namespaces zum lesen und schreiben von EAD Dokumenten verwendet werden sollen. Üblicherweise enthalten beide den gleichen Wert. Es können jedoch auch EAD2 Dokumente gelesen und als EAD3 Dokumente exportiert werden. Dann müssen die entsprechenden Namespaces definiert werden und bei den xpath Ausdrücken der einzelnen Metadaten darauf geachtet werden, dass beide Varianten angeggeben sind. Daher ist es einfacher, den beigelegten Konverter zu nutzen und die Konvertierung von EAD2 nach EAD3 vor dem einspielen der Dokumente zu machen.

- Namespace für ead2 (aktuell): urn:isbn:1-931666-22-9
- Namespace für ead3 (aktuell): http://ead3.archivists.org/schema/
- Namespace für ead4 (im draft Status): https://archivists.org/ns/ead/v4


## Konfiguration der verschiedenen Knotentypen

Die Knotentypen werden mit Hilfe des `<node>` Elements definiert.

```xml
        <node name="archive" icon="fa fa-archive" rootNode="true" allowProcessCreation="false">
            <child>folder</child>
        </node>

        <node name="folder" icon="fa fa-folder-open-o" allowProcessCreation="false">
            <child>folder</child>
            <child>file</child>
            <child>image</child>
            <child>audio</child>
            <child>video</child>
            <child>other</child>
        </node>

        <!-- item level nodes -->
        <node name="file" ruleset="File" icon="fa fa-file-text-o" allowProcessCreation="true" />
        <node name="image" ruleset="Picture" icon="fa fa-file-image-o" allowProcessCreation="true" />
        <node name="audio" ruleset="Audio" icon="fa fa-file-audio-o" allowProcessCreation="true" />
        <node name="video" ruleset="Video" icon="fa fa-file-video-o" allowProcessCreation="true" />
        <node name="other" ruleset="Other" icon="fa fa-file-o"  allowProcessCreation="true" />
```

Das Attribut `name` muss einen eindeutigen Wert enthalten, hierüber wird der Typ intern identifiziert. In `ruleset` steht der Name des Strukturelements im Regelsatz, der verwendet werden soll, wenn für diesen Knoten ein Vorgang erzeugt wird. Diese Funktion steht nur zur Verfügung, wenn das Attribut `allowProcessCreation` mit dem Wert `true` gesetzt ist. In `icon` wird das Symbol konfiguriert, dass in der Oberfläche für diesen Knotentyp angezeigt werden soll.

Mittels `rootNode="true|false"` wird definiert, ob der Knoten als oberstes Element des Archivbaums verwendet werden darf. In `<child>` können alle Knotentypen angegeben werden, die unterhalb des jeweiligen Typs erlaubt sind.