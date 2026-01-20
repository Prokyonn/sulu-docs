Generating thumbnails for PDF files with Ghostscript
===================================================

Ghostscript (gs) is a PostScript and PDF language interpreter and previewer.
Sulu can use the **gs** command-line program to generate thumbnail/preview images for PDF files.
The location of the **gs** program depends on your system and must be configured for Sulu to find it.

Add the configuration to ``config/packages/sulu_media.yaml``:

.. code-block:: yaml

    sulu_media:
        ghost_script:
            path: /usr/bin/gs

Common locations are:

* ``/usr/bin/gs``
* ``/usr/local/bin/gs``

You can also use the ``which gs`` command to identify the correct location.
