# Konfiguration der Nutzeroberfläche

## Übersicht

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_step_layoutwizzard
Repository               | [https://github.com/intranda/goobi-plugin-step-layoutwizzard](https://github.com/intranda/goobi-plugin-step-layoutwizzard)
Lizenz              | GPL 2.0 oder neuer 
Letzte Änderung    | 25.07.2024 14:16:44


Die Konfigurationsdatei des Goobi Step Plugins, das die Nutzeroberfläche für den LayoutWizzard darstellt, muss im `config` Verzeichnis der Goobi-Installation liegen. Ihr Dateiname lautet in aktuellen Goobi-Versionen `plugin_intranda_step_LayoutWizzard.xml`.

Üblicherweise lautet der vollständige Pfad zu dieser Konfigurationsdatei wie folgt:

```bash
/opt/digiverso/goobi/config/plugin_intranda_step_LayoutWizzard.xml
```

Der Inhalt dieser Konfigurationsdatei ist folgendermaßen aufgebaut:

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

**ACHTUNG:** Bitte beachten Sie, dass hier der korrekte Pfad zur Konfigurationsdatei des LayoutWizzards   innerhalb des Elements`<layout-wizzard-config-path>` angegeben ist.