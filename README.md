Latex CreateSpace BookCover
===========================

This is a template for creating a quality book cover PDF suitable for
use by CreateSpace and Ingram Spark print-on-demand paperback publishers.
The default setup is for a 6" x 9" paperback, but the parameters are
are easily changed.

## Structure

`BookParameters.tex` is a LaTeX file containing defines about your book. 
You edit this while constructing your book. In my projects, all LaTeX
root files for the same publication include this file.

The spine width is determined by the page count and the width of the
paper stock. The CreateSpace white stock is set by default. 

You will need to provide your own image files. I've included some example
files that are CC licensed from Wikipedia as an example.

There are four files you can expect to edit:

- **cover/BackCover.tex**
- **cover/FrontCover.tex**
- **cover/SpineCover.tex**
- **BookParameters.tex**

You will need to adjust image sizes and spacing for your own text and 
image shapes by editing the .tex file. Reasonable familiarity with LaTeX
is required.

### Building ###

```bash
cd cover
pdflatex BackCover.tex 1> ./logs/1_pdflatex.log
pdflatex FrontCover.tex 1> ./logs/2_pdflatex.log
pdflatex SpineCover.tex 1> ./logs/3_pdflatex.log
pdflatex Cover.tex 1> ./logs/4_pdflatex.log
cp Cover.pdf ..
```
or you can run the provided script:

```bash
    ./bin/makecover
```

## License

This code is MIT Licensed and under the Free License 1.0.0. You are free to
choose the one that works for you. if you want to grab parts for use in your
own project then please remove my name and hack away.

## Installing 

Just copy the contents to your directories and use as is. You will need a
complete LaTeX environment, which includes `pdflatex`. I recommend TexLive
2016. I also use `TeXstudio` for interactive editing of the LaTeX files.


