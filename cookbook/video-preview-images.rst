Generating thumbnails for video files with FFmpeg
=================================================

FFmpeg is a library for processing video files. Sulu can use these libraries
to generate thumbnail images for video files.

1. Install the FFmpeg library:

.. code-block:: bash

    composer require php-ffmpeg/php-ffmpeg

2. Add the configuration to ``config/packages/sulu_media.yaml``:

.. code-block:: yaml

    sulu_media:
        ffmpeg:
            ffmpeg_binary: /usr/local/bin/ffmpeg # path to ffmpeg
            ffprobe_binary: /usr/local/bin/ffprobe # path to ffprobe
