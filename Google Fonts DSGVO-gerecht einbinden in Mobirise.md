# Mit Mobirise Google Fonts DSGVO-gerecht nutzen
********************************************************



*NB: Backups sind wichtig und man sollte immer eines haben. Was ich hier beschreibe, habe ich ausprobiert und es funktioniert. Trotzdem machst Du Deine Änderungen auf eigene Gefahr! Es gibt von meiner Seite keinen Support zu diesem Workaround, mehr als diese Seite gibt es nicht.*
   
*Mobirise Version 5.9.4 / Stand: 28. September 2023 *
---

### Mobirise App mit Funktion für lokale Fonts (aber ziemlich schlecht)

Mobirise (ein nicht perfekter, aber praktischer und preiswerter Webbaukasten) bietet eine Funktion zur Verwendung lokaler Fonts. Man muss sie in ein Projekt importieren, und beim Publizieren werden die Fonts zusammen mit den generierten HTML Files per FTP in dein Web-Root geladen. Das Problem dabei: Schriftfamilien kann man so nicht nutzen, nur einzelne Schnitte. was zielmich unbequem für eine Layousphase ist.

**Zudem werden die Fontdateien beim Upload in TTF Truetype Fonts umgewandelt. Dadurch wächst die Detenmenge der Fonts (die von webroot zu laden sind) ganz erheblich. Sehr schnell entstehen dabei TTF Fontschnitte, die 500K haben. Zum Vergleich: ein WOF2 Webfont des gleichen Schirtschnittes hat gerademal 40K, weniger als ein Zehntel der Datenmenge.**

> Also muss der Wunsch sein, a) Google Fonts zu verwenden, b) beim Layouten in Mobirise keine Einschränkung zu haben, c) WOF2 Webfonts auf dem Webspace zu benutzen und d) dabei DSGVO compliant zu sein. 

### Layoutphase immer mit Google Font-Linking

Für Layoutzwecke ist es immer besser, *ZUNÄCHST EINMAL* Google Fonts per Hyperlink zu Google einzubetten, da man flexibel bleibt und immer ohne großen Auswand eine Schrift (mit allen Schnitten) sehr schnell und einfach einbinden und nutzen kann.
In einer Layoutphase spielt das eine große Rolle, die Entwicklung von Feinheiten bei der Typopgrafie bleibt einfach und intuitiv.

Nach Abschluss des Layouts, der Abnahme und der Fertigstellung aller Templates und Seitenlayouts *MÜSSEN* jedoch in jedem Fall die verlinkten Schriften durch selbst-gehostete Google Font Schriften ersetzt werden. Denn sonst ist die Website NICHT DSGVO konform und damit abmahnfähig. Ich beschreibe hier, wie das klappt

> Mobirise bietet einen direkten Zugang zu Google Fonts. Man lädt die gewünschte **Schrift-Familie** herunter, und kann problemlos und direkt damit arbeiten. **Schnitte wie fett oder kursiv stehen dann innerhalb der Schriftfamilie zur Verfügung**. Die Schriften werden automatisch herunter geladen und im Projektordner gesichert, **damit man lokal auf dem Rechner WYSIWYG arbeiten kann**. 

Lädt man ein solches Projekt jedoch einfach ohne Anpassung auf den Webserver, verlässt man die rechtssichere Zone. Denn die Schriften werden über einen Link direkt von Google geladen, und nicht wie vorgeschrieben, vom eigenen Server

---

### Website IMMER ohne Google Font-Linking

Die verwendeten Schriften können im Mobirise Projektordner auf dem Arbeitsrechner bleiben.

**Vor dem Uplaod des Mobirise Projetes müssen die verwendeten Googleschriften in WOF2 Webfonts konvertiert verwenden** Diese Google Fonts kann man zusammen mit dem passenden CSS-Code mit dem [Google Webfont Helper](https://google-webfonts-helper.herokuapp.com/fonts) generieren und herunterladen.

Diese Webfonts und das CSS-File müssen händisch ins Root des Test- oder Webservers kopiert werden. Sie werden dort in einem eigenen Verzeichnis namens »webfont« abgelegt.

Dann wird die Projektdatei vom Mobirise auf deinem Rechner dahin gehend verändert, dass der beim Export generierte HTML Code keine Links mehr zu Google enthält und stattdessen einen Link zum CSS-File im »webfont« Ordner enthält.


### Wie macht man das?

- In der Layoutphase auf jeden Fall mit angelinkten, **externen** Google Fonts auf dem Testserver arbeiten.
- Nach Abschluss der Layoutphase werden **die verwendeten Google Schriftfamilien bei [Google Webfont Helper](https://google-webfonts-helper.herokuapp.com/fonts) samt CSS-Code als WOF2 Webfonts generiert
- **Die WOF2 Webfonts und das dazugehörige CSS werden zusammen in einen Ordner namens »webfonts« im Root des Webservers gesichert**.
- WOFF2 als Schriftformat genügt völlig.
- Das CSS-file wird im Google Webfont Helper automatisch erzeugt (in einer Box unter der Schriftauswahl kann man den Serverpfad ergänzen/ändern/korrekt eingeben). Den »kopieren«-Button klicken und den CSS Code in einer Datei sichern , z.B. als **fontstyles.css**
– Die fontstyles.css Datei zusammen mit den Schriften in einen Ordner namens »webfonts« ablegen, der bei Mobirise im root des Webservers liegen muss.
- **Im  Mobirise-Project File muss nun der Google Link durch einen lokalen Link ersetzt werden. Das geht folgendermaßen …**

---
## Mobirise Project patchen
********************************************************

- Gehe in den Mobirise-Projektordner und öffne das betreffende **»project.mobirise«** mit einem Text-Editor
	- beim Mac: ~/Library/Application Support/Mobirise.com/Mobirise/projects/project-2023-09-28_XXXXXXXX/project.mobirise
- Kann man gut identifizieren anhand der Screenshots im Projektordner
- Kann man eindeutig identifizieren anhand der ersten Zeilen im Projectfile: Zeile 3 ("name": "DEIN_PROPJEKTNAME")
- Suche nach **»sitefonts«** (etwa Zeile 44)

*Innerhalb des Mobirise-Project File* (Code ähnlich wie hier, **mein Schriftbeispiel ist Fira**. Schriftnamen und Pfade können bei Dir anders sein) muss nach Abschluss der Layoutarbeiten die **"url"** von einem absoluten Link zu Google in einen relativen Link zu deinen selbst gehosteten Schriften geändert werden.
```
"siteFonts": [
	{
	"css"; "'Fira Sans' sans-serif"
	"name": "Fira Sans"
	"url": "https://fonts.googleapis.com/ css?family=Fira+Sans: 100,100i,200,200i,300,300i,400,400i,500,500i,600,600i,700,700i,800,800i,900,900i"
	}
]
```
**geändert werden in**
```

"siteFonts": [
	{
	"css"; "'Fira Sans' sans-serif"
	"name": "Fira Sans"
	"url": "webfonts/fontstyles.css?"
	}
]
```
**Man achte auf das Fragezeichen am Ende des relativen CSS Links!!**

### Das Fragezeichen ist wichtig, denn Mobirise hat hier einen Bug (immer noch)
********************************************************************************

Mobirise generiert einen Teil des Codes automatisch und ändert den Link ebenso automatisch beim Uploaden zu **webfonts/fontstyles.css?&display=swap**

**»&display=swap«** wurde automatisch hinzugefügt. **Und ohne unser Fragezeichen funktioniert dieser Funktionsaufruf nicht, und der Link schon gar nicht**.

Jetzt sollte alles funktionieren. Fertig.

#### Öffne nun das Projekt in Mobirise und lade es auf den Webspace. Die HTML Dateien werden nun geändert und sollten keine fonts.googleapis.com-Links mehr enthalten. Zur Kontrolle mal in den Quelltext schauen, vielleicht hast Du was übersehen.

Die Definition der Schriftenlinks bleibt im Project File erhalten, solange man an den Projektschriften (in den Site Styles) **nichts** mehr ändert. Deshalb erst layouten, und dann coden ;-)
