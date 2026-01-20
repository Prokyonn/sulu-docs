Storing Media in External Storage
=================================

Sulu stores its media using the `flysystem file system abstraction`_. This allows you to easily configure different storage backends.

By default, Sulu uses the local file system. A list of other supported storage backends and their installation instructions
can be found in the Flysystem documentation here: https://github.com/thephpleague/flysystem-bundle/blob/3.x/docs/2-cloud-storage-providers.md

The following represents the default configuration for Sulu and can be adjusted to meet your needs:

.. code-block:: yaml

    # config/packages/flysystem.yaml
    flysystem:
        storages:
            default.storage:
                adapter: 'local'
                options:
                    directory: '%kernel.project_dir%/var/storage/default'

    # config/packages/sulu_media.yaml
    sulu_media:
        storage:
            flysystem_service: 'default.storage'


.. warning::

    Please check the section :ref:`what-about-image-formats` to avoid confusion about how image formats are handled in Sulu,
    and why they cannot be stored in the configured flysystem storage.

.. _what-about-image-formats:

What About Image Formats?
-------------------------

.. note::

    Only the original files will be uploaded to the external storage. Image formats and thumbnails will still be generated
    in the local directory. This is because image formats are generated in Sulu on demand. Specifically, when
    an image format is requested for the first time, Sulu generates the image from the original file and stores it in the public
    directory. The web server then acts as a proxy. If the image is requested again, it checks the public
    directory and returns the previously generated image instead of regenerating it. External storages like S3,
    Google Cloud Storage, or Azure Blob Storage do not natively support this proxy or CDN functionality.

    If you want to store image formats in an external service, you must use a CDN like Fastly, Cloudflare, or others
    that support caching generated image formats for extended periods. Some hosters allow you to configure a CDN directly on specific URLs.
    In Sulu, all URLs under ``/uploads/media/*`` must be routed through a proxy or CDN. If your chosen CDN
    requires a custom domain, you can use Symfony's CDN feature via:

    ``{{ asset(media.thumbnail['40x40']) }}``

    and `configure a CDN domain`_ in the Symfony ``framework.assets`` configuration. If you have tested your proxy or CDN and it correctly
    caches the generated images, you can disable saving thumbnails to the local filesystem by setting
    ``sulu_media.format_cache.save_image`` to ``false`` in ``config/packages/sulu_media.yaml``. It is recommended to use an environment variable
    to still enable local storage during development.

    Important: Never disable the format cache unless you have set up a CDN or proxy. Otherwise, your server will
    regenerate the image format on every request, which can overwhelm your server as image generation is resource-intensive.


.. _Configure a CDN domain: https://symfony.com/doc/6.4/reference/configuration/framework.html#base-urls
.. _flysystem file system abstraction: https://github.com/thephpleague/flysystem
