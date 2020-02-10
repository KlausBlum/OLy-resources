# SVG requirements

Currently the **svg** and **svg [-dcrop]** file formats are only supported by LibreOffice. They cannot be used in OpenOffice.

Using the "normal" templates will not work, e.g. the `Default` template. Instead, use the corresponding template versions:

* for the **svg** format, use the **[SVG]** template versions, e.g. `Default [SVG]`
* for the **svg [-dcrop]** format, use the **[SVG -dcrop]** template versions, e.g. `Default [SVG -dcrop]`

Using these formats also requires some important preparations:

<h2>1.) Providing fonts</h2>

To prevent having LilyPond's default text fonts replaced by different fonts, you need to have installed those three font families (each of them as regular, bold, italic and bold italic version):

* TeXGyreSchola
* TeXGyreHeros
* TeXGyreCursor

Please check if they are already available on your system. Probably they are not, so you will now have to **install them manually**:

If your LilyPond version starts with **2.19**, you can already find necessary files on your computer. If you have LilyPond **2.18.2** or older, you can download those files from the web. 

* **LilyPond 2.19:** look inside LilyPond's `otf` folder.

  * **on Linux systems:**
go to the folder `/usr/share/lilypond/2.19.83/fonts/otf/` (whereas `2.19.83` might have to be replaced by your actual version number),
  * **on Windows systems:**
inside your LilyPond program folder (typically `C:\Program Files (x86)\LilyPond` or `C:\Program Files\LilyPond`), go to the subfolder `\usr\share\lilypond\current\fonts\otf`.

* **LilyPond 2.18.2 and older:** You can download those font families from the web, e.g.
  * <a href="https://www.fontsquirrel.com/fonts/tex-gyre-schola">www.fontsquirrel.com/fonts/tex-gyre-schola</a>
  * <a href="https://www.fontsquirrel.com/fonts/tex-gyre-heros">www.fontsquirrel.com/fonts/tex-gyre-heros</a>
  * <a href="https://www.fontsquirrel.com/fonts/tex-gyre-cursor">www.fontsquirrel.com/fonts/tex-gyre-cursor</a> 

In either case, you now have twelve files matching the pattern `texgyre*.otf`. Those files now **have to be *installed*** by you. Usually, right-clicking on a `*.otf` file should bring up a context menu that contains an appropriate command.

After that font installation procedure, **you are now ready** to use the **svg [-dcrop]** file format. 

The **svg** file format is mainly interesting for backwards compatibility (from OLy 0.5.0 to 0.5.10, it was the only vector format usable in LibreOffice). If you want to work with existing documents that use it, you should continue reading this page. If there is no such reason for you to use it, you can stop reading here and quit this page.  :-)

## 2.) Adapting templates

If you have updated your templates during the installation of OLy 1.0.0 or later (with the "Restore templates" button) or this OLy version was the first you have ever installed, you can skip this chapter and continue with "[3.) Manual cropping](#3-manual-cropping)". 
If your templates come from an OLy version starting with 0.5 and you did not yet prepare the templates with an `[SVG]` in their name, here is some more work for you.

We now have to explicitely <a href="http://lilypond.org/doc/v2.19/Documentation/notation/fonts.html#entire-document-fonts">specify</a> the font families. Therefore we need to go to the template folder (its location can be viewed in the OLy <em>Config</em> dialog) and manually edit the template files that have <em>[SVG]</em> in their name.
Inside the <code>paper{...}</code> section, there is a paragraph that has been prepared for that purpose:

<pre class="lilypond"><span class="lilypond-comment comment">% If LilyPond's default fonts are not installed and therefore "invisible" to other applications,
% you can define a replacement font here:

<span style="background-color: #ffd2aa;">%{</span><span style="background-color: #fff5aa;">
#(define fonts
   (make-pango-font-tree
    "TeXGyreSchola"           ; adjust this font name according to your needs
    "TeXGyreHeros"            ; adjust this font name according to your needs
    "TeXGyreCursor"           ; adjust this font name according to your needs
    (/ staff-height pt 20)))
</span><span style="background-color: #ffd2aa;">%}</span></span></pre>

The command itself is enclosed in <em>block comment</em> signs <code>%{</code> and <code>%}</code>. Putting a simple blank space between <code>%</code> and <code>{</code> will turn the first into an ordinary comment, and the code between them will become visible for LilyPond:

<pre class="lilypond"><b>% {</b>
<span class="scheme-scheme">#</span><span class="scheme-delimiter">(</span><span class="scheme-keyword keyword">define</span> <span class="scheme-scheme">fonts
   </span><span class="scheme-delimiter">(</span><span class="scheme-function function">make-pango-font-tree
    </span><span class="scheme-string string">"TeXGyreSchola"           </span><span class="scheme-comment comment">; adjust this font name according to your needs
    </span><span class="scheme-string string">"TeXGyreHeros"            </span><span class="scheme-comment comment">; adjust this font name according to your needs
    </span><span class="scheme-string string">"TeXGyreCursor"           </span><span class="scheme-comment comment">; adjust this font name according to your needs
    </span><span class="scheme-delimiter">(</span><span class="scheme-keyword keyword">/</span> <span class="scheme-scheme">staff-height pt</span> <span class="scheme-number value">20</span><span class="scheme-delimiter">)))
</span><b>%}</b></pre>

Now save the template and repeat the procedure with the other <em>[SVG]</em> templates.

If your templates date from OLy 0.5.0 to 0.5.3, the paragraph will rather look like this:

<pre class="lilypond"><span class="lilypond-comment comment">% If LilyPond's default fonts are not installed and therefore "invisible" to other applications,
% you can define a replacement font here:

% {
% for LilyPond 2.19.11 and older, it only works like this:
</span><span class="scheme-scheme">#</span><span class="scheme-delimiter">(</span><span class="scheme-keyword keyword">define</span> <span class="scheme-scheme">fonts
   </span><span class="scheme-delimiter">(</span><span class="scheme-function function">make-pango-font-tree
    </span><span class="scheme-string string">"Century Schoolbook"  </span><span class="scheme-comment comment">; adjust this font name according to your system
    </span><span class="scheme-string string">"sans-serif"
    "monospace"
    </span><span class="scheme-delimiter">(</span><span class="scheme-keyword keyword">/</span> <span class="scheme-scheme">staff-height pt</span> <span class="scheme-number value">20</span><span class="scheme-delimiter">)))
</span><span class="lilypond-comment comment">%}</span></pre>

In that case, just replace it by the code given above.

Phew! Now you should be able to use that SVG feature without further issues.

<h2>3.) Manual cropping</h2>

Many templates in OLy make use of <code>include "lilypond-book-preamble.ly"</code>. This will <em>not</em> work with the <code>.svg</code> backend. Therefore, for every template there is a "[SVG]" version that works without <code>lilypond-book-preamble</code>. That means, however, there's no automatic cropping.
Those templates use the <em>Line Width</em> field for "paper" width, and a "paper" height must be specified as well:

<a href="http://lilypondblog.org/wp-content/uploads/2017/04/OLy02-Editor-Window-01.gif"><img src="http://lilypondblog.org/wp-content/uploads/2017/04/OLy02-Editor-Window-01-300x232.gif" alt="OLy02-Editor-Window-01" class="aligncenter size-medium wp-image-4745" width="300" height="232"></a>

After compiling, the resulting image will have <em>exactly</em> the intended dimensions:

<a href="http://lilypondblog.org/wp-content/uploads/2017/04/OLy02-Uncropped-Snippet-01.gif"><img src="http://lilypondblog.org/wp-content/uploads/2017/04/OLy02-Uncropped-Snippet-01-300x141.gif" alt="OLy02-Uncropped-Snippet-01" class="aligncenter size-medium wp-image-4747" width="300" height="141"></a>

To get rid of the unnecessary white margins, one could adjust the width and height values by trial-and-error and recompile several times. But I think it's much easier to manually crop the image by selecting it and hitting the appropriate button (or right-clicking it and choosing the "Crop" command from the context menu):

<a href="http://lilypondblog.org/wp-content/uploads/2017/04/OLy02-Cropping-01b.gif"><img src="http://lilypondblog.org/wp-content/uploads/2017/04/OLy02-Cropping-01b-300x157.gif" alt="OLy02-Cropping-01b" class="aligncenter size-medium wp-image-4755" width="300" height="157"></a>

The green dots turn into red cropping marks that can be dragged to a new position:

<a href="http://lilypondblog.org/wp-content/uploads/2017/04/OLy02-Cropping-02.gif"><img src="http://lilypondblog.org/wp-content/uploads/2017/04/OLy02-Cropping-02-300x133.gif" alt="OLy02-Cropping-02" class="aligncenter size-medium wp-image-4750" width="300" height="133"></a>

When editing an existing OLy object, you won't always have to repeat the cropping procedure. In the main OLy window, you can choose to keep the object's actual size and crop settings:

<a href="http://lilypondblog.org/wp-content/uploads/2017/04/OLy02-Keep-Crop-01.gif"><img src="http://lilypondblog.org/wp-content/uploads/2017/04/OLy02-Keep-Crop-01-300x120.gif" alt="OLy02-Keep-Crop-01" class="aligncenter size-medium wp-image-4752" width="300" height="120"></a>

In the <em>Config</em> dialog, you can specify if you want this option to be turned on or off by default. There are independent settings for Writer and Impress/Draw.

Okay, now you are ready to go. If you want to get a little more background knowledge about why you had to install fonts and (eventually) adapt templates, you can continue reading... but now there are no more steps to take. Instead, you can stop reading here and start typesetting your music.  :-)

## Appendix: What's that font problem?

If your musical snippet contains some text markup, probably it would happen to you that a <code>.svg</code> image will not be displayed with the original font. Instead, some replacement font would appear:

<a href="http://lilypondblog.org/wp-content/uploads/2017/04/OLy02-Fonts-01.gif"><img src="http://lilypondblog.org/wp-content/uploads/2017/04/OLy02-Fonts-01-300x191.gif" alt="OLy02-Fonts-01" class="aligncenter size-medium wp-image-4762" width="300" height="191"></a>

For the above picture, a simple <code>{c1^"What does this look like?"}</code> has been first rendered as <code>.png</code> image, second as <code>.svg</code>.

Now what has happened here?

In most cases, LilyPond is used to create <code>.pdf</code> files. Any font that is used will be <em>embedded</em> in the <code>.pdf</code> document. That means, it is displayed correctly, no matter if the font is installed on the target system or not.
This is <em>not</em> the case when creating <code>.svg</code> files: Having them displayed as intended requires to have the fonts <em>installed</em> on the system.

The thing is, even on <em>your</em> computer, LilyPond can use its own fonts without them being installed, so probably they are not. You will find those font files in their respective folder
<strong>on Linux systems:</strong>
<code>/usr/share/lilypond/<em>2.18.2</em>/fonts/otf/</code> (whereas <code><em>2.18.2</em></code> might have to be replaced by your actual version number),
<strong>on Windows systems:</strong>
inside your LilyPond program folder (typically <code>C:\Program Files (x86)\LilyPond</code> or <code>C:\Program Files\LilyPond</code>) in the subfolder <code>\usr\share\lilypond\current\fonts\otf</code>.

Which font files are we looking for? That depends on your LilyPond version.

<strong>Version 2.18.2 and earlier</strong>

According to the <a href="http://lilypond.org/doc/v2.18/Documentation/notation/fonts.html">2.18 Notation Reference</a>, the <em>roman</em> (serif) font defaults to "New Century Schoolbook".
Well, to be exact, it's the four files <code>CenturySchL-Bold.otf</code>, <code>CenturySchL-BoldItal.otf</code>, <code>CenturySchL-Ital.otf</code> and <code>CenturySchL-Roma.otf</code> which will show up as "Century Schoolbook L" font family.

Installing those four font files on your system might do the trick. On <strong>Linux</strong> (Ubuntu Studio 16.04) it worked well for me, but on <strong>Windows</strong> 7 only the bold and italic version were available to the system. If that applies to you as well, just continue reading. We're not yet at the end...

In addition, the <a href="http://lilypond.org/doc/v2.18/Documentation/notation/fonts.html">2.18 Notation Reference</a> tells that there are no default fonts for <em>sans</em> and <em>typewriter</em>, so different computers will produce different output. If you want to take precautions against that, also continue reading.

<strong>Version 2.19</strong>

In the <a href="http://lilypond.org/doc/v2.19/Documentation/notation/fonts.html">2.19 Notation Reference</a> you can see that LilyPond now provides fonts for all three font families: <em>roman</em>, <em>sans</em> and <em>typewriter</em> now default to "TeXGyreSchola", "TeXGyreHeros" and "TeXGyreCursor".
The first thing to do is installing them. Therefore, in your <code>otf</code> folder shown above, locate twelve files matching <code>texgyre*.otf</code> and install them on your computer. They seem to work without problems, even on Windows. ;-)

Users of Version <strong>2.18</strong> can download those font families from the web, e.g.:
* <a href="https://www.fontsquirrel.com/fonts/tex-gyre-schola">www.fontsquirrel.com/fonts/tex-gyre-schola</a>
* <a href="https://www.fontsquirrel.com/fonts/tex-gyre-heros">www.fontsquirrel.com/fonts/tex-gyre-heros</a>
* <a href="https://www.fontsquirrel.com/fonts/tex-gyre-cursor">www.fontsquirrel.com/fonts/tex-gyre-cursor</a>

Unfortunately, the developers decided to make another change: <code>.svg</code> graphic files created by LilyPond 2.19 no more contain font names by default. Instead, they use aliases like "serif", "sans-serif" and "monospace". While there might be <a href="http://lilypond.1069038.n5.nabble.com/markup-sans-does-not-always-give-sans-serif-output-in-svgs-in-2-19-36-td187143.html">some reasons for that decision</a>, we are therefore required to make the adjustments described in *chapter 2: Adapting templates.* 

To simplify this whole mess, as of version 1.0.0 OLy will assume that you have installed the necessary fonts as described in *chapter 1: Providing fonts*. All `[SVG]` and `[SVG -dcrop]` templates already contain the required declarations. 
 
