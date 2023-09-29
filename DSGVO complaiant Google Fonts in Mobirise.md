# Use Google Fonts in compliance with DSGVO with Mobirise
********************************************************

*NB: Backups are important and you should always have one. What I describe here, I tried and it works. Nevertheless, you make your changes at your own risk! There is no support from my side for this workaround, there is nothing more than this page.

*Mobirise Version 5.9.4 / As of: September 29, 2023 *

****************************************************
### Mobirise app with local fonts feature (but pretty bad).

Mobirise (a not perfect, but practical and cheap web builder) offers a feature to use local fonts. You have to import them into a project, and when publishing, the fonts are uploaded to your web root via FTP along with the generated HTML files. The problem with this is that you can't use font families this way, only individual cuts. which is purposefully inconvenient for a layous phase.

**In addition, the font files are converted to TTF Truetype fonts during upload. Thus the amount of fonts (to be loaded by webroot) grows considerably. Very fast TTF fonts are created, which have 500K. For comparison: a WOF2 webfont of the same style has just 40K, less than a tenth of the amount of data.

> So the desire must be a) to use Google Fonts, b) to have no restriction in layouting in Mobirise, c) to use WOF2 webfonts on the webspace and d) to be DSGVO compliant while doing so. 

****************************************************
### Layout always with Google Font linking

For layout purposes, it is always better to *FURTHER ON* embed Google Fonts via hyperlink to Google, as you remain flexible and can always include and use a font (with all weights) very quickly and easily without much effort.
In a layout phase this plays a big role, the development of subtleties in typography remains simple and intuitive.

However, after the layout is complete, approved and all templates and page layouts are finalized, the linked fonts *MUST* be replaced with self-hosted Google Font fonts in any case. Because otherwise the website is NOT DSGVO compliant and therefore liable to warning. I describe here how to do that

> Mobirise offers direct access to Google Fonts. You download the desired **font family**, and can easily and directly work with it. **Cuts like bold or italic are then available within the font family**. The fonts are automatically downloaded and saved in the project folder, **so that you can work WYSIWYG locally on your computer**. 

However, if you simply load such a project onto the web server without customization, you leave the legally safe zone.

****************************************************
### Website with Google Fonts, but WITHOUT Google Font linking.

- The used Google fonts loaded with Mobirise remain in the Mobirise project folder on the work computer.

- Before the FTP export of the Mobirise project to the web server, the used Google fonts must be converted to WOF2 web fonts.

- These web fonts and the CSS file must then be copied manually to the root of the test or web server.

- The project file from the Mobirise on your computer must be adapted.

****************************************************
### Use the WOF2 google webfonts - step by step.

- In the layout phase **be sure to work with the Google fonts available in Mobirise. And only those.**
- After finishing the layout phase **the used Google font families are generated at [Google Webfont Helper](https://gwfh.mranftl.com/fonts) together with CSS code as WOF2 web fonts
- **The WOF2 web fonts and the corresponding CSS are saved together in a folder called "webfonts" in the root of the web server**.
- WOFF2 as font format is completely sufficient.
- The CSS file is automatically generated in Google Webfont Helper (in a box below the font selection you can add/change/correct the server path). Click the "copy" button and save the CSS code in a file, e.g. as **fontstyles.css**.
- Put the fontstyles.css file together with the fonts in a folder called "webfonts", which must be in the root of the web server at Mobirise.
- **In the Mobirise project file, the Google link must now be replaced with a local link. See next point ...**

****************************************************
## Patch Mobirise Project

- Go to the Mobirise project folder and open the relevant **"project.mobirise "** with a text editor
	- on Mac: ~/Library/Application Support/Mobirise.com/Mobirise/projects/project-2023-09-28_XXXXXXXX/project.mobirise
- Can be easily identified from the screenshots in the project folder
- Can be clearly identified from the first lines in the project file: Line 3 ("name": "YOUR_PROPJECTNAME")
- Search for **"sitefonts "** (about line 44)

*Inside the Mobirise-Project File* (code similar to here, **my font example is Fira**. Font names and paths may be different for you) needs to be changed from an absolute link to Google to a relative link to your self-hosted fonts after the layout work is complete.

```
"siteFonts": [
	{
	"css"; "'Fira Sans' sans-serif"
	"name": "Fira Sans"
	"url": "https://fonts.googleapis.com/ css?family=Fira+Sans: 300,300i,500,500i,700i"
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
**Look out for the question mark at the end of the relative CSS link!!!**

****************************************************
### The question mark is important, because Mobirise has a bug here (still)

Mobirise automatically generates part of the code and also automatically changes the link when uploading to **webfonts/fontstyles.css?&display=swap**.

**"&display=swap "** was added automatically. **And without our question mark, this function call doesn't work, and the link certainly doesn't**.

With the ? everything should work now. Done.

> Now open the project in Mobirise and upload it to your webspace. The HTML files will be changed now and should not contain fonts.googleapis.com links anymore. Check the source code, maybe you missed something.

****************************************************
### The great advantages of this method

- The definition of the font links remains in the project file as long as you **don't** change anything else in the project fonts (in the site styles).
- On the workstation you can work freely with the defined Google Fonts (bold, italic, H1 ... etc.). The previews in Mobirise work fine.
- When uploading, no more fonts.googleapis link is generated.
- Only WOF2 fonts are used, which loads much faster and works better.
- The export to the webserver is DSGVO compliant.

> - One more hint. If you decide to change or extend the font selection later, you have to repeat the procedure. Therefore first layout, and then code ;-)
 > - And if you want to export an export to your workstation for control purposes, you have to copy the "webfonts" folder including its content into this directory, so that the display here is done with the correct fonts.

Translated with www.DeepL.com/Translator (free version)
