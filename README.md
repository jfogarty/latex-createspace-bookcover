Latex CreateSpace BookCover
===========================

This is a template for creating a quality book cover PDF suitable for
use by CreateSpace and Ingram Spark print-on-demand paperback publishers.
The default setup is for a 6" x 9" paperback, but the parameters are
are easily changed.

[cover]: images/CoverExample.jpg "The Example Createspace Paperback Cover"

![This would be a Paperback Cover Image, but I can't find the file, Sorry.][cover]

Note that if you are writing a non-fiction book, you might consider doing so
in LaTeX. [My Non-Fiction Book/eBook template](https://github.com/jfogarty/latex-nonfiction-ebook-template) on [Github](https://github.com/) is a bit more complex
to use, but will do all the heavy lifting of generating page runners, tables
of content (both summary and detailed), bibliography, multi-leveled index,
and lists of terms and abbreviations. It will regenerate your book for
print-on-demand (POD) printers, as ePub files, as a distributable PDF
file, as an HTML web site, and as a raw ASCII file (an annoying requirement
for obtaining catalog in publication data).

## Structure

`BookParameters.tex` is a LaTeX file containing defines about your book. 
You edit this while constructing your book. In my projects, all LaTeX
root files for the same publication include this file.

The spine width is determined by the page count and the width of the
paper stock. The CreateSpace white stock is set by default. 

You will need to provide your own image files. I've included some example
files that are CC licensed from Wikipedia as an example.

There are four files you can expect to edit:

- **BookParameters.tex**
- **cover/BackCover.tex**
- **cover/FrontCover.tex**
- **cover/SpineCover.tex**

You will need to adjust image sizes and spacing for your own text and 
image shapes by editing the .tex file. Reasonable familiarity with LaTeX
is required.

### Building Your Cover PDF File ###

Run the provided script:

```bash
    ./bin/makecover
```

This should produce output that looks like:
```
$ makecover
    - Book Format 6in x 9in (315 pages)
    - ISBN13: 978-4-4444444-4-6, Price: $9.95 [50995]
    - GhostScript generation of ISBN Barcode image
    - Pass 1 - Generating Front Cover
    - Pass 1 - Generating Back Cover
    - Pass 1 - Generating Spine for Cover
    - Pass 2 - Generating Front Cover
    - Pass 2 - Generating Back Cover
    - Pass 2 - Generating Spine for Cover
    - Assembling Final Cover
    - Removing intermediate files
    - Done.
```
If any step fails (usually due to missing or incorrectly configured tools)
you should examine the log files generated into the `cover/logs` directory.

## Page Count and Spine

The **TotalPageCount** parameter in `BookParameters.tex` controls the
thickness of your book's spine. The page count times the **SinglePageThicknessPt**
determines the thickness. This is currently set to the thickness of standard
white paper used by CreateSpace, but you may need to change it to match the bond
used by your print on demand publisher.

Once you have built a PDF for your book, you can set the page count by:

- [1] editing `BookParameters.tex` directly **or**
- [2] running `./bin/setBookParameter TotalPageCount nnn` **or**
- [3] running `setBookTotalPageCount [logfile]` against
the final pdflatex log file created when you built your pdf.
This extracts the page count from the log and modifies `BookParameters.tex`.
You will need the `./bin` directory in your path.

`SpineCover.tex` automatically adjusts the format of the spine based on
the page count, but you will need to edit it if your book title, subtitle,
author name, or publisher names are significantly longer than those
provided in the example.

## ISBN Numbers

You should have ISBN numbers for each verson of your book. 
In the United States these must be either purchased directly from
[Bowker](http://www.bowker.com/) at [MyIdentifiers.com](https://www.myidentifiers.com/get-your-isbn-now). I recommend buying a block of 10 ISBNs for $250. Alternatively, you can
get a 'free' ISBN from [CreateSpace](https://www.createspace.com) if you are
publishing your print version only with them.
There are issues with doing this, that may make it difficult
to use other printers, but you may decide to save the bucks (not recommended).

Once you have your ISBNs, modify the `BookParameters.tex` fields, **PrintISBN**,
**PrintISBNShort**, **EbookISBN**, and **EbookISBNShort**. 13 digit ISBN numbers
can be converted to 10 digit and vice versa using [this tool](http://pcn.loc.gov/isbncnvt.html). Make sure you select **Hyphenate ISBNs**. You must have the
dashes in the correct places in your barcodes.
 
## Price and ISBN Barcode

The **PrintPrice** parameter is the price that will be added to the barcode 
placed in the lower right corner of the back cover. Undefine (put a % in
front of) the parameter to generate a barcode without a price. Note that
most retailers will not accept books without the price extension.

This barcode is generated with ghostscript using Terry Burton's most excellent
[PostScript based barcode generator](https://github.com/bwipp/postscriptbarcode).
Use $99.99 if your price exceeds $100 (good luck selling that puppy).
 
## eBook Cover Art

A special .tex file `FrontCoverEbook.tex` is included to help you create the
large format art required for eBook covers. Apple iBooks and Amazon Kindle now
require images with 1400+ pixels on short side. Ingram Spark requires a minimum
of 1600 pixels on shortest side and 2560 pixels on the longer side.

This project typesets a 12" x 18" PDF file. You can then use [Gimp](https://www.gimp.org/), [Pinta](https://pinta-project.com/pintaproject/pinta/), or
other image capture programs to grab a suitable image for upload. I often just
use the [Okular PDF Viewer](https://okular.kde.org/) on Linux to select and
save cover images.


## License

This project is MIT Licensed and under the Free Public License 1.0.0. You are
free to choose the one that works for you. if you want to grab parts for use
in your own project then please remove my name and hack away.

## Installing 

Just copy the contents to your directories and use as is. You will need a
complete LaTeX environment, which includes `pdflatex`. 

I **strongly** recommend installing [TeX Live 2016](https://www.tug.org/texlive/doc/texlive-en/texlive-en.html).
Others who have attempted to use TexLive 2013 have seen problems with the pgf package.

You can check that you have a proper TeXLive install with **tex --version**:

```
  $ which tex # This is a linux command to discover the install path.
      /usr/local/texlive/2016/bin/x86_64-linux/tex
  $ tex --version
      TeX 3.14159265 (TeX Live 2016)
      kpathsea version 6.2.2
      ...

```

If your cover includes an ISBN barcode, then you must also have the `gs`
(GhostScript) command available from the command line. Check the install with:

```
    $ which gs
    /usr/bin/gs
    $ gs --version
    9.10
```

I use [TeXstudio](http://www.texstudio.org/) for interactive editing of the LaTeX files. 
I also strongly recommend installing it.


## `texclean` Utility

Running `pdflatex` generates a lot of trash intermediate files. You can
delete these with this `./bin/texclean` utility.

```bash
Usage: texclean [texfilename]
Clean up trash files for a LaTeX .tex file.

-a             clean for all .tex files in the directory.
-t             trial run -- commands are shown but not executed.
-q             quiet output during command execution.
-v             verbose output during command execution.

All the various intermediate files created during LaTex processing
of a file are deleted.
```


