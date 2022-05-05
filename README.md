# Google Fonts DSGVO-gerecht nutzen - Spezialfall Mobirise
********************************************************

## Beispiele Mobirise (und CMS allgemein)
********************************************************

*Ein kleines Vorwort wie immer: Backups sind wichtig und man sollte immer eines haben. Und ohne Backup sollte man nicht an Quellcode oder Project Files rumschrauben. Was wir hier beschreiben, haben wir ausprobiert und es funktioniert. Trotzdem machst Du Deine Änderungen auf eigene Gefahr! Es gibt keinen Support zu diesem Workaround, mehr als diese Seite gibt es nicht. Wenn sich jemand darauf beziehen möchte, dann einen Link zu dieser Seite setzen.*
   
*Mobirise Version 5.6.8 / Stand: 5. Mai 2022*

---

### Layoutphase immer mit Google Font-Linking

Für Layoutzwecke ist es immer besser, *ZUNÄCHST EINMAL* Google Fonts per Hyperlink zu Google einzubetten, da man flexibel bleibt und immer ohne großen Auswand eine Schrift (mit allen Schnitten) sehr schnell und einfach einbinden und nutzen kann.
In einer Layoutphase spielt das eine große Rolle, die Entwicklung von Feinheiten bei der Typopgrafie bleibt einfach und intuitiv.

Nach Abschluss des Layouts, der Abnahme und der Fertigstellung aller Templates und Seitenlayouts *MÜSSEN* jedoch in jedem Fall die verlinkten Schriften durch selbst-gehostete Google Font Schriften ersetzt werden. Denn sonst ist die Website NICHT DSGVO konform und damit abmahnfähig. 

> Mobirise bietet einen direkten Zugang zu Google Fonts und lädt die gewünschte Schrift-Familie herunter, man kann problemlos und direkt damit arbeiten. Schnitte wie fett oder kursiv stehen dann innerhalb der Schriftfamilie zur Verfügung. Die Schriften werden automatisch herunter geladen und **im Projektordner gesichert**, damit man lokal auf dem Rechner WYSIWYG arbeiten kann. Lädt man ein solches Projekt jedoch auf den Webserver, verlässt man die rechtssichere Zone.

---

### Live immer ohne Google Font-Linking

Diese Schriften können im Fall von Mobirise im Projektordner auf dem Arbeitsrechner bleiben, **jedoch müssen die im Quelltext angelinkten Googleschriften durch Links ersetzt werden, die auf die auf dem Webserver hinterlegten Schriften verweisen.** Diese Schriften (Google Fonts) kann man zusammen mit dem passenden CSS-Code mit dem [Google Webfont Helper](https://google-webfonts-helper.herokuapp.com/fonts) herunterladen.

> Mobirise bietet zwar die Option, eigene Schriften zu verwenden, welche dann mit den anderen Daten auf den Webspace geladen werden. Das ist DSGVO-konform. Aaaaaber der große Nachteil davon ist, dass immer nur einzelne Schnitte (wie z.B. regular, bold oder italic) hinzugefügt werden können, keine Schriftfamilien. Die möglichen Formatzuweisungen im Editor von Mobirise sind damit praktisch wertlos, die automatisch generierten Schriftnamen unübersichtlich etc, kein Vergleich zum Bedienkomfort. Zum Layouten und Bearbeiten alles andere als komfortabel.

### Folgendes ist also zu tun:

- In der Layoutphase auf jeden Fall mit angelinkten, **externen** Google Fonts auf dem Testserver arbeiten, egal ob CMS-Template oder Mobirise.
- Nach Abschluss der Layoutphase sind **die verwendeten Schriftfamilien bei [Google Webfont Helper](https://google-webfonts-helper.herokuapp.com/fonts) samt CSS-Code herunterzuladen und auf dem Webserver zu speichern (am besten Schriften und CSS zusammen in einen Ordner namens »fonts«, wie hier im Beispiel beschrieben)**, egal ob CMS-Template oder Mobirise.
- WOFF und WOFF2 als Schriftformate genügen völlig.
- Das CSS-file wird im Google Webfont Helper automatisch erzeugt (in einer Box unter der Schriftauswahl kann man den Serverpfad ergänzen/ändern/korrekt eingeben). Den »kopieren«-Button klicken und den CSS Code in einer Datei sichern , z.B. als **fontstyles.css**
– Die fontstyles.css Datei zusammen mit den Schriften in einen Ordner namens »fonts« ablegen, der dann 
	- bei Mobirise im root des Webservers
	- beim CMS sinnvollerweise im Templateordner des Webservers abgelegt wird.
- Im CMS-Template **müssen die Links zum Google-Font-Server nun entfernt und durch einen Link zum selbst gehosteten CSS-Fontfile ersetzt werden**.
- **Im  Mobirise-Project File muss nun der Google Link durch einen lokalen Link ersetzt werden.**

---
### CMS Template updaten
********************************************************
Link zum Fonts Stylesheet in das PHP-Template einfügen.
Dieser Fontordner im Beispielcode liegt direkt im Template Verzeichnis (das kann man aber auch frei wählen, je nach Template, und muss den Code entsprechend anpassen, hier Beispiel WBCE, Erwähnung nur der Vollständigkeit halber) ...

    <link rel="stylesheet" type="text/css" href="<?php echo TEMPLATE_DIR; ?>/fonts/fontstyles.css" />


»fontstyles.css« liegt im Ordner »fonts«, und der liegt im Templateordner - der Pfad ist demzufolge /fonts/fontstyles.css

> **Ganz wichtig: Die alten https://fonts.googleapis.com/css?blablabla Links müssen aus dem Template bzw. Stylesheet vollständig entfernt werden!**

---
## Mobirise Project updaten
********************************************************

Das läuft komplett anders.

- Gehe in den Mobirise-Projektordner und öffne das betreffende **»project.mobirise«** mit einem Text-Editor
	- beim Mac: ~/Library/Application Support/Mobirise.com/Mobirise/projects/project-2022-05-05_123456/project.mobirise
- Kann man gut identifizieren anhand des Screenshots im Projektordner
- Kann man eindeutig identifizieren anahnd der ersten Zeilen im Projectfile
- Suche nach **»fontname«**

Dort innerhalb des Mobirise-Project File (Code ähnlich wie hier, Schriftnamen und Pfade können anders sein) muss nach Abschluss der Layoutarbeiten der **"link"**
```
"fontName": {
"name": "Libre Franklin",
"css": "'Libre Franklin', serif",
"link": "https://fonts.googleapis.com/css?family=Libre+Franklin:400,400i,600,600i"
            }
```
**geändert werden in**
```
"fontName": {
"name": "Libre Franklin",
"css": "'Libre Franklin', serif",
"link": "https://DEINEDOMAIN.COM/fonts/fontstyles.css"
            }
```
Code ähnlich wie hier, Schriftnamen und Pfade können anders sein. DEINEDOMAIN.COM ersetzen durch deine eigene Domain.
Ein relativer Link sollte aber auch funktionieren. **!Aber wir sind noch nicht fertig!**

### Und nun etwas ganz Wichtiges betreffend Mobirise
********************************************************

Mobirise generiert einen Teil des Codes automatisch und ändert den Link ebenso automatisch beim Uploaden zu **https://YOURDOMAIN.COM/fonts/fontstyles.css&display=swap**

**»&display=swap«** wurde automatisch hinzugefügt. Nein, das kann man nicht abschalten. Weshalb nicht der Link angepasst werden muss, **sondern bei Mobirise der Suffix des CSS-Dateinamens**.

Es **MUSS**
- der Dateiname des CSS von **fontstyles.css**
- zu **fontstyles.css&display=swap** geändert werden.

Jetzt sollte alles funktionieren. Fertig.

#### Öffne nun das Projekt in Mobirise und lade es auf den Webspace. Die HTML Dateien werden nun geändert und sollten keine fonts.googleapis.com-Links mehr enthalten. Zur Kontrolle mal in den Quelltext schauen, vielleicht hast Du was übersehen.

Die Definition der Schriftenlinks bleibt im Project File erhalten, solange man an den Projektschriften (in den Site Styles) **nichts** mehr ändert. Deshalb erst layouten, und dann coden ;-)
