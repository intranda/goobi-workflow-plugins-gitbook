The central configuration of the LayoutWizzard takes place in a separate configuration file. This file can be located anywhere in the file system, because its path can be specified in any program component of the LayoutWizzard. Usually the path to this central configuration file is as follows:

```bash
/opt/digiverso/LayoutWizzard/layoutwizzard_config.xml
```

The content of such a configuration looks exemplary as follows:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!-- intranda Layout Wizzard v1.1 configuration file -->

<config>
	<useOpenCV>false</useOpenCV>
	<!-- Higher values decrease the likelihood of images being labeled as outliers 
		("suspicious angle/pagesize") -->
    <outliers>        
       <type>NOT_PROCESSED</type>
	   <errorMultiplier>3.0</errorMultiplier>
	   <weightExponent>2.0</weightExponent>
    </outliers>
<!-- 	<contentServerUrl>http://G2GURL/goobi/cs/cs</contentServerUrl> -->
	<contentServerUrl>/cs/cs</contentServerUrl>
	<outputFolderSuffix>media</outputFolderSuffix>
	<analysisImagesBasePath>/opt/digiverso/git/layout-wizzard/LayoutWizzard/samples/debug
	</analysisImagesBasePath>
	<previews>
		<previewsPerPage>100</previewsPerPage>
		<maxPreviewsCached>100</maxPreviewsCached>
		<previewHeight>600</previewHeight>
		<largePreviewWidth>5000</largePreviewWidth>
	</previews>
	<processingThreads>4</processingThreads>
	<analysisTimeout>
		<duration>20</duration>
		<unit>SECONDS</unit>
	</analysisTimeout>
	<saving>
		<defaultCompression quality="85">JPEG
		</defaultCompression>
		<overwriteExistingImages>true</overwriteExistingImages>
		<ignoreImageMetadataErrors>false</ignoreImageMetadataErrors>
	</saving>

	<analysis id="bound_book">
    <info>
        <label>Bound book</label>
    </info>
		<pageMode>ALTERNATING_START_RIGHT</pageMode>
		<analysisStep name="PAGESKEW" use="true" order="1">
			<saveAnalysisImages visibility="INVISIBLE" path="deskew">false
			</saveAnalysisImages>
			<deskewerMode visibility="VISIBLE">ALL_EDGES</deskewerMode>
			<lineFinderMode visibility="INVISIBLE">OUTERCONTOURS
			</lineFinderMode>
			<lineGroupingMode visibility="INVISIBLE">GROUP_BY_DISTANCE
			</lineGroupingMode>
			<analysisImageSize>300</analysisImageSize>
			<rimAreaToIgnoreLines>0.05</rimAreaToIgnoreLines>
			<lowerCannyThreshold>2</lowerCannyThreshold>
			<cannyRatio>2</cannyRatio>
			<!-- <houghLineThreshold>10</houghLineThreshold> -->
			<minHoughLineLength>10</minHoughLineLength>
			<maxHoughLineGapSize>4</maxHoughLineGapSize>
			<featureSizeThreshold>5</featureSizeThreshold>
			<maxLineAngleDeviation>5</maxLineAngleDeviation>
			<maxLineDistance>10</maxLineDistance>
		</analysisStep>

		<analysisStep name="CONTENTAREA" use="true" order="2">
			<analysisImageSize>150</analysisImageSize>
			<saveAnalysisImages visibility="INVISIBLE" path="edgeDetection">false
			</saveAnalysisImages>
			<bitonalThreshold>220</bitonalThreshold>
			<featureSizeThreshold>2.0</featureSizeThreshold>
			<contentPadding visibility="VISIBLE">-10</contentPadding>
			<bitonalInvert visibility="HIDDEN">false</bitonalInvert>
			<rimAreaToIgnoreLines>0.0</rimAreaToIgnoreLines>
		</analysisStep>

		<analysisStep name="BOOKSPINE" use="true" order="3">
			<saveAnalysisImages visibility="INVISIBLE" path="spineDetection">false
			</saveAnalysisImages>
			<lineFinderMode visibility="HIDDEN">SUZUKICONTOURS
			</lineFinderMode>
			<lineGroupingMode visibility="HIDDEN">GROUP_BY_DISTANCE
			</lineGroupingMode>
			<croppingAggressiveness visibility="VISIBLE">CAUTIOUS
			</croppingAggressiveness>

			<analysisImageSize>400</analysisImageSize>
			<rimAreaToIgnoreLines>2</rimAreaToIgnoreLines>
			<lowerCannyThreshold>1</lowerCannyThreshold>
			<cannyRatio>3</cannyRatio>
			<minHoughLineLength>10</minHoughLineLength>
			<maxHoughLineGapSize>2</maxHoughLineGapSize>
			<featureSizeThreshold>0.1</featureSizeThreshold>
			<maxLineAngleDeviation>5</maxLineAngleDeviation>
			<maxLineDistance>5</maxLineDistance>
			<spineOffset visibility="VISIBLE">0</spineOffset>
		</analysisStep>
	</analysis>
	
	<analysis id="default">
	   [...]
	</analysis>
	
</config>
```

The configuration consists of some general settings and several `<analysis>` blocks. The `<analysis>` blocks mainly control the settings for the automatic analysis. Different projects or tasks can use different settings by passing the id of the `<analysis>` block to the automatic analysis.

General settings always affect all operations and are not overwritten by operation-specific settings.

## General settings

The following list of general configuration paths is not complete. However, it contains all configurations that must be individually adapted for an installation.

| Path | Description |
| :--- | :--- |
| `previews/previewsPerPage` | Number of images per page in the Goobi LayoutWizzard plugin preview |
| `previews/previewHeight` | Height of the thumbnail file in pixels displayed in the preview view. Smaller images allow faster display, but have a lower resolution. |
| `previews/largePreviewWidth` | Width in pixels of the thumbnail file displayed for the single-page view of the Goobi LayoutWizzard plugin Smaller images allow faster display, but have a lower resolution. |
| `processingThreads` | The maximum number of simultaneously running analysis or storage processes. This applies to Goobi and TaskManager separately. The images are processed sequentially for each operation. However, simultaneous processing may occur if several LayoutWizard jobs are running in parallel in the TaskManager. |
| `analsisTimeout/duration` | This value specifies the maximum time for analyzing or saving an image after which the execution for the image should be aborted. An analysis that was interrupted due to timeouts is noted, but the analysis of the following images is continued. The missing analysis data can be added in the manual control. However, a canceled save always ends the TaskManager job with an error. Useful values for the timeout are between 4 seconds and about one minute, depending on the performance and reliability of the system and the size and complexity of the images to be analyzed. |
| `analysisTimeout/unit` | This value defines the time unit in which analysisTimeout/duration is specified. Possible values are `MICROSECONDS`, `MILLISECONDS`, `SECONDS` and `MINUTES`. |
| `saving/defaultCompression` | This value determines the compression level that is used by default for saving the derivatives. Valid values here are `NONE` or `JPEG`. The `quality` attribute specifies the compression quality for JPEG compression. It must be between `0` and `100`. |
| `saving/overwriteExistingImages` | This value can be used to determine whether existing image derivatives should be overwritten during saving. |
| `saving/ignoreImageMetadataErrors` | Here you can specify whether the derivatives should also be saved if not all image metadata can be transferred. This can happen, for example, if unknown metadata exists for the Java image library. It is therefore advisable to always leave this value set to `false` as long as this setting is not explicitly required. |

## Analysis configuration

Each `<analysis>` block has an attribute `id` that controls which block is used for a particular analysis. The last block must have the `id="default"`. Settings from this block are always used if no analysis id is passed to an analysis call, or if a setting is not configured in the actually used block. All other blocks consist accordingly of the subset of configurations that differ from the `default` configuration.

The following settings can exist in each `<analysis>` block:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Path</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>info/label</code>
      </td>
      <td style="text-align:left">This is the name of the analysis setting in the plugin interface.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>pageMode</code>
      </td>
      <td style="text-align:left">This value defines the default page mode to be used. The specifications
        valid for this are defined within the <a href="../01_use/02_imageview/01_folders.md">folder and file options</a>.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>externalCommands/@use</code>
      </td>
      <td style="text-align:left">At this point it is determined whether the generation of images for the
        analysis and storage of the derivatives should be done by an external program.
        This can speed up the image generation considerably under certain circumstances,
        but it can also be more error-prone, since the generation then takes place
        outside of Java.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>externalCommands/convert</code>
      </td>
      <td style="text-align:left">This value defines the console command to call the external program for
        generating images. The execution specifics are appended to this command,
        following the format of <code>ImageMagick</code>. The called program must
        therefore be able to be called with parameters compatible with <code>ImageMagick</code>.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>analysisStep</code>
      </td>
      <td style="text-align:left">This value contains all internal parameters of the respective automatic
        analysis step.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>analysisStep/@name</code>
      </td>
      <td style="text-align:left">
        <p>This defines the internal name of the block. It must correspond to one
          of the following values:</p>
        <ul>
          <li><code>PAGESKEW</code>: Align Page</li>
          <li><code>CONTENTAREA</code>: Cutting the page</li>
          <li><code>BOOKSPINE</code>: Detect the book spine</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>analysisStep/@use</code>
      </td>
      <td style="text-align:left">This value can be used to determine whether an analysis step should be
        used. The value <code>false</code> deactivates the analysis step.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>analysisStep/@order</code>
      </td>
      <td style="text-align:left">At this point the sequence of the analysis step within the entire analysis
        is determined.</td>
    </tr>
  </tbody>
</table>

The settings in the `<analysisStep>` blocks concern specific parameters of the analysis algorithms. They are not described further here. However, users can potentially adjust any parameter in the interface. If the settings made in this way prove sufficient to be adopted in the configuration, the corresponding block in the configuration file can be set to the new value. The appropriate parameter block can be determined by finding the `<analysisStep>` for the respective analysis step in the configuration file and changing the block with the internal parameter name there. The internal parameter name is displayed in the user interface as a tooltip when the mouse pointer is held over the label of the changed parameter.

Additionally, all analysis parameter blocks can have the `visibility` attribute, which controls the visibility of the parameter in the user interface. If this attribute is missing, the default value `HIDDEN` is used.

| Visibility | Description |
| :--- | :--- |
| `VISIBLE` | The parameter is always visible in the interface when the corresponding step is selected. |
| `HIDDEN` | The parameter is only visible in the user interface when the analysis step block in the user interface is in extended mode. |
| `INVISIBLE` | The parameter is not displayed at all in the interface. |