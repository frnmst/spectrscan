# spectrscan

An unintrusive frontend of scanimage which acts as a
paper to pdf converter suitable for texts.

## Table of contents

[](TOC)

- [spectrscan](#spectrscan)
    - [Table of contents](#table-of-contents)
    - [Notes](#notes)
    - [Examples](#examples)
    - [Help](#help)
    - [Relevant features](#relevant-features)
    - [Relevant links and ideas](#relevant-links-and-ideas)
    - [Dependencies and explanations](#dependencies-and-explanations)
    - [Coming soon](#coming-soon)
    - [Origin of the name](#origin-of-the-name)
    - [Troubleshooting](#troubleshooting)
    - [Tested on](#tested-on)
    - [License](#license)

[](TOC)

## Notes

Remember that the output file supplied in the command line can either be an 
empty file or an existing PDF file. In the latter case, newly scanned documents
will be automatically appended to the tail of the file.

Also note that the mode, resolution and source options must be supported by 
your scanner.

Default options are defined in the `./spectrscan.conf` file which is a symlink 
that points to the `./spectrscan.HP_Officejet_2620.conf` profile.

## Examples

- Scan double sided paper using the ADF

        ./spectrscan -t out.pdf

  Take 3 papers as an example. Each paper has 2 sides, so there are
  6 total sides. Mark each side with a progressive number:

        1,2,3,4,5,6

  Put the papers in the ADF so that sides 1,3,5 (in this order) will be scanned.
  Side 1 is the one facing towards you.

  Once these 3 sides are scanned you should see sides 6,4,2 (in this order) out 
  of the ADF. Side 6 is now facing towards you. This means that the ADF 
  reverses the order of the papers (if you have a scanner with this feature).

  You can now sides 6,4,2 (in this order) in the ADF. spectrscan will sort the
  the sides so that the final result will be

        1,2,3,4,5,6

  In case your scanner does not reverse the order you can use the following 
  option

        ./spectrscan  --two-sided-reverse out.pdf
  

  which is comparable to a result of

        1,3,5,6,4,2

  for the previous example

- Scan in colour, with a resolution of 300 DPI, using the flatbed,
  on the output file `out.pdf`

        ./spectrscan -m Color -r 300 -s Flatbed out.pdf

- Enable unpaper (same procedure for imagemagick):

        ./spectrscan -u out.pdf
        ./spectrscan --unpaper_options='<unpaper options>' out.pdf

If the scanned text results unreadable try using the `Gray` mode instead
of the default `Lineart`.

By default contrast is set at a very high level. You can modify the parameters 
with something like

        ./spectrscan --imagemagick_options='-normalize -level 20%,100%,1.0'

and see what happens.

## Help

```
Usage: spectrscan [OPTION] OUTFILE
An unintrusive frontend of scanimage which acts as a
paper to pdf converter suitable for texts.

If the ouput file exists then the new scanned documents will be added
as the tail of the existing one.

The default system scanner is used.

Mandatory arguments to long options are mandatory for short options too.
Options:
    -h, --help                          print this help
    -i, --imagemagick=OPTIONS           pass options to ImageMagick
                                        to post-process the documents
    --list-modes                        list all possible scan modes
    --list-resolutions                  list all possible resolutions
    --list-sources                      list all possible sources
    -m, --mode=MODE                     scan in Color, Lineart, Gray or whatever
                                        supported method
    --print-flags                       print the enabled options. This can also
                                        be used to print the default options
    -r, --resolution=RESOLUTION         page resolution in DPI
    -s, --source=SOURCE                 scan from the ADF, Flatbed or whatever
                                        supported method
    -t, --two-sided                     toggle preserve the order in
                                        double sided paper: scan a batch of
                                        papers one side, then the other
    --two-sided-reverse                 same as '--two-sided' but with the need of
                                        reversing every single paper. This
                                        option conflicts with '--two-sided'
    -u, --unpaper[=OPTIONS]             enable unpaper. You may pass
                                        options to unpaper


Exit status:
 0  if OK,
 1  if an error occurred.

This  is  free  software: you are free to change and redistribute it.  There
is NO WARRANTY, to the extent  permitted by law.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
Copyright  ©  2017  Franco Masotti.
```

## Relevant features

- Parallel image processing (based on the number of processor cores)
  cuts the time by a factor of `#cores` after page scanning has taken place.
  This is very effective for a large number of pages.

- Two-sided page scanning.

- Basic unpaper and ImageMagick post-processing.

## Relevant links and ideas

https://bugs.launchpad.net/simple-scan/+bug/983441

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
  - TIFF to PDF converter and image processing tool

- [unpaper](https://github.com/Flameeyes/unpaper)
  - Remove issues with scanned images (paper margins, etc...)

- [PDFtk](https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit/) 
  - Assemble the PDF documents into a single file
  - [Debian version](https://libreplanet.org/wiki/List_of_software_that_does_not_respect_the_Free_System_Distribution_Guidelines#pdftk)
    which is fully free
  - This "binary" [AUR version](https://aur.archlinux.org/packages/pdftk-bin/) is based on the Debian version

- [GNU Parallel](http://www.gnu.org/software/parallel/)
  - Execute image post processing jobs in parallel

- [GNU stream editor](https://www.gnu.org/software/sed/)
  - Remove leading zeros from filenames

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

- show preview
- compression
- number of pages to scan
- option for prompting for each page to scan
- better default options to pass to unpaper
- OCR (training (GOCR)? + text file outputs)
  - options: keep PDF, keep txt, keep both
- better processing
  - watch inotifies for a new out*.tiff
    would be faster than post-processing in parallel

## Origin of the name

The name comes from [spectrwm](https://github.com/conformal/spectrwm)

## Troubleshooting

If pdftk repots that PDF files are missing this is probably due to the
new ImageMagick security policies. Remove `PDF` from `/etc/ImageMagick-7/policy.xml`

This is the error returned by `convert`:

    convert: attempt to perform an operation not allowed by the security policy `PDF' @ error/constitute.c/IsCoderAuthorized/408.

## Tested on

| Distribution | Scanner Name | `vendor:product` | Connection | 
|--------------|--------------|------------------|------------|
| Parabola GNU/Linux-libre | HP Officejet 2620 | `03f0:c911` | USB |

## License

![https://www.gnu.org/graphics/gplv3-127x51.png](https://www.gnu.org/graphics/gplv3-127x51.png)

Copyright (C) 2017-2018 frnmst (Franco Masotti) <franco.masotti@live.com>

spectrscan is free software: you can redistribute it and/or modify it 
under the terms of the GNU General Public License as published by the Free 
Software Foundation, either version 3 of the License, or (at your option) any 
later version.
