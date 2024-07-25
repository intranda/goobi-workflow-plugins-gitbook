# Installation

## Overview

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_step_layoutwizzard
Repository               | [https://github.com/intranda/goobi-plugin-step-layoutwizzard](https://github.com/intranda/goobi-plugin-step-layoutwizzard)
Licence              | GPL 2.0 or newer 
Last change    | 25.07.2024 14:16:44


## Program Libraries

The installation consists of a total of four program libraries that must be accessible in Apache Tomcat or Goobi:

| File | Location |
| :--- | :--- |
| `layoutwizzard.jar` | In the `lib` folder of the Goobi webapp in the Tomcat |
| `plugin_intranda_step_LayoutWizzard.jar` | In the `plugins/step` folder in the Goobi working directory |
| `plugin_intranda_step_LayoutWizzard-GUI.jar` | In the `plugins/GUI` folder in the Goobi working directory |

## Configuration files

In addition to these program files, two configuration files are required, one for the Goobi plug-in and one for the underlying LayoutWizzard program library.

### Plugin configuration

The configuration file of the plugin `plugin_LayoutWizzardPlugin.xml` must be located in the `config` configuration directory within the Goobi working directory. This is usually the path to the file:

```bash
/opt/digiverso/goobi/config/plugin_LayoutWizzardPlugin.xml
```

Within this configuration file the path to the actual central configuration of the LayoutWizard is specified. The structure of this file looks like this:

```xml
<!-- Goobi Plugin configuration file -->
<config_plugin>
	<config>
		<!-- which projects to use for (can be more then one, otherwise use *) -->
        <project>*</project>
        <!-- which steps to use for (can be more then one, otherwise use *) -->
        <step>*</step>
        
        <!-- The absolute path to the layoutwizzard_config.xml to use -->
	    <layout-wizzard-config-path>
	        /opt/digiverso/LayoutWizzard/layoutwizzard_config.xml
	    </layout-wizzard-config-path>
	    
	    <!-- The initial view of the LayoutWizzard Plugin. Can be one of
	    	* PREVIEW: The preview page of the plugin, displaying all pages in a vertical list
	    	* SINGLEVIEW: The single view page of the plugin, displaying all settings and the first page
	    	* SIMPLECROP: A zoomable cropping view for processes with few large images. Does not support spne cropping-->
	    <startPage>PREVIEW</startPage>
	    
	    <!-- The initial viewing mode if the PREVIEW view. One of 
	    	* ALTERNATING to see only left/right images per page
	        * DOUBLE_PAGE to see opposite pages next to each other -->
	    <previewMode>ALTERNATING</previewMode>
	    
	    <!-- Config for appearance of single/large image -->
		<singleImage>
			<!-- Config for frame around the crop area -->
			<cropFrame>
				<!-- Thickness of frame -->
				<linewidth>2</linewidth>
				<!-- Color of frame -->
				<linecolor>#00fa9a</linecolor>
				<!-- Radius around the frame in which the frame can be selected for drag/resize -->
				<clickradius>20</clickradius>
				<!-- The color filling the area outside the crop frame in the Save View -->
				<fillcolor>#ffffff</fillcolor>
			</cropFrame>
			<!-- Config for line marking the spine position -->
			<spineMarker>
				<!-- Thickness of line -->
				<linewidth>2</linewidth>
				<!-- Color of line -->
				<linecolor>#ff0000</linecolor>
				<!-- Radius around the line in which the line can be selected for dragging -->
				<clickradius>20</clickradius>
			</spineMarker>
		</singleImage>
	    <!-- Config for appearance of images in preview mode -->
		<preview>
			<!-- Config for frame around the crop area -->
			<cropFrame>
				<!-- Thickness of frame -->
				<linewidth>2</linewidth>
				<!-- Color of frame -->
				<linecolor>#00fa9a</linecolor>
				<!-- Radius around the frame in which the frame can be selected for drag/resize -->
				<clickradius>20</clickradius>
				<!-- The color filling the area outside the crop frame in the Save View -->
				<fillcolor>#f1f2f3</fillcolor>
			</cropFrame>
			<!-- Config for line marking the spine position -->
			<spineMarker>
				<!-- Thickness of line -->
				<linewidth>2</linewidth>
				<!-- Color of line -->
				<linecolor>#ff0000</linecolor>
				<!-- Radius around the line in which the line can be selected for dragging -->
				<clickradius>20</clickradius>
			</spineMarker>
		</preview>
		
		<!-- Display cropping control elements in preview mode -->
	    <previewCroppingOptions>
	        <show>true</show>
	    </previewCroppingOptions>
	    
	    <!-- Display the option to select page orientation in preview mode (may be prone to errors) -->
	    <previewOrientationSelect>
	        <show>false</show>
	    </previewOrientationSelect>
	    
	    <!-- Options for Global cropping options  -->
	    <globalCroppingOptions>
	    	<!-- Display Global cropping options in single page mode -->
	        <show>true</show>
	        <!-- Unit to display crop frame coordinates in. One of
	        	* μm
	        	* mm
	        	* cm
	        	* in -->
	        <unit>mm</unit>
	        <!-- Settings for keyboard controls of crop frame. set use="true" to allow changing crop frame using keyboard shortcuts -->
	        <keyboardControls use="true">
	        	<!-- Key(s) to keep pressed when moving the frame. Can be SHIFT, CTRL or ALT -->
	            <moveMaskKey>SHIFT</moveMaskKey>
	            <moveMaskKey>CTRL</moveMaskKey>
	        	<!-- Key(s) to keep pressed when resizing the frame. Can be SHIFT, CTRL or ALT -->
	            <resizeMaskKey>SHIFT</resizeMaskKey>
	            <!-- Amount of units (see above) to move the frame for each key press event -->
	            <stepSize>0.1</stepSize>
	        </keyboardControls>
	    </globalCroppingOptions>
	    
	    <!-- Display information about the the cropping/deskewing value of the current step -->
	    <info show="true">
	    	<!-- Display format for spine location. {f} is a floating number, {u} the appropriate unit -->
	        <spine>
	            <format>Falz: {f}{u}</format>
	        </spine>
	    </info>
    </config>
</config_plugin>
```

### LayoutWizzard configuration

The actual configuration file specifies various parameters for the layout analysis process. These parameters are listed as examples in the following configuration file. As defined in the plugin configuration file, it is located under the following path:

```bash
/opt/digiverso/intranda/LayoutWizzard/layoutwizzard_config.xml
```

As an example, this configuration file has the following content:

```xml
<!-- intranda Layout Wizzard configuration file -->
<config>
    <contentServerUrl>http://demo03.intranda.com/goobi/cs/cs</contentServerUrl>
    <defaultOutputFolderSuffix>media</defaultOutputFolderSuffix>
    <analysisImagesBasePath>/home/florian/LayoutWizzard/samples/</analysisImagesBasePath>
    <previews>
        <previewsPerPage>100</previewsPerPage>
        <maxPreviewsCached>10000</maxPreviewsCached>
        <previewWidth>700</previewWidth>
        <imageHeightLarge>800</imageHeightLarge>
    </previews>
    <outliers>
        <errorMultiplier>3.0</errorMultiplier>
        <weightExponent>2.0</weightExponent>
    </outliers>
    <saving>
        <defaultCompression quality="85">NONE</defaultCompression>
        <overwriteExistingImages>true</overwriteExistingImages>
    </saving>
    <analysis id="firstPageLeft">
        <firstPageOrientation>LEFT</firstPageOrientation>
    </analysis>
    <analysis id="firstPageRight">
        <firstPageOrientation>RIGHT</firstPageOrientation>
    </analysis>
    <analysis>
        <analysisStep name="PAGESKEW" type="edges" use="true">
            <saveAnalysisImages visibility="INVISIBLE" path="DESKEW">false</saveAnalysisImages>
            <deskewerMode visibility="VISIBLE">ALL_EDGES</deskewerMode>
            <lineFinderMode>CONTOURS</lineFinderMode>
            <lineGroupingMode>GROUP_BY_DISTANCE</lineGroupingMode>
            <featureSizeThreshold>10.0</featureSizeThreshold>
            <analysisImageSize>300</analysisImageSize>
            <lowerCannyThreshold>70</lowerCannyThreshold>
            <cannyRatio>2</cannyRatio>
            <distanceResolution>1</distanceResolution>
            <angleResolution>1</angleResolution>
            <minHoughLineLength>10</minHoughLineLength>
            <houghLineThreshold>50</houghLineThreshold>
            <maxHoughLineGapSize>2</maxHoughLineGapSize>
            <maxLineAngleDeviation>5</maxLineAngleDeviation>
            <maxLineDistance>7</maxLineDistance>
            <rimAreaToIgnoreLines>0.0</rimAreaToIgnoreLines>
        </analysisStep>
        <analysisStep name="CONTENTAREA" use="true">
            <analysisImageSize>0</analysisImageSize>
            <saveAnalysisImages visibility="INVISIBLE" path="edgeDetection">false</saveAnalysisImages>
            <bitonalThreshold>150</bitonalThreshold>
            <bitonalInvert>false</bitonalInvert>
            <featureSizeThreshold>10.0</featureSizeThreshold>
            <contentPadding visibility="VISIBLE">0</contentPadding>
        </analysisStep>
        <analysisStep name="BOOKSPINE" use="true">
            <analysisImageSize>400</analysisImageSize>
            <saveAnalysisImages visibility="INVISIBLE" path="spineDetection">false</saveAnalysisImages>
            <lineFinderMode>CONTOURS</lineFinderMode>
            <lineGroupingMode visibility="INVISIBLE">GROUP_BY_DISTANCE</lineGroupingMode>
            <croppingAggressiveness visibility="VISIBLE">BALANCED</croppingAggressiveness>
            <lowerCannyThreshold>40</lowerCannyThreshold>
            <cannyRatio>2</cannyRatio>
            <distanceResolution>1</distanceResolution>
            <angleResolution>1</angleResolution>
            <minHoughLineLength>20</minHoughLineLength>
            <houghLineThreshold>10</houghLineThreshold>
            <maxHoughLineGapSize>4</maxHoughLineGapSize>
            <maxLineAngleDeviation>5</maxLineAngleDeviation>
            <maxLineDistance>5</maxLineDistance>
            <featureSizeThreshold>0.1</featureSizeThreshold>
            <rimAreaToIgnoreLines>0.0125</rimAreaToIgnoreLines>
            <maxGroupAngleDeviation visibility="INVISIBLE">10</maxGroupAngleDeviation>
            <spineOffset visibility="VISIBLE">5</spineOffset>
        </analysisStep>
    </analysis>
</config>
```

For details on customizing the configurations, see the [Configuration section](02_configuration.md).