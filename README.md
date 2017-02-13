wisdom-songbook
===============

wisdom-songbook is a collection of song lyrics etc for the contributors' 
private use. 


Environment
-----------

wisdom-songbook is a project written in LaTeX. You must have LaTeX with
`pdflatex` command and some standard packages installed on your system.
Also `lilypond-book` binary must be found.

Our project requires only some pretty standard LaTeX packages, which 
are included in many LaTeX installations by default, to be installed on 
the system. They are all included with `\usepackage` commands in the 
beginning of the project's main file: `wisdom-songbook.tex`.

One of the package dependencies, `songs`, is included in the project
tree and used instead of a one possibly installed on the system. This 
is because of compatibility reasons to ensure a certain version: the 
package is used heavily and some of its commands are redefined.

The `songs` package uses two binaries to create song indexes. These must
be manually compiled from the source in `ext_packages/src` and placed
into `ext_packages/bin/`.

#### Compiling `songs` binaries on UNIX ####

This requires only basic compiling tools: `make` and `gcc` which are
most likely already installed on your system. 

Go to the base directory of our project and execute the following commands:

        cd ext_packages/src
        tar xvzf songs-2.18.tar.gz
        cd songs-2.18
        COMPILEBASEDIR=$(pwd)
        ./configure
        make   # don't worry if there are some errors
        make install prefix="$COMPILEBASEDIR/tmpinstall"
        mkdir ../../bin
        cp tmpinstall/bin/* ../../bin/
        cd ..
        rm -R songs-2.18
        

Creating a PDF document
-----------------------

If you're on an UNIX system, you can simply use the provided 
`compile_wisdom.sh` shell script to build a PDF document out of our 
project.

Otherwise, you should to run binaries in the following sequence: 
`lilypond-book`, `pdflatex`, `songidx` (for song titles), `songidx`
(for authors) and `pdflatex` again.


Project structure and guidelines
--------------------------------

Project's main file is `wisdom-songbook.tex`. It includes all the
other files in the project and contains configuration.

Long `\renewcommand`s ought to be put into `wisdom-songbook-extra.tex` to
maintain readability of the main file.

Song data and other *content* will be in various files inside `content`
subdirectory and will be inputed into the main file. Images are put into
`content/img`.

External packages (only `songs` for now) are in `ext_packages` subdirectory.

Lines ought to be less than 100 characters long (is anyone using 80 column
terminals still?), unless it is too much trouble.

See `songs` package documentation in [http://songs.sourceforge.net/songsdoc/songs.html](http://songs.sourceforge.net/songsdoc/songs.html).

Stuff inside `songs` environment (the files in `content` directory named
with a prefix `songs_`) ought to contain only individual songs (and data 
related to them) between `\beginsong` and `\endsong` tags plus other 
data wrapped in an `intersong` environment. 

Use `\sclearpage` to jump to the beginning of a new page and `\scleardpage` to
hop to the beginning of a new left-side page. Suggest a good page brake spot
with `\brk`. These are mostly not needed, as `songs` package does quite a good
job in deciding these.

If using measure bars and a measure bar ends at the end of a lyric line, add
a measure bar line to *both* the end of the line and the beginning of the 
next one (if one exists).

If it is necessary to mark beats, use `.` as a chord name, like this: `\[.]`.

Full melodies are written using `lilypond` syntax. See documentation in 
[http://lilypond.org/](http://lilypond.org/). It seems best to put `lilypond`
parts outside of verses (but inside of a song) to ensure correct line breaking.


Tentative TODO
--------------

*  Add more songs
*  Add chords and measure marks for existing songs
*  Add translations and explanations for existing songs
*  Organize songs better and decide the categories (= chapters / parts)
*  Possibly add poems, prayers etc in between songs or in their own category(?)
*  Possibly integrate lilypond and use where needed to display melodies
*  Further develop visual style of the end document
*  Make printing of double-sided A5/A6 easy using single/double-sided A4 
   printer

