Content Repository
==================

The Content Repository was developed to query raw content data.
Developers can decide which properties to load.

The main goal of the repository is to centralize the core features ghost, shadow
and internal links. These aspects are handled automatically when using this repository.

Usage
-----

There are two ways to use it. One in the frontend via a REST API and one via a backend service.

API
...

The URL of the API is `/admin/api/pages` or `/admin/api/pages/<uuid>`.

The mandatory parameters are:

.. list-table::

    * - Name
      - Example
      - Description
    * - locale
      - de
      - Localization to use.
    * - webspace
      - sulu_io
      - Webspace from which to load content.
    * - fields
      - ''
      - Comma separated list of properties.

.. note::

   If the ``fields`` parameter is not set the content will be resolved with the slow
   legacy system (which is deprecated).

You can specify following optional parameters:

.. list-table::

    * - Name
      - Default
      - Description
    * - exclude-ghosts
      - false
      - If true, ghost pages will be filtered.
    * - exclude-shadows
      - false
      - If true, shadow pages will be filtered.

Via the mapping, you can specify which properties will be loaded by the
repository (for example: 'title,order,article').

The list endpoint also accepts a `parent` parameter. If this parameter is set,
the given page will be used to query for children; otherwise, the webspace root is the
default.

Service
.......

The ID of the service is `sulu_page.content_repository`. The methods can be
used as described in the PHPDocs.

The mapping variable contains information for the mapping process.

.. list-table::

    * - Name
      - Description
    * - hydrateShadow
      - If false, no shadow pages are returned.
    * - hydrateGhost
      - If false, no ghost pages are returned.
    * - followInternalLink
      - If false, links are not resolved.
    * - properties
      - List of hydrated properties

You can build this mapping using the mapping builder:

.. code-block:: php

    $mapping = MappingBuilder::create()
        ->setHydrateGhost()
        ->setHydrateShadow()
        ->addProperties(['title'])
        ->getMapping();
