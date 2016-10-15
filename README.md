Latex CreateSpace BookCover
===========================

This is a template for creating a quality book cover PDF suitable for
use by CreateSpace and Ingram Spark print-on-demand paperback publishers.
The default setup is for a 6" x 9" paperback, but the parameters are
are easily changed.

[cover]: images/CoverExample.jpg "The Example Createspace Paperback Cover"

![This would be a Paperback Cover Image, but I can't find the file, Sorry.][cover]

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

### Building ###

You can run the provided script:

```bash
    ./bin/makecover
```

which executes exactly these commands:

```bash
mkdir -p cover/logs
cd cover
pdflatex -synctex=1 -interaction=nonstopmode  FrontCover.tex 1> ./logs/1_pdflatex.log
pdflatex -synctex=1 -interaction=nonstopmode  BackCover.tex 1> ./logs/2_pdflatex.log
pdflatex -synctex=1 -interaction=nonstopmode  SpineCover.tex 1> ./logs/3_pdflatex.log
pdflatex -synctex=1 -interaction=nonstopmode  FrontCover.tex 1> ./logs/4_pdflatex.log
pdflatex -synctex=1 -interaction=nonstopmode  BackCover.tex 1> ./logs/5_pdflatex.log
pdflatex -synctex=1 -interaction=nonstopmode  SpineCover.tex 1> ./logs/6_pdflatex.log
pdflatex -synctex=1 -interaction=nonstopmode  Cover.tex 1> ./logs/7_pdflatex.log
../bin/texclean -a 1> ./logs/8_texclean.log
cp Cover.pdf ..
```

## Page Count and Spine

The *TotalPageCount* parameter in `BookParameters.tex` controls the
thickness of your book's spine. The page count times the *SinglePageThicknessPt*
determines the thickness. This is currently set to the thickness of standard
white paper used by CreateSpace, but you may need to change it to match the bond
used by your print on demand publisher.

Once you have built a PDF for your book, you can set the page by:

- [1] editing `BookParameters.tex` directly
- [2] or running `./bin/setBookParameter TotalPageCount nnn`
- [3] or running `setBookTotalPageCount [logfile]` against
the final pdflatex log file created when you built your pdf.
This extracts the page count from the log and modifies `BookParameters.tex`.
You will need the `./bin` directory in your path.

`SpineCover.tex` automatically adjusts the format of the spine based on
the page count, but you will need to edit it if your book title, subtitle,
author name, or publisher names are significantly longer than those
provided in the example.
 
## License

This code is MIT Licensed and under the Free License 1.0.0. You are free to
choose the one that works for you. if you want to grab parts for use in your
own project then please remove my name and hack away.

## Installing 

Just copy the contents to your directories and use as is. You will need a
complete LaTeX environment, which includes `pdflatex`. I recommend TexLive 2016.

I also use `TeXstudio` for interactive editing of the LaTeX files.


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


