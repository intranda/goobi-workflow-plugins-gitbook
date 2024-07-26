# Konfiguration des LayoutWizzards

## Übersicht

Name                     | Wert
-------------------------|-----------
Identifier               | intranda_step_layoutwizzard
Repository               | [https://github.com/intranda/goobi-plugin-step-layoutwizzard](https://github.com/intranda/goobi-plugin-step-layoutwizzard)
Lizenz              | GPL 2.0 oder neuer 
Letzte Änderung    | 25.07.2024 14:16:44


Die zentrale Konfiguration des LayoutWizzards findet in einer eigenständigen Konfigurationsdatei statt. Diese kann an einem beliebigen Ort im Dateisystem liegen, da ihr Pfad in jedem Programmbestandteil des LayoutWizzards angegeben werden kann. Üblicherweise lautet der Pfad zu dieser zentralen Konfigurationsdatei folgendermaßen:

```bash
/opt/digiverso/LayoutWizzard/layoutwizzard_config.xml
```

Der Inhalt einer solchen Konfiguration sieht beispielhaft wie folgt aus:

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

Die Konfiguration besteht aus einigen allgemeinen Einstellungen und mehreren `<analysis>`Blöcken. Die `<analysis>` Blöcke regeln im Wesentlichen die Einstellungen für die automatische Analyse. Verschiedene Projekte oder Vorgänge können dabei unterschiedliche Einstellungen verwenden, indem Sie der automatischen Analyse die `id` des `<analysis>` Blocks übergeben. 

Allgemeine Einstellungen betreffen immer alle Vorgänge und werden auch nicht durch vorgangsspezifische Einstellungen überschrieben.

## Allgemeine Einstellungen

Die folgende Liste an allgemeinen Konfigurationspfaden ist nicht vollständig. Sie enthält jedoch alle Konfigurationen die individuell für eine Installation angepasst werden müssen.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Pfad</th>
      <th style="text-align:left">Beschreibung</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>previews/previewsPerPage</code>
      </td>
      <td style="text-align:left">Anzahl an Bildern pro Seite der Vorschauansicht im Goobi-LayoutWizzard-Plugin</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>previews/previewHeight</code>
      </td>
      <td style="text-align:left">Höhe der Thumbnail-Datei in Pixeln, die in der Vorschauansicht angezeigt
        werden. Kleinere Bilder ermöglichen eine schnellere Anzeige, haben
        aber eine geringere Auflösung.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>previews/largePreviewWidth</code>
      </td>
      <td style="text-align:left">Breite der Thumbnail-Datei in Pixeln, die für die Einzelseiten-Ansicht
        des Goobi-LayoutWizzard-Plugins angezeigt wird. Kleinere Bilder ermöglichen
        eine schnellere Anzeige, haben aber eine geringere Auflösung.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>processingThreads</code>
      </td>
      <td style="text-align:left">Die maximale Anzahl gleichzeitig laufender Analyse- oder Speichervorgänge.
        Dies gilt für Goobi und TaskManager separat. Pro Vorgang werden die
        Bilder sequenziell bearbeitet. Eine gleichzeitige Bearbeitung kann jedoch
        vorkommen, wenn mehrere LayoutWizzard-Jobs im TaskManager parallel laufen.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>analsisTimeout/duration</code>
      </td>
      <td style="text-align:left">
        <p>Dieser Wert gibt die maximale Dauer für die Analyse oder das Speichern
          eines Bildes an, nach der die Ausführung für das Bild abgebrochen
          werden soll. Eine wegen Timeouts abgebrochene Analyse wird vermerkt, die
          Analyse der folgenden Bilder jedoch fortgesetzt. Die fehlenden Analysedaten
          können in der manuellen Kontrolle nachgetragen werden. Ein abgebrochenes
          Speichern beendet jedoch immer den TaskManager-Job mit einem Fehler.</p>
        <p>Sinnvolle Werte für den Timeout liegen zwischen 4 Sekunden und etwa
          einer Minute, je nach Leistung und Zuverlässigkeit des Systems, und
          der Grö&#xDF;e und Komplexität der zu analysierenden Bilder.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>analysisTimeout/unit</code>
      </td>
      <td style="text-align:left">Dieser Wert definiert die Zeiteinheit, in der <code>analysisTimeout/duration</code> angegeben
        wird. Mögliche Werte sind <code>MICROSECONDS</code>, <code>MILLISECONDS</code>, <code>SECONDS</code> und <code>MINUTES</code>.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>saving/defaultCompression</code>
      </td>
      <td style="text-align:left">Mit diesem Wert wird die Kompressionsstufe festgelegt, die standardmä&#xDF;ig
        für das Speichern der Derivate verwendet wird. Gültige Werte
        sind hierbei <code>NONE</code> oder <code>JPEG</code>. Das Attribut <code>quality</code> gibt
        die Kompressionsqualität bei JPEG-Kompression an. Sie muss zwischen <code>0</code> und <code>100</code> liegen.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>saving/overwriteExistingImages</code>
      </td>
      <td style="text-align:left">Mit diesem Wert kann festgelegt werden, ob bereits existierende Bildderivate
        während des Speicherns überschrieben werden sollen.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>saving/ignoreImageMetadataErrors</code>
      </td>
      <td style="text-align:left">Hier wird angegeben, ob die Derivate auch gespeichert werden sollen, wenn
        nicht alle Bildmetadaten übernommen werden können. Dies kann
        beispielsweise vorkommen, wenn für die Java-Bildbibliothek unbekannte
        Metadaten vorhanden sind. Es ist daher ratsam diesen Wert immer auf <code>false</code> zu
        lassen, solange diese Einstellung nicht explizit benötigt wird.</td>
    </tr>
  </tbody>
</table>

## Analysis-Konfiguration

Jeder `<analysis>` Block hat ein Attribut `id`, das regelt, welcher Block für eine bestimmte Analyse verwendet wird. Der letzte Block muss die `id="default"` haben. Einstellungen aus diesem Block werden immer verwendet, wenn einem Analyseaufruf keinen Analyse-Id übergeben wird, oder wenn eine Einstellung nicht im eigentlich verwendeten Block konfiguriert ist. Alle anderen Blöcke bestehen entsprechend aus der Teilmenge an Konfigurationen, die von der `default`-Konfiguration abweichen.

Folgende Einstellungen können in jedem `<analysis>` Block existieren: 

<table>
  <thead>
    <tr>
      <th style="text-align:left">Pfad</th>
      <th style="text-align:left">Beschreibung</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>info/label</code>
      </td>
      <td style="text-align:left">Hierbei handelt es sich um die Bezeichnung der Analyse-Einstellung in
        der Plugin-Oberfläche.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>pageMode</code>
      </td>
      <td style="text-align:left">
        <p>Dieser Wert definiert den standardmä&#xDF;ig zu verwendenden Seitenmodus.
          Die hierfür gültigen Angaben sind innerhalb der</p>
        <p><a href="../01_use/02_imageview/01_folders.md">Ordner- und Dateioptionen</a> beschrieben.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>externalCommands/@use</code>
      </td>
      <td style="text-align:left">An dieser Stelle wird festgelegt, ob die Erzeugung von Bildern für
        die Analyse und das Speichern der Derivate durch ein externes Programm
        erfolgen soll. Dies kann die Bilderzeugung unter Umständen erheblich
        beschleunigen, aber auch fehleranfälliger sein, da die Erzeugung dann
        au&#xDF;erhalb von Java stattfindet.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>externalCommands/convert</code>
      </td>
      <td style="text-align:left">Mit dieser Wert wird der Konsolenbefehl definiert, mit dem das externe
        Programm zum Erzeugen von Bildern aufgerufen werden soll. An diesen Befehl
        werden die Spezifika der Ausführung angehängt, die dem Format
        von <code>ImageMagick</code> folgen. Das aufgerufene Programm muss also mit
        Parametern aufgerufen werden können, die kompatibel zu <code>ImageMagick</code> sind.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>analysisStep</code>
      </td>
      <td style="text-align:left">Dieser Wert enthält alle internen Parameter den jeweiligen automatischen
        Analyseschritt.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>analysisStep/@name</code>
      </td>
      <td style="text-align:left">
        <p>Hiermit wird der interne Name des <code>&lt;analysisStep&gt;</code> Blocks
          festgelegt. Er muss einem der folgenden Werte entsprechen:</p>
        <ul>
          <li><code>PAGESKEW</code>: Seite ausrichten</li>
          <li><code>CONTENTAREA</code>: Seite zuschneiden</li>
          <li><code>BOOKSPINE</code>: Falz erkennen</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>analysisStep/@use</code>
      </td>
      <td style="text-align:left">Mit diesem Wert kann festgelegt werden, ob ein Analyseschritt verwendet
        werden soll. Der Wert <code>false</code> deaktiviert den Analyseschritt.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>analysisStep/@order</code>
      </td>
      <td style="text-align:left">An dieser Stelle wird die Reihenfolge des Analyseschrittes innerhalb der
        gesamten Analyse festgelegt.</td>
    </tr>
  </tbody>
</table>

Die Einstellungen in den `<analysisStep>` Blöcken betreffen spezifische Parameter der Analyse-Algorithmen. Sie werden hier nicht weiter beschrieben. Benutzer können jedoch in der Oberfläche potentiell jeden Parameter anpassen. Sollten sich so vorgenommene Einstellungen genug bewähren, um in die Konfiguration übernommen zu werden, kann der entsprechende Block in der Konfigurationsdatei auf den neuen Wert gesetzt werden. Der passende Parameter-Block kann hierbei ermittelt werden, indem der `<analysisStep>` zum jeweiligen Analyse-Schritt in der Konfigurationsdatei herausgefunden  und dort der Block mit dem internen Parameternamen geändert wird. Der interne Parametername wird in der Oberfläche als Tooltip angezeigt, wenn der Mauszeiger über das Label des veränderten Parameters gehalten wird.

Zusätzlich können alle Analyse-Parameter-Blöcke das Attribut `visibility` besitzen, das die Sichtbarkeit des Parameters in der Oberfläche regelt. Fehlt dieses Attribut wird der default-Wert `HIDDEN` verwendet.

| Sichtbarkeit | Beschreibung |
| :--- | :--- |
| `VISIBLE` | Der Parameter ist in der Oberfläche immer sichtbar, wenn der zugehörige Schritt ausgewählt ist. |
| `HIDDEN` | Der Parameter ist in der Oberfläche nur sichtbar, wenn der Analyseschritt-Block in der Oberfläche im erweiterten Modus ist. |
| `INVISIBLE` | Der Parameter wird in der Oberfläche gar nicht angezeigt. |