# spectrscan

An unintrusive frontend of scanimage which acts as a
paper to pdf converter suitable for texts.

## Help

```
Usage: spectrscan [OPTIONS] OUTFILE
An unintrusive frontend of scanimage which acts as a
paper to pdf converter suitable for texts.

If the ouput file exists then the new scanned documents will be added
as the tail of the existing one.

The default system scanner is used (so this must be already configured).

Options:
    -h, --help                  print this help
    -m, --mode                  scan in Color, Lineart, Gray or whatever
                                supported method
        --list-modes            list all possible scan modes
    -o, --odd-even              preserve the order in double sided paper:
                                scan a batch of papers one side, then the other
    -r, --resolution            page resolution in DPI
        --list-resolutions      list all possible resolutions
    -s, --source                scan from the ADF, Flatbed or whatever
                                supported method
        --list-sources          list all possible sources

Default: --mode=Lineart --resolution=600 --source=ADF

Dependencies: Sane, Imagemagick, Pdftk, GNU Parallel, GAWK.

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

## Relevant links and ideas

https://bugs.launchpad.net/simple-scan/+bug/983441

http://netpbm.sourceforge.net/doc/pamfix.html

http://www.jduck.net/blog/2008/01/05/ocr-scanning/

https://www.ubuntu-user.com/Magazine/Archive/2013/18/Scanning-and-editing-text-with-OCR

## Coming soon

- Options to add
  - Compression
  - Number of pages to scan
  - Basic image enhancer options (to pass to ImageMagick)
  - `unpaper`
  - OCR (training (GOCR)? + text file outputs)
- Better parallel processing
  - Watch inotifies for a new out*.pnm
    would be faster than post-processing in parallel

## Dependencies

- [GNU Bash](http://www.gnu.org/software/bash/bash.html)
- [Gawk](http://www.gnu.org/software/gawk/)
- [SANE](http://www.sane-project.org/)
- [ImageMagick](http://www.imagemagick.org/)
- [PDFtk](https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit/) 
  [Debian version which is fully free](https://libreplanet.org/wiki/List_of_software_that_does_not_respect_the_Free_System_Distribution_Guidelines#pdftk)
  [AUR version](https://aur.archlinux.org/packages/pdftk-bin/)
- [GNU Parallel](http://www.gnu.org/software/parallel/)

## Origin of the name

The name comes from [spectrwm](https://github.com/conformal/spectrwm)

## License

![https://www.gnu.org/graphics/gplv3-127x51.png](https://www.gnu.org/graphics/gplv3-127x51.png)

Copyright (C) 2017 frnmst (Franco Masotti) <franco.masotti@student.unife.it>

spectrscan is free software: you can redistribute it and/or modify it 
under the terms of the GNU General Public License as published by the Free 
Software Foundation, either version 3 of the License, or (at your option) any 
later version.
