# spectrscan

An unintrusive frontend of scanimage which acts as a
paper to pdf converter suitable for texts.

## Table of contents

[](TOC)

- [spectrscan](#spectrscan)
    - [Table of contents](#table-of-contents)
    - [Examples](#examples)
        - [Default options](#default-options)
        - [Two-sided](#two-sided)
        - [Misc](#misc)
    - [Path](#path)
    - [Help](#help)
    - [Relevant features](#relevant-features)
    - [Relevant links and ideas](#relevant-links-and-ideas)
    - [Dependencies and explanations](#dependencies-and-explanations)
    - [Coming soon](#coming-soon)
    - [Origin of the name](#origin-of-the-name)
    - [Troubleshooting](#troubleshooting)
    - [License](#license)

[](TOC)

## Examples

Remember that the output file supplied in the command line can either be an 
empty file or an existing PDF file. In the latter case, newly scanned documents
will be automatically appended to the tail of the file.

### Default options

- Scan in lineart mode, from the automatic document feeder,
  with a resolution of 600 DPI, using Unpaper and with image enhancing options 
  on the output file `out.pdf`

        ./specrescan out.pdf

### Two-sided

- Same as before but for double sided paper

        ./spectrscan -o out.pdf

### Misc

- Scan in colour, with a resolution of 300 DPI, using the flatbed,
  on the output file `out.pdf`

        ./spectrscan -m Color -r 300 -s Flatbed out.pdf

- Disable unpaper (same procedure for imagemagick):

        ./spectrscan -ufalse out.pdf
        ./spectrscan --unpaper_options=false out.pdf

If the scanned text results unreadable try using the `Gray` mode instead
of the default `Lineart`.

Currently, passing options to unpaper and imagemagick is not working. You 
should edit the options directly in the script. By default, contrast is set at 
a very high level. You can edit

    imagemagick_options="-normalize -level 70%,100%,1.0"

with something like:

    imagemagick_options="-normalize -level 20%,100%,1.0"

and see what happens.

## Path

You can call spectrscan from any directory by modifying the shell's path.
You can then call spectrscan:

    cd <specrscan repo directory>
    PATH="$PATH:$(pwd)"
    cd <document destination directory>
    spectrscan <whatever>

## Help

```
Usage: spectrscan [OPTIONS] OUTFILE
An unintrusive frontend of scanimage which acts as a
paper to pdf converter suitable for texts.

If the ouput file exists then the new scanned documents will be added
as the tail of the existing one.

The default system scanner is used.

Options:
    -h, --help                  print this help
    -i, --imagemagick-options   pass options to ImageMagick
                                to post-process the documents
    -m, --mode                  scan in Color, Lineart, Gray or whatever
                                supported method
        --list-modes            list all possible scan modes
    -o, --odd-even              toggle preserve the order in double sided paper:
                                scan a batch of papers one side, then the other
    -r, --resolution            page resolution in DPI
        --list-resolutions      list all possible resolutions
    -s, --source                scan from the ADF, Flatbed or whatever
                                supported method
        --list-sources          list all possible sources
    -u, --unpaper-options       pass options to unpaper
                                to post-process the documents

Current enabled options:
--imagemagick-options="-contrast-stretch 0.5%x10% -compress lzw"
--mode "Lineart" --odd-even="false" --resolution "600"
--source "ADF" --unpaper-options="true"

The magic values of "true" and "false" can be used
to enable or disable:
    --odd-even=<value>
    --unpaper-options=<value> (if "false", unpaper is disabled)

Dependencies:   GNU Bash; GNU Core Utilities; Gawk; SANE; ImageMagick
                unpaper; PDFtk; GNU Parallel; Netpbm

Exit status:
 0  if OK,
 1  if an error occurred.

Copyright  ©  2017  Franco Masotti.  License GPLv3+: GNU GPL version 3 or
later <http://gnu.org/licenses/gpl.html>.
This  is  free  software: you are free to change and redistribute it.  There
is NO WARRANTY, to the extent  permitted by law.
```

## Relevant features

- Parallel immage processing (based on the number of processor cores)
  cuts the time by a factor of `#cores` after page scanning has taken place.
  This is very effective for a large number of pages.

- Odd-even page numbers scanning.

- Basic unpaper and ImageMagick post-processing.

## Relevant links and ideas

https://bugs.launchpad.net/simple-scan/+bug/983441

http://netpbm.sourceforge.net/doc/pamfix.html

http://www.jduck.net/blog/2008/01/05/ocr-scanning/

https://www.ubuntu-user.com/Magazine/Archive/2013/18/Scanning-and-editing-text-with-OCR

http://www.jpeek.com/articles/linuxmag/2006-08/

## Dependencies and explanations

- [GNU Bash](http://www.gnu.org/software/bash/bash.html)
  - Scipting language interpreter

- [GNU Core Utilities](https://www.gnu.org/software/coreutils/)
  - Basic software like `ls`, `cat`, etc...

- [Gawk](http://www.gnu.org/software/gawk/)
  - Filter some output from miscellaneous commands

- [SANE](http://www.sane-project.org/)
  - Scanner software

- [ImageMagick](http://www.imagemagick.org/)
  - PNM to PDF converter and image processing tool

- [unpaper](https://github.com/Flameeyes/unpaper)
  - Remove issues with scanned images (paper margins, etc...)

- [PDFtk](https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit/) 
  - Assemble the PDF documents into a single file
  - [Debian version](https://libreplanet.org/wiki/List_of_software_that_does_not_respect_the_Free_System_Distribution_Guidelines#pdftk)
    which is fully free
  - This "binary" [AUR version](https://aur.archlinux.org/packages/pdftk-bin/) is based on the Debian version

- [GNU Parallel](http://www.gnu.org/software/parallel/)
  - Execute image post processing jobs in parallel

- [Netpbm](http://netpbm.sourceforge.net/)
  - Fix the newly scanned immages by truncating the unnecessary parts. This 
    is mostly useful when scanning from the ADF
  - In my case I have an HP Officejet 2620 connected via USB to a server as a 
    network printer/scanner. Scanning using the "Flatbed" option poses no 
    problems, while using the "ADF", the image is somehow corrupted
    and it contains a black box adjacent to the scanned document.
    If I try to post process the image with unpaper and/or ImageMagick
    serveral errors are reported. The only solution I found was to use
    `pamfix -truncate`. I still haven't determined the cause of this problem
  - A "libre" version of the package is available in Parabola GNU/Linux-libre,
    described like this:

    > A toolkit for manipulation of graphic images, without nonfree parts and
    > patent issues 

## Coming soon

- Options to add
  - Compression
  - Number of pages to scan
  - Option for prompting for each page to scan
  - Better default options to pass to unpaper
  - OCR (training (GOCR)? + text file outputs)
    - options: keep PDF, keep txt, keep both
- Better parallel processing
  - Watch inotifies for a new out*.pnm
    would be faster than post-processing in parallel

## Origin of the name

The name comes from [spectrwm](https://github.com/conformal/spectrwm)

## Troubleshooting

If pdftk repots that PDF files are missing this is probably due to the
new ImageMagick security policies. Remove `PDF` from `/etc/ImageMagick-7/policy.xml`

This is the error returned by `convert`:

    convert: attempt to perform an operation not allowed by the security policy `PDF' @ error/constitute.c/IsCoderAuthorized/408.

## License

![https://www.gnu.org/graphics/gplv3-127x51.png](https://www.gnu.org/graphics/gplv3-127x51.png)

Copyright (C) 2017 frnmst (Franco Masotti) <franco.masotti@student.unife.it>

spectrscan is free software: you can redistribute it and/or modify it 
under the terms of the GNU General Public License as published by the Free 
Software Foundation, either version 3 of the License, or (at your option) any 
later version.
