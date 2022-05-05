# Using Google Fonts in compliance with the GDPR - special case Mobirise
********************************************************

## Example Mobirise (and CMS in general)
********************************************************

*A little preface as always: backups are important and you should always have one. And without a backup you should not mess around with source code or project files. What we describe here, we have tried and it works. However, you make your changes at your own risk! There is no support for this workaround, there is nothing more than this page. If someone wants to refer to it, then put a link to this page.
   
*Mobirise version 5.6.8 / as of May 5, 2022 / deepl-translation*

---

### Layout phase always with Google Font linking

For layout purposes, it is always better to embed *FINALLY ONCE* Google Fonts via hyperlink to Google, because you stay flexible and can always include and use a font (with all weights) very quickly and easily without much effort.
In a layout phase this plays a big role, the development of subtleties in typography remains simple and intuitive.

However, after the layout is complete, approved and all templates and page layouts are finalized, the linked fonts *MUST* be replaced with self-hosted Google Font fonts in any case. Because otherwise the website is NOT DSGVO compliant and therefore liable to warning. 

> Mobirise offers direct access to Google Fonts and downloads the desired font family, you can easily and directly work with it. Cuts like bold or italic are then available within the font family. The fonts are automatically downloaded and **saved** in the project folder, so you can work WYSIWYG locally on your computer. However, if one loads such a project on the web server, one leaves the legally safe zone.

---

### Live always without Google Font linking

In the case of Mobirise, these fonts can remain in the project folder on the work computer, **but the Google fonts linked in the source code must be replaced by links that refer to the fonts stored on the web server.** These fonts (Google Fonts) can be downloaded together with the appropriate CSS code using the [Google Webfont Helper](https://google-webfonts-helper.herokuapp.com/fonts).

> Mobirise does offer the option to use your own fonts, which are then loaded onto the webspace with the other data. This is DSGVO compliant. BUTTT the big disadvantage of this is that only single weights (such as regular, bold or italic) can ever be added, not font families. The possible format assignments in the editor of Mobirise are thus practically worthless, the automatically generated font names confusing etc, no comparison to the normal ease of use. Layouting and editing is anything but comfortable.

### So here's what to do:

- In the layout phase, definitely work with linked, **external** Google Fonts on the test server, whether CMS template or Mobirise.
- After finishing the layout phase, **download the used font families at [Google Webfont Helper](https://google-webfonts-helper.herokuapp.com/fonts) together with CSS code and save them on the web server (preferably fonts and CSS together in a folder named "fonts", as described here in the example)**, no matter if CMS template or Mobirise.
- WOFF and WOFF2 as font formats are completely sufficient.
- The CSS-file is created automatically in the Google Webfont Helper (in a box below the font selection you can add/change/correct the server path). Click the "copy" button and save the CSS code in a file, e.g. as **fontstyles.css**.
- Put the fontstyles.css file together with the fonts into a folder called "fonts", which is then 
	- at Mobirise in the root of the webserver
	- at the CMS it makes sense to put it in the template folder of the webserver.
- In the CMS template **the links to the Google font server must now be removed and replaced with a link to the self-hosted CSS font file**.
- **In the Mobirise project file, the Google link must now be replaced with a local link**.

---
### Update CMS template
********************************************************
Insert the link to the fonts stylesheet into the PHP template.
This font folder in the example code is located directly in the template directory (but you can also choose this freely, depending on the template, and must adjust the code accordingly, here example WBCE, mention only for completeness) ...

    <link rel="stylesheet" type="text/css" href="<?php echo TEMPLATE_DIR; ?>/fonts/fontstyles.css" />


"fontstyles.css" is in the "fonts" folder, and it is in the template folder - so the path is /fonts/fontstyles.css

> Very important: These old https://fonts.googleapis.com/css?blablabla links must be completely removed from the template or stylesheet!

---
## Mobirise Project update
********************************************************

This works completely different.

- Go to the Mobirise project folder and open the corresponding **"project.mobirise "** with a text editor
	- on Mac somewhat like ~/Library/Application Support/Mobirise.com/Mobirise/projects/project-2022-05-05_123456/project.mobirise
- Can be easily identified by the screenshot in the project folder
- Can be clearly identified from the first lines in the project file
- Search for **"fontname "**

There inside the Mobirise-Project File (code similar to here, font names and paths may be different) after finishing the layout work the **"link"**
```
"fontName": {
"name": "Libre Franklin",
"css": "'Libre Franklin', serif",
"link": "https://fonts.googleapis.com/css?family=Libre+Franklin:400,400i,600,600i"
            }
```
**changed to
```
"fontName": {
"}, ``name'': ``Libre Franklin'',
"}, "css": ``Libre Franklin'', serif,
"link": "https://DEINEDOMAIN.COM/fonts/fontstyles.css"
            }
```
Code similar to here, font names and paths may be different. Replace DEINEDOMAIN.COM with your own domain.
A relative link should also work. **But we are not done yet!**

### And now something very important concerning Mobirise
********************************************************

Mobirise generates a part of the code automatically and also changes the link automatically when uploading to **https://YOURDOMAIN.COM/fonts/fontstyles.css&display=swap**.

**"&display=swap "** was added automatically. No, you can't turn that off. Which is why it's not the link that needs to be adjusted, **but for Mobirise, the suffix of the CSS filename**.

It **MUST** be
- the filename of the CSS from **fontstyles.css**
- to **fontstyles.css&display=swap**.

Now everything should work. Done.

#### Now open the project in Mobirise and upload it to your webspace. The HTML files are now changed and should not contain fonts.googleapis.com links anymore. Check the source code, maybe you missed something.

The definition of the font links remains in the project file as long as you **don't** change anything in the project fonts (in the site styles). Therefore first layout, and then code ;-)
