How to Manage Analytics
=======================

Sulu gives the content manager an easy way to manage analytic codes and appends
them automatically to the website output without any changes in the Twig
template. You can find the list of analytics under the webspace section.

The analytics consist of:

.. list-table::
    :header-rows: 1

    * - Title
      - To identify it.
    * - Domains
      - On which domain this analytic should be appended.
    * - All Domains
      - Should it be appended to all domains.
    * - Type
      - The type (google, google_tag_manager, matomo, custom).
    * - Content
      - The code or key of the analytic.

Sulu can handle different types of analytic systems, like Google or Matomo.
These codes will be automatically added with the given key and site-id (for
Matomo). To add other systems, simply choose the type `custom` and copy and paste
the code into the textarea.

.. warning::

    Be aware that custom analytics will not be evaluated and appended without
    validation; therefore, it could break the website directly after saving.

Override analytics template
---------------------------

You are able to override the analytics template with the
`Symfony template overriding mechanism <http://symfony.com/doc/current/book/templating.html#overriding-bundle-templates>`_.

There are four relevant template folders:

* ``SuluWebsiteBundle/Analytics/google``
* ``SuluWebsiteBundle/Analytics/google_tag_manager``
* ``SuluWebsiteBundle/Analytics/matomo``
* ``SuluWebsiteBundle/Analytics/custom``

Each of these folders can contain multiple templates according to the desired
position of its content:

* ``body-open.html.twig``
* ``body-close.html.twig``
* ``head-open.html.twig``
* ``head-close.html.twig``

You can access the following information in the Twig variable ``analytics``.

.. list-table::
    :header-rows: 1

    * - Name
      - Type
      - Description
    * - id
      - int
      - A unique identifier of the analytic.
    * - title
      - string
      - The title of the analytic.
    * - allDomains
      - boolean
      - Indicates whether the analytic is on all domains or only a specific one.
    * - content
      - mixed
      - Differs for the type.
    * - type
      - string
      - google / google_tag_manager / matomo / custom
    * - domains
      - array
      - Array of associated domains.

.. note::

    The ``content`` property contains the key for the type `google` / `google_tag_manager`,
    for `matomo` an associated array of ``url`` and ``siteId``, and for the `custom` type,
    the whole script (except the ``<script>`` tag).
