## Format

<img src="https://raw.githubusercontent.com/KlausBlum/OLy-resources/master/images/format-dropdown-03.png">

The *Format* dropdown box on the Config dialogue lets you choose which graphic file format OLy will use to insert musical snippets into your document. 
Each format has its specific advantages and disadvantages. 
Currently, OLy supports the following formats: 

**[PNG](#png)**
* `-`  limited image quality (bitmap format)
* `+` works immediately, no further preparations required
* `-` does not support inserting as separate images (system-by-system)
* `+` usable in both LibreOffice and OpenOffice

**[EPS](#eps)**
* `+` good image quality (vector format)
* `-`  only usable in OpenOffice (not in LibreOffice)
* `+` supports inserting as separate images (system-by-system)
* `-`  images are not displayed on screen and in PDF export (only in printout).

**[SVG](#svg-and-svg--dcrop)**
* `+` good image quality (vector format)
* `-` requires dedicated templates
* `-` requires installing additional fonts
* `-` no automatic cropping (must be done manually)
* `-` does not support inserting as separate images (system-by-system)
* only usable in LibreOffice (not in OpenOffice)

**[SVG [-dcrop]](#svg-and-svg--dcrop)**
* `+` good image quality (vector format)
* `-` requires dedicated templates
* `-` requires installing additional fonts
* `-` does not support inserting as separate images (system-by-system)
* only usable in LibreOffice (not in OpenOffice)


**[PDF to SVG](#pdf-to-svg)** (recommended)
* `+` good image quality (vector format)
* `+` no dedicated templates and no additional fonts required
* `-`  requires third-party conversion software
* `+` supports inserting as separate images (system-by-system)

### **PNG**

This file format is used by default, because it works equally in LibreOffice and OpenOffice, on Windows, Linux or Mac. 
No additional steps have to be taken before using it. 

The **Resolution** for PNG images can be specified. The default value (300) should be usable. Increasing this value will raise image quality, but also the file size.

However, PNG is not the best choice with regard to image quality: [File formats - bitmap vs. vector](https://github.com/openlilylib/LO-ly/wiki/File-formats:-bitmap-vs.-vector#file-formats-bitmap-vs-vector).

Furthermore, PNG does not support the *"insert as separate images (system-by-system)"* option.

### **EPS**
For several reasons, the EPS format is not recommended for daily use:

At the moment, EPS is only usable in OpenOffice. LibreOffice cannot handle it. 

EPS images are printed correctly, but OpenOffice does not display them on screen. It's helpful to use PNG format at first. Once the musical snippets look as indended, they can be recompiled using the EPS format.

EPS images also are missing in PDF export. At least on Windows, there is a workaround for that: Printing the document with a "print to pdf" driver leads to correct results. Those printer drivers create a PDF file instead of printing on paper. 
Windows 10 has a built-in printer driver named _"Microsoft Print to PDF"_ that works well here. 

### **SVG** and **SVG [-dcrop]**
There are two different ways to use the SVG file format. It is only supported by LibreOffice (and not by OpenOffice). 
Using it also requires some important preparations that are described in detail here: 
[SVG requirements](https://github.com/openlilylib/LO-ly/wiki/SVG-requirements#svg-requirements).

In short: 
* probably you have to install some fonts. 
* SVG format requires templates marked [SVG] in their name, SVG [-dcrop] format requires templates marked [SVG -dcrop].

The "SVG [-dcrop]" format might be the recommended option for you if you don't want to install additional conversion software (which would be necessary for using "PDF to SVG", see below.)

### **PDF to SVG**

With this option selected, OLy will call LilyPond to produce a PDF file. 
Unlike with the SVG option, this works with any template, no restrictions apply. 
Also, no additional fonts have to be installed. _Therefore, this is the file format I'd currently recommend to choose._

However, as a second step, this PDF file has to be converted into an SVG file which then will be inserted into your LO document. 
That means, you will have to install an additional conversion software. In OLy's Config dialogue, specify the exact command line to be called. 

**For Linux and Mac**, this is easy: You can install the "pdf2svg" package via your package manager. 

The default settings for the command line to be called should already be visible. 

<img src="https://raw.githubusercontent.com/KlausBlum/OLy-resources/master/images/pdf2svg-linux-01.png">

As command in the first edit field you simply need `pdf2svg`. The checkbox for `"*.svg*"` needs to be activated. Leave the second edit field empty.

The resulting command line will look similar to this (replace "klaus" with your actual user name): 

`pdf2svg "/home/klaus/.cache/ooolilypond/tmp/OOoLilyPond.pdf" "/home/klaus/.cache/ooolilypond/tmp/OOoLilyPond.svg"`

Between the contents of the two edit fields, OLy inserts the filename `OOoLilyPond.pdf` preceeded by the correct path, wrapped in quotation marks - and (if the checkbox is activated) the same with `OOoLilyPond.svg`.

**For Windows**, a suitable third-party tool has to be installed at your own risk. 
At the moment, the only one I know of can be found here: 

http://blog.alivate.com.au/poppler-windows/

Direct download: http://blog.alivate.com.au/wp-content/uploads/2018/10/poppler-0.68.0_x86.7z

(While I don't know if I should recommend a software I barely know, all I can say is, I've been using it for several months now and to me it performs allright. Apparently it consists of the same two libraries as pdf2svg, ported to windows.) 

There is no installation, you just unpack its contents to a location of your choice (for example `C:\Portable\`).

In the first edit field, specify the complete path to the executable file, wrapped in quotation marks (for example `"C:\Portable\poppler-0.68.0\bin\pdftocairo.exe"`. The checkbox should not be activated, the second edit field must be empty.

<img src="https://raw.githubusercontent.com/KlausBlum/OLy-resources/master/images/pdf2svg-windows-01.png">

As above, OLy inserts the PDF file name including path. 
In my case ("Klaus" is my user name) the resulting command line is:

`"C:\Portable\poppler-0.68.0\bin\pdftocairo.exe" -svg "C:\Users\Klaus\AppData\Local\Temp\OOoLilyPond.pdf"`

That's it. 

If you know any other tools for that conversion task, I'd be happy to mention them here. (Inkscape cannot [yet?] be used for this.)

