The configuration file of the Goobi Step plug-in, which is the user interface for LayoutWizzard, must be located in the `config` directory of the Goobi installation. Its file name in current Goobi versions is `plugin_intranda_step_LayoutWizzard.xml`.

Usually the full path to this configuration file is as follows:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_LayoutWizzard.xml
```

The content of this configuration file is structured as follows:

```xml
<config_plugin>
  <config>
    <project>*</project>
    <step>*</step>
    <layout-wizzard-config-path>
        /opt/digiverso/LayoutWizzard/layoutwizzard_config.xml
    </layout-wizzard-config-path>
    <startPage>PREVIEW</startPage>
	  <!--<startPage>SINGLEVIEW</startPage>-->
    <previewMode>ALTERNATING</previewMode>
    <contentBorder>
        <width>2</width>
        <color>#00fa9a</color>
        <opacity>1</opacity>
    </contentBorder>
    <spineMarker>
        <outer>
            <linewidth>20</linewidth>
            <linecolor>#D73946</linecolor>
            <opacity>0.1</opacity>
        </outer>
        <inner>
            <linewidth>2</linewidth>
            <linecolor>#ff0000</linecolor>
            <opacity>1</opacity>
        </inner>
    </spineMarker>
	<preview>
		<cropFrame>
			<linewidth>2</linewidth>
	        <linecolor>#44ffdd</linecolor>
	        <fillcolor>#f1f2f3</fillcolor>
	        <clickradius>20</clickradius>
		</cropFrame>
		<spineMarker>
			<linewidth>2</linewidth>
	        <linecolor>#ff3344</linecolor>
	        <clickradius>20</clickradius>
		</spineMarker>
	</preview>
    <previewCroppingOptions>
        <show>true</show>
    </previewCroppingOptions>
    <previewOrientationSelect>
        <show>false</show>
    </previewOrientationSelect>
    <globalCroppingOptions>
        <show>true</show>
        <unit>mm</unit>
        <keyboardControls use="true">
            <moveMaskKey>SHIFT</moveMaskKey>
            <moveMaskKey>CTRL</moveMaskKey>
            <resizeMaskKey>SHIFT</resizeMaskKey>
            <stepSize>0.1</stepSize>
        </keyboardControls>
    </globalCroppingOptions>
    <info show="true">
        <spine>
            <format>Falz: {f}{u}</format>
        </spine>
    </info>
  </config>
</config_plugin>
```

**ATTENTION:** Please make sure that the correct path to the LayoutWizzard configuration file within the`<layout-wizzard-config-path>` element is specified here.