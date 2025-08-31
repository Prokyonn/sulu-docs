Generating Thumbnails for PDF Files with Ghostscript
====================================================

Ghostscript (gs) is a PostScript and PDF language interpreter and previewer.
Sulu is able to use the **gs** command-line program to generate thumbnail/preview images for PDF files.
The location of the **gs** program depends on your system and needs to be configured for Sulu to find it.

Add the configuration to `config/packages/sulu_media.yml`:

.. code-block:: yaml

    sulu_media:
        ghost_script:
            path: /usr/bin/gs

Common locations are:

* /usr/bin/gs
* /usr/local/bin/gs

You can also try to use the ``which gs`` command to get the location.
