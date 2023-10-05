# Mit Mobirise Google Fonts DSGVO-gerecht nutzen
********************************************************

*NB: Backups sind wichtig und man sollte immer eines haben. Was ich hier beschreibe, habe ich ausprobiert und es funktioniert. Trotzdem machst Du Deine Änderungen auf eigene Gefahr! Es gibt von meiner Seite keinen Support zu diesem Workaround, mehr als diese Seite gibt es nicht.*
   
*Mobirise Version 5.9.4 / Stand: 29. September 2023 *

***************************************************
### Mobirise App mit Funktion für lokale Fonts (aber ziemlich schlecht)

Mobirise (ein nicht perfekter, aber praktischer und preiswerter Webbaukasten) bietet eine Funktion zur Verwendung lokaler Fonts. Man muss sie in ein Projekt importieren, und beim Publizieren werden die Fonts zusammen mit den generierten HTML Files per FTP in dein Web-Root geladen. Das Problem dabei: Schriftfamilien kann man so nicht nutzen, nur einzelne Schnitte. was zielmich unbequem für eine Layousphase ist.

**Zudem werden die Fontdateien beim Upload als TTF Truetype Fonts ~~umgewandelt~~ auf den Server geladen. Dadurch wächst die Detenmenge der Fonts (die von webroot zu laden sind) ganz erheblich. Sehr schnell ~~entstehen~~ sind dabei TTF Fontschnitte, die 500K haben. Zum Vergleich: ein WOFF2 Webfont des gleichen Schirtschnittes hat gerademal 40K, weniger als ein Zehntel der Datenmenge.**

> Also muss der Wunsch sein, a) Google Fonts zu verwenden, b) beim Layouten in Mobirise keine Einschränkung zu haben, c) WOFF2 Webfonts auf dem Webspace zu benutzen und d) dabei DSGVO compliant zu sein. 

***************************************
### Layoutphase immer mit Google Font-Linking

Für Layoutzwecke ist es immer besser, *ZUNÄCHST EINMAL* Google Fonts per Hyperlink zu Google einzubetten, da man flexibel bleibt und immer ohne großen Auswand eine Schrift (mit allen Schnitten) sehr schnell und einfach einbinden und nutzen kann.
In einer Layoutphase spielt das eine große Rolle, die Entwicklung von Feinheiten bei der Typopgrafie bleibt einfach und intuitiv.

Nach Abschluss des Layouts, der Abnahme und der Fertigstellung aller Templates und Seitenlayouts *MÜSSEN* jedoch in jedem Fall die verlinkten Schriften durch selbst-gehostete Google Font Schriften ersetzt werden. Denn sonst ist die Website NICHT DSGVO konform und damit abmahnfähig. Ich beschreibe hier, wie das klappt

> Mobirise bietet einen direkten Zugang zu Google Fonts. Man lädt die gewünschte **Schrift-Familie** herunter, und kann problemlos und direkt damit arbeiten. **Schnitte wie fett oder kursiv stehen dann innerhalb der Schriftfamilie zur Verfügung**. Die Schriften werden automatisch herunter geladen und im Projektordner gesichert, **damit man lokal auf dem Rechner WYSIWYG arbeiten kann**. 

Lädt man ein solches Projekt jedoch einfach ohne Anpassung auf den Webserver, verlässt man die rechtssichere Zone. Denn die Schriften werden über einen Link direkt von Google geladen, und nicht wie vorgeschrieben, vom eigenen Server

***************************************
### Website mit Google Fonts, aber OHNE Google Font-Linking

- Die mit Mobirise geladenen verwendeten Google Schriften bleiben im Mobirise Projektordner auf dem Arbeitsrechner.

- Vor dem FTP-Export des Mobirise Projetes auf den Webserver müssen die verwendeten Googleschriften in WOFF2 Webfonts konvertiert verwenden

- Diese Webfonts und das CSS-File müssen dann händisch ins Root des Test- oder Webservers kopiert werden.

- Die Projektdatei vom Mobirise auf deinem Rechner muss angepasst werden.

***************************************
### Nutze WOFF2 Google Webfonts – Schritt für Schritt

- In der Layoutphase **auf jeden Fall mit den in Mobirise verfügbaren Google Schriften arbeiten.**
- Nach Abschluss der Layoutphase werden **die verwendeten Google Schriftfamilien bei [Google Webfont Helper](https://gwfh.mranftl.com/fonts) samt CSS-Code als WOFF2 Webfonts generiert
- **Die WOFF2 Webfonts und das dazugehörige CSS werden zusammen in einen Ordner namens »webfonts« im Root des Webservers gesichert**.
- WOFF2 als Schriftformat genügt völlig.
- Das CSS-file wird im Google Webfont Helper automatisch erzeugt (in einer Box unter der Schriftauswahl kann man den Serverpfad ergänzen/ändern/korrekt eingeben). Den »kopieren«-Button klicken und den CSS Code in einer Datei sichern , z.B. als **fontstyles.css**
– Die fontstyles.css Datei zusammen mit den Schriften in einen Ordner namens »webfonts« ablegen, der bei Mobirise im root des Webservers liegen muss.
- **Im  Mobirise-Project File muss nun der Google Link durch einen lokalen Link ersetzt werden. Siehe nächster Punkt …**

***************************************
## Mobirise Project patchen

- Gehe in den Mobirise-Projektordner und öffne das betreffende **»project.mobirise«** mit einem Text-Editor
	- beim Mac: ~/Library/Application Support/Mobirise.com/Mobirise/projects/project-2023-09-28_XXXXXXXX/project.mobirise
- Kann man gut identifizieren anhand der Screenshots im Projektordner
- Kann man eindeutig identifizieren anhand der ersten Zeilen im Projectfile: Zeile 3 ("name": "DEIN_PROPJEKTNAME")
- Suche nach **»sitefonts«** (etwa Zeile 44)

*Innerhalb des Mobirise-Project File* (Code ähnlich wie hier, **mein Schriftbeispiel ist Fira**. Schriftnamen und Pfade können bei Dir anders sein) muss nach Abschluss der Layoutarbeiten die **"url"** von einem absoluten Link zu Google in einen relativen Link zu deinen selbst gehosteten Schriften geändert werden.
```css
"siteFonts": [
	{
	"css"; "'Fira Sans' sans-serif"
	"name": "Fira Sans"
	"url": "https://fonts.googleapis.com/ css?family=Fira+Sans: 300,300i,500,500i,700i"
	}
]
```
**geändert werden in**
```css

"siteFonts": [
	{
	"css"; "'Fira Sans' sans-serif"
	"name": "Fira Sans"
	"url": "webfonts/fontstyles.css?"
	}
]
```
**Man achte auf das Fragezeichen am Ende des relativen CSS Links!!**

***************************************
### Das Fragezeichen ist wichtig, denn Mobirise hat hier einen Bug (immer noch)

Mobirise generiert einen Teil des Codes automatisch und ändert den Link ebenso automatisch beim Uploaden zu **webfonts/fontstyles.css?&display=swap**

**»&display=swap«** wurde automatisch hinzugefügt. **Und ohne unser Fragezeichen funktioniert dieser Funktionsaufruf nicht, und der Link schon gar nicht**.

Mit dem ? sollte jetzt alles funktionieren. Fertig.

> Öffne nun das Projekt in Mobirise und lade es auf den Webspace. Die HTML Dateien werden nun geändert und sollten keine fonts.googleapis.com-Links mehr enthalten. Zur Kontrolle mal in den Quelltext schauen, vielleicht hast Du was übersehen.

***************************************
###  Die großen Vorteile dieser Methode

- Die Definition der Schriftenlinks bleibt im Project File erhalten, solange man an den Projektschriften (in den Site Styles) **nichts** mehr ändert.
- Auf dem Arbeitsrechner kann mit den festgelegten Google Schriften frei gearbeitet werden (fett, kursiv, H1 ... usw.). Die Previews in Mobirise funktionieren einwandfrei.
- Bei Upload wird kein fonts.googleapis-Link mehr erzeugt.
- Es werden ausschliesslich WOFF2 Fonts verwendet, was viel schneller lädt und besser funktioniert.
- Der Export auf den Webserver ist DSGVO konform.

> - Ein Hinweis noch. Entscheidet man sich später doch noch für eine Änderung oder Erweiterung der Schriftauswahl, muss man die Prozedur wiederholen. Deshalb erst layouten, und dann coden ;-)
 > - Und wenn man einen Export auf dem Arbeitsrechner zur Kontrolle exportieren möchte, so muss man den »webfonts« Ordner natürlich samt Inhalt auch in dieses Verzeichnis kopieren, damit die Darstellung hier mit den richtigen Schriften erfolgt.
