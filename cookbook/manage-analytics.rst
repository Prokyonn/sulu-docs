How to Manage Analytics
=======================

Sulu gives the content manager an easy way to manage analytics codes and appends
them automatically to the website output without any changes in the
Twig template. You can find the list of analytics under the "Webspace" section.

An analytic consists of:

.. list-table::
    :header-rows: 1

    * - Title
      - To identify the analytic.
    * - Domains
      - The domain on which this analytic should be appended.
    * - All Domains
      - Whether the analytic should be appended to all domains.
    * - Type
      - The type (e.g., Google, Google Tag Manager, Matomo, or Custom).
    * - Content
      - The code or key for the analytic.

Sulu can handle different types of analytics systems, such as Google or Matomo.
These codes will be automatically added with the given key and site ID (for
Matomo). To add other systems, simply choose the "Custom" type and copy and paste
the code into the text area.

.. warning::

    Be aware that custom analytics will not be evaluated and appended without
    validation. Therefore, it could break the website directly after saving.

Overriding the Analytics Template
---------------------------------

You can override the analytics template with the
`Symfony template overriding mechanism <http://symfony.com/doc/current/book/templating.html#overriding-bundle-templates>`_.

There are four relevant template folders:

* ``SuluWebsiteBundle/Analytics/google``
* ``SuluWebsiteBundle/Analytics/google_tag_manager``
* ``SuluWebsiteBundle/Analytics/matomo``
* ``SuluWebsiteBundle/Analytics/custom``

Each of these folders can contain multiple templates, according to the desired
position of its content:

* ``body-open.html.twig``
* ``body-close.html.twig``
* ``head-open.html.twig``
* ``head-close.html.twig``

You can access the following information in the Twig variable `analytics`.

.. list-table::
    :header-rows: 1

    * - Name
      - Type
      - Description
    * - id
      - int
      - A unique identifier for the analytic.
    * - title
      - string
      - The title of the analytic.
    * - allDomains
      - boolean
      - Indicates whether the analytic is on all domains or only a specific one.
    * - content
      - mixed
      - Differs depending on the type.
    * - type
      - string
      - `google`, `google_tag_manager`, `matomo`, or `custom`.
    * - domains
      - array
      - An array of associated domains.

.. note::

    The `content` property contains the key for the `google` and `google_tag_manager` types,
    an associative array of `url` and `siteId` for the `matomo` type, and the whole script
    (except the `<script>` tag) for the `custom` type.
