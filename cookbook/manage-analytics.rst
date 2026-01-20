Managing Analytics
==================

Sulu provides content managers with an easy way to manage analytics codes and automatically appends
them to the website output without requiring changes to Twig templates. You can find the list of analytics under the webspace section.

The analytics configuration consists of:

.. list-table::
    :header-rows: 1

    * - Property
      - Description
    * - Title
      - A unique label to identify the configuration.
    * - Domains
      - The specific domains where these analytics should be appended.
    * - All Domains
      - Whether the analytics should be appended to all domains.
    * - Type
      - The analytics provider (google, google_tag_manager, matomo, custom).
    * - Content
      - The tracking code or key for the analytics service.

Sulu supports various analytics systems such as Google Analytics or Matomo.
These codes are automatically added with the given key or site ID (for Matomo). To add other systems, select the "custom" type and paste the tracking code into the textarea.

.. warning::

    Custom analytics are appended without validation; therefore, incorrect code can break the website immediately after saving.

Overriding Analytics Templates
------------------------------

You can override the analytics templates using the
`Symfony template overriding mechanism <http://symfony.com/doc/current/book/templating.html#overriding-bundle-templates>`_.

The relevant template folders are:

* ``SuluWebsiteBundle/Analytics/google``
* ``SuluWebsiteBundle/Analytics/google_tag_manager``
* ``SuluWebsiteBundle/Analytics/matomo``
* ``SuluWebsiteBundle/Analytics/custom``

Each of these folders can contain multiple templates corresponding to the desired
position of the content:

* ``body-open.html.twig``
* ``body-close.html.twig``
* ``head-open.html.twig``
* ``head-close.html.twig``

You can access the following information in the Twig variable ``analytics``:

.. list-table::
    :header-rows: 1

    * - Name
      - Type
      - Description
    * - id
      - int
      - A unique identifier for the analytics configuration.
    * - title
      - string
      - The title of the analytics configuration.
    * - allDomains
      - boolean
      - Indicates whether the analytics apply to all domains or only specific ones.
    * - content
      - mixed
      - Data structure varies by type.
    * - type
      - string
      - google / google_tag_manager / matomo / custom
    * - domains
      - array
      - Array of associated domains.

.. note::

    For Google or Google Tag Manager, the ``content`` property contains the key.
    For Matomo, it contains an associative array with ``url`` and ``siteId``.
    For the custom type, it contains the entire script (excluding the ``<script>`` tag).
