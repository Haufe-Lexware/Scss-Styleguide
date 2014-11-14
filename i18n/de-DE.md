Scss-Styleguide
===============

Denkweise
---------
* [OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/) / [SMACSS](http://smacss.com/)
* Progressive Enhancement
* Responsive Entwicklung: Mobile first

Inhaltsverzeichnis
------------------

1. [Ordnerstruktur](#folderstructure)
2. [Strukturiere ein Modul](#structure-a-module)
3. [Dateien importieren](#importing-files)
4. [Namenskonventionen (BEM/SMACSS)](#naming-conventions)
5. [Kommentare (Sassdoc)](#comments)
6. [Variablen](#variables)
7. [Breakpoints verwenden](#handling-breakpoints)
8. [Kategorisierung von CSS-Regeln](#categorizing-css)
9. [Hinweise bezüglich CSS](#css-stuff)
10. [Anderes](#other)

<a name="folderstructure">Ordnerstruktur</a>
-----------

Es ist unglaublich wichtig eine gute Struktur für den Entwicklungsprozess zu haben und ist sozusagen die Basis von allem. Besonderes Augenmerk sollte darauf gelegt werden, dass Strukturen in jeglichen Projekten gleich sind oder sich zumindest ähneln - es wird besonders im Team mit anderen Entwicklern helfen, sich zurecht zu finden in fremden Projekten.
Daher werde ich dir nun meine persönliche Struktur, basierend auf SMACSS, zeigen:

```scss
scss/
|- _base/
|  |- _config.scss
|  |- _presets.scss
|  |- _headings.scss
|  |- ...

|- _layouts/
|  |- _l-base.scss
|  |- _l-grid.scss

|- _modules/
|  |- _m-buttons.scss
|  |- _m-tabs.scss

|- application.scss

stylesheets/
|- application.css
```

<a name="structure-a-module">Strukturiere ein Modul</a>
-----------

Eine Struktur für Module? Man könnte meinen, dass es zu weit ginge, doch das denke ich eher nicht. Es kann deutlich an Komplexität, durch diverse Variationen, gewinnen. Daher ist es besonders wichtig eine Struktur zu schaffen die weiterhin Übersichtlichkeit schafft.

```scss
@charset "UTF-8";

// Config
$button-bgcolor: $blue;
$button-fontcolor: $white;

// Base
.m-button {}
.m-button--primary {}
.m-button--secondary {}
```

#### Modul mit ~~drei~~ zwei verschiedenen Bereichen
* Zuerst der Bereich für eine Konfiguration. Dort können unteranderem Farben definiert werden die für das Modul notwendig sind.
* Nun kommen jegliche Styles für das Modul.
* ~~Die Fußzeile ist für die Datei gedacht, die jeglichen Status eines Moduls enthält. Mehr nicht.~~


<a name="importing-files">Dateien importieren</a>
-----------

Ein Projekt besteht meist aus sehr vielen Bausteinen. Ich mag es nicht, wenn man das Inkludieren von Dateien zu sehr verschachelt. Daher bevorzuge ich es eine Datei zu haben, worin man alle Bausteine importiert - in meinem Fall die `application.scss`. Dafür habe ich eine Struktur, wie in diesem Beispiel, entwickelt:

```scss
// Base
@import "_base/_config",
        "_base/_presets",
        "_base/_headings";

// Layouts
@import "_layouts/_l-default";

// Modules
@import "_modules/_m-buttons",
        "_modules/_m-tabs";
```

### Zusammengefasst:
* Strukturiere in Base, Layout und Module
* Für jeden Berdeich nur einmal `@import` deklarieren
* Entferne die Dateiendung

### Blähe deine Dateien nicht auf
Wenn du ein großes Projekt mit sehr vielen Modulen und Varianten hast, dann musst du deine jetzige Struktur in Frage stellen. Es kann schnell passieren, dass man sehr viel Code in einer Datei hat und dann wird es schwer sie zu pflegen.

Daher müssen wir kleine Verbesserung an der Struktur vornehmen und Ordner hinzufügen, wenn du merkst dass die Dateien zu groß werden. Also teilen wir diese noch einmal in kleine Bausteine auf und importieren alle in einer Datei.

```scss
// Base
@import "_base/_config",
        "_base/_presets",
        "_base/_headings";

// Layouts
@import "_layouts/_l-default";

// Module
@import "_modules/_forms/_m-forms";
```

In der `_m-forms.scss` musst alle Bausteine vom Modul "Form" importieren - sie befinden sich im Ordner `_forms`. Die Struktur kann wie in dieser Datei aussehen:

```scss
// Bausteine
@import "_m-form-base",
        "_m-form-two-one-small",
        "_m-form-row",
        "_m-form-with-prefix";
```

**Du musst selber entscheiden, welchen Weg du bevorzugst - die Entscheidung ist abhängig von dem Projekt, welches du umsetzen möchtest. Daher kann man nicht sagen, welcher Weg der Beste ist.**

<a name="comments">Kommentare (Sassdoc)</a>
-----------

Es ist unglaublich wichtig den Code sauber zu dokumentieren. Wenn du länger nicht an einem Projekt gearbeitet hast und dann wieder darauf schaust, dann wirst du dich fragen was du dort geschrieben hast. Es ist schwer alles nachzuvollziehen ohne Kommentare und daher gebe ich dir den Ratschlag, besonders in Sass, *[Sassdoc](https://github.com/SassDoc/sassdoc)* zu benutzen. Es ist ein schönes Tool um eine solide Dokumentation für Variablen, Mixins, Funktionen etc. zu erzeugen. Es müssen nur mehr Kommentare geschrieben werden, aber du profitierst davon.

```scss
/**
 * Get the width of a column
 *
 * @author Tim Hartmann
 * @group addons
 *
 * @param {number} $col - define which column do you need
 * @param {number} $max-cols (12) - maximum columns
 *
 * @example scss
 * col(4);
 * // 25%
 */
@mixin col($col, $max-cols: 12) {
  width: (100%/$max-cols)*$col;
}

```
*In der ersten Zeile von deinem Kommentar schreibt du eine kleine Beschreibung über den Schnipsel. Es hilft einen groben Einblick zu bekommen.*

`@author`: Gib den Namen des Autors an. <br>
`@group`: Schnipsel zu gruppieren kann der Struktur helfen <br>
`@param`: Mögliche Parameter für ein Mixin / eine Funktion und beschreibe den erwarteten Datentyp. <br>
`@example`: Schreibe ein kurzes Beispiel wie du es benutzen kannst


**[Beispiel](http://scss-components.com/sassdoc/)** <br/>
**[Sassdoc Dokumentation](https://github.com/SassDoc/sassdoc)**


<a name="naming-conventions">Namenskonvention (BEM/SMACSS)</a>
-----------

### Block, Element, Modifizierung / Skalierbare und modulare Architektur in CSS
Ich liebe beide Wege. Wie auch immer, SMACSS ist ein Muss für jeden Frontend-Entwickler, der skalierbare Frontends bauen möchte. Jonathan Snook ist der Autor und hat seine Erfahrung in diesem Buch niedergeschrieben. In meinen Augen ist die Kombination aus beiden Techniken der beste Weg für die Namenskonvention.

```scss
// Layouts
// Prefix "l-"
.l-default {}

// Module
// Prefix "m-"
.m-accordion {}

// Kindselement des Akkordions
// Seperator: "__"
.m-accordion__trigger {}

// Modifizierung des Akkordions
// Seperator: "--"
.m-accordion--plain {}

// Status
// Meistens mit dem Prefix wie "is-"
.is-active {}
.is-hidden {}
```

<a name="variables">Variablen</a>
-----------

Zuerst wird eine Palette mit allen Farben gebaut, die für den globalen Einsatz gedacht ist - alles in der `_config.scss`. Wenn du modulspezifische Variablen hast, dann packe sie in die Datei des passenden Moduls. Lesbarkeit ist sehr wichtig und daher benutze ich einen Trenner in der Benamung von Variablen: ein **Bindestrich**.

```scss
// Farben (global)
$white: #fff;
$blue: #1675d6;
$red: #e8402a;

// Spezielle Benamung
$button-bgcolor: $blue;
$button-fontcolor: $white;
```

<a name="handling-breakpoints">Breakpoints verwenden: "Element Queries"</a>
-----------

"Element Queries"? Was schreibst du da, Tim? Uhh, "Element Queries" sind so viel besser mit Sass als Media Queries. Weißt du noch, als du all deine Styles für das anpassbare Verhalten in einer eigenen Datei ausgelagert hast? Meiner Meinung nach ist das schlecht. Wenn du ein Modul schreibst, dann solltest du auch dort das Verhalten regeln. **Nicht in einer anderen Datei.**

```scss
// Scss
.m-navigation {
  &:after {
    @media screen and (min-width: 767px) {
      content: '';
      display: inline-block;
      width: 100%;
    }
  }
  li {
    display: block;

    @media screen and (min-width: 767px) {
      float: left;
    }
  }
}

// Generiertes CSS
@media screen and (min-width: 767px) {
  .m-navigation:after {
    content: '';
    display: inline-block;
    width: 100%;
  }
}

.m-navigation li {
  display: block;
}

@media screen and (min-width: 767px) {
  .m-navigation li {
    float: left;
  }
}
```

Für die Performance ist es nicht ausschlaggebend, dass man nur einmal einen Media Query deklariert. Wir können es wie "Element Queries" benutzen. **Du kannst mehr darüber in [diesem Blogpost](http://helloanselm.com/2014/web-performance-one-or-thousand-media-queries/) von Anselm Hannemann lesen.**


<a name="categorizing-css">Kategorisierung von CSS-Regeln</a>
-----------

Wenn ich Scss schreibe, dann bin ich sehr konsequent und möchte Regeln für jegliche Dinge festlegen. Ich hab ein paar für die Kategorisierung aufgestellt:

```scss
// Basis
.element {
  margin: 10px;
  padding: 10px;

  border: 1px solid red;
  background: blue;

  color: white;
  font-size: 16px;

  -webkit-transform: translate3d(0,0,0);
}
```

* Box
  * `margin`, `padding`, `display`, etc.
* Border & Background
  * `border`, `border-radius`, `background-color` and so on
* Text
  * `color`, `font-size`, `text-transform`, etc.
* Anderes
  * `animations`, `transform`'s, ...


<a name="css-stuff">Hinweise bezüglich CSS</a>
-----------

### Vermeide IDs
IDs sind sehr spezifisch. Meistens sind sie nur notwendig für Funktionen in Javascript. Der bessere Weg sind Klassen.

### Vermeide `!important`
Wenn du `!important` benötigst, dann ist es mit deiner Architektur in CSS komisch. Du benötigst dies nur, wenn du Styles hart überschreiben möchtest. Wenn du eine saubere Struktur hast, dann musst du das nicht verwenden - es ist wie als wenn du mit einem Vorschlaghammer die Nuss knacken willst.

### Vermeide Kinds-Selektoren
Warum sollte man dies vermeiden? Ist es nicht ein cooles CSS-Feature? Ich denke eher nicht.
Als ich damals mit dem Coding angefangen habe, dort habe ich es sehr oft verwendet. Wie auch immer, ich habe gelernt dass es ein Modul zu spezifisch macht. Dadurch basiert das CSS auf die Struktur des Moduls und daher sind Änderung daran weniger möglich. **Benutze mehr Klassen!**

```scss
// Bad example
// Child-selector
.m-tabs > li {}

// Good example
// Classname
.m-tabs .m-tabs__trigger {}

// Better example
// Less nesting
.m-tabs__trigger {}
```

<a name="other">Sonstiges</a>
-----------

* Benutze überall Leerzeichen anstatt Tabs
* Ein Leerzeichen zwischen dem Element und der Klammer
* Zeilenumbruch zwischen Styles und neuen Elementen
* Zeilenumbruch nach der geschlossenen Klammer
* Benutze HEX Codes statt RGB Codes für Farben
* Wandle HEX Codes in RGB mit Sass um
* Schlechtes Beispiel:
  * color: ```rgba(255,255,255,.3);```
* Gutes Beispiel:
  * color: ```rgba(#000, .3);```

```scss
// Die selben Styles für mehr als ein Element
// Trennung durch einen Zeilenumbruch
.element,
.second-element {
  color: red;
}

.m-tabs {
  background: rgba($black, .3);
}
```