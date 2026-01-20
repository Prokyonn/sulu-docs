Smart content
=============

Description
-----------

Displays a list of items based on a configurable filter. Depending on
the ``SmartContentProvider``, you can define the source of the items (datasource),
required tags, sorting criteria, and the number of results. Additionally, you can define presentation
types so that content managers can choose how items are displayed (e.g., in one or two columns). The filter is saved as a JSON string in the
database.

``SmartContentProviders`` are backend modules that handle the selected filters and
return matching items. There are several built-in providers, and you can easily add your own. This process is
described in :doc:`/cookbook/smart-content-data-provider`.

A key feature is the ``exclude_duplicates`` parameter, which allows filtering out items already used on a page. If set to ``true``, the smart content utilizes the :doc:`/bundles/website/reference-store`
to detect and filter already used items.

Parameters
----------

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - ``provider``
      - string
      - ``SmartContentProvider`` alias for the content. Default: ``pages``.
    * - ``max_per_page``
      - integer
      - Limits the results per page. Omit this parameter to disable pagination.
    * - ``page_parameter``
      - string
      - Defines the page number key to be used in the website query string. Default: ``p``.
    * - ``tags_parameter``
      - string
      - Defines the tags key to be used in the website query string. This comma-separated list of tag names will be combined (AND) with the tags selected in the backend. Default: ``tags``.
    * - ``categories_parameter``
      - string
      - Defines the categories key to be used in the website query string. This comma-separated list of category IDs will be combined (AND) with the tags selected in the backend. Default: ``categories``.
    * - ``website_tags_operator``
      - string
      - ``OR`` or ``AND`` to define how tags are combined in the query. Default: ``OR``.
    * - ``website_categories_operator``
      - string
      - ``OR`` or ``AND`` to define how categories are combined in the query. Default: ``OR``.
    * - ``properties``
      - collection
      - Defines the property names to be exposed in the HTML template.
    * - ``present_as``
      - collection
      - A collection of strings configured for different
        presentation modes. If more than one element is provided, the user can
        choose between them. The selected value is also passed to the HTML template.
    * - ``category_root``
      - string
      - Root category (key) for displaying the category tree.
    * - ``exclude_duplicates``
      - bool
      - If the provider supports duplicate detection, the content type filters
        already loaded records. Default: ``false``.

Return Value
------------

These values are available in the ``view`` variable in Twig templates:

.. list-table::
    :header-rows: 1

    * - Name
      - Type
      - Description
    * - ``dataSource``
      - string
      - UUID of the datasource.
    * - ``includeSubFolders``
      - bool
      - ``true`` if subfolders are crawled.
    * - ``categories``
      - string[]
      - Selected categories.
    * - ``categoryOperator``
      - string
      - Operator used to combine selected categories.
    * - ``tags``
      - string[]
      - Selected tags.
    * - ``tagOperator``
      - string
      - Operator used to combine selected tags.
    * - ``types``
      - string[]
      - Selected types.
    * - ``websiteCategories``
      - string[]
      - Categories selected via GET parameters.
    * - ``websiteCategoryOperator``
      - string
      - Operator used to combine GET parameter categories.
    * - ``websiteTags``
      - string[]
      - Tags selected via GET parameters.
    * - ``websiteTagOperator``
      - string
      - Operator used to combine GET parameter tags.
    * - ``sortBy``
      - string
      - Selected sort column.
    * - ``sortMethod``
      - string
      - Selected sort method (``ASC`` or ``DESC``).
    * - ``presentAs``
      - string
      - Selected presentation mode value.
    * - ``limitResult``
      - string
      - Selected limit for results.
    * - ``page``
      - int
      - Current page number.
    * - ``hasNextPage``
      - bool
      - ``true`` if another page exists.

The ``content`` values vary depending on the ``SmartContentProvider``.

.. note::

    You can identify available content properties using the Twig ``dump`` function.

SmartContentProvider
--------------------

Sulu includes several built-in ``SmartContentProviders`` for common Sulu resources.

Content Pages
~~~~~~~~~~~~~

Alias: "pages"

This provider filters content pages. You can choose a parent page as a data
source, whose child pages will be filtered.

**Parameters**

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - ``properties``
      - collection
      - Defines the property names to be exposed in the HTML template.

.. note::

    "properties" can include structure properties or extension data:

    * ``title`` - a property of the structure.
    * ``excerpt.title`` - a property of the excerpt structure extension.

    For an example, see :ref:`example`.

Snippets
~~~~~~~~

Alias: "snippets"

This provider filters snippets.

**Parameters**

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - ``type``
      - string
      - If defined, only snippets of this type are returned.
    * - ``properties``
      - collection
      - Defines the property names to be exposed in the HTML template.

Contacts - People
~~~~~~~~~~~~~~~~~

Alias: "contacts"

This provider filters contacts.

Accounts - Organizations
~~~~~~~~~~~~~~~~~~~~~~~~

Alias: "accounts"

This provider filters accounts.

Media
~~~~~

Alias: "media"

This provider filters media items.


**Parameters**

.. list-table::
    :header-rows: 1

    * - Parameter
      - Type
      - Description
    * - ``mimetype_parameter``
      - string
      - Name of the MIME type GET parameter (default: ``mimetype``).
    * - ``type_parameter``
      - string
      - Name of the media type GET parameter (default: ``type``).


The provider also supports additional filtering on the website. Use the
``mimetype_parameter`` and ``type_parameter`` to specify the name of the
GET parameters.

For example, you can filter by MIME type by adding ``?mimetype=application/pdf``
to the URL. Similarly, use ``?type=image`` to filter by media type
(which represents a group of MIME types).

.. _example:

Example for "pages" SmartContentProvider
-----------------------------------------

Page Template
~~~~~~~~~~~~~

.. code-block:: xml

    <property name="pages" type="smart_content">
        <meta>
            <title lang="en">Smart Content</title>
        </meta>

        <params>
            <param name="provider" value="pages"/>
            <param name="max_per_page" value="5"/>
            <param name="page_parameter" value="p"/>

            <param name="properties" type="collection">
                <param name="article" value="article"/>
                <param name="excerptTitle" value="excerpt.title" />
                <param name="excerptDescription" value="excerpt.description "/>
                <param name="excerptMore" value="excerpt.more" />
                <param name="excerptTags" value="excerpt.tags" />
                <param name="excerptCategories" value="excerpt.categories" />
                <param name="excerptImage" value="excerpt.image" />
                <param name="excerptIcon" value="excerpt.icon" />
            </param>

            <param name="present_as" type="collection">
                <param name="two">
                    <meta>
                        <title lang="en">Two columns</title>
                    </meta>
                </param>

                <param name="one">
                    <meta>
                        <title lang="en">One column</title>
                    </meta>
                </param>
            </param>
        </params>
    </property>

Twig Template
~~~~~~~~~~~~~

.. code-block:: twig

    <ul class="pagination">
        {% set page = view.pages.page %}

        {% if page-1 >= 1 %}
            <li><a href="{{ sulu_content_path(content.url) }}?p={{ page-1 }}">&laquo;</a></li>
        {% endif %}

        {% if view.pages.hasNextPage %}
            <li><a href="{{ sulu_content_path(content.url) }}?p={{ page+1 }}">&raquo;</a></li>
        {% endif %}
    </ul>

    <div property="pages">
        {% for page in content.pages %}
            <div class="col-lg-{{ view.pages.presentAs == 'two' ? '6' : '12' }}">
                <h2>
                    <a href="{{ sulu_content_path(page.url) }}">{{ page.title }}</a>
                </h2>

                <p>
                    <i>{{ page.excerptTitle }}</i> | <i>{{ page.excerptTags|join(', ') }}</i>
                </p>

                {% if page.excerptImages|length > 0 %}
                    <img src="{{ page.excerptImages[0].thumbnails['50x50'] }}" alt="{{ page.excerptImages[0].title }}"/>
                {% endif %}

                {{ page.article|raw }}
            </div>
        {% endfor %}
    </div>

.. note::

    If you have not defined the ``max_per_page`` parameter, you can omit the
    pagination.

Built-in SmartContentProviders
-------------------------------

Sulu includes the following built-in ``SmartContentProviders``:

**Pages** (alias: ``pages``)
    Filters content pages from the page tree. Supports tags, categories, types,
    datasource, pagination, sorting, and audience targeting.

**Snippets** (alias: ``snippets``)
    Filters snippets. Supports tags, categories, types, pagination, sorting, and
    audience targeting.

**Articles** (alias: ``articles``, if SuluArticleBundle is installed)
    Filters articles. Supports tags, categories, types (via article types/groups),
    pagination, and sorting.

**Media** (alias: ``media``)
    Filters media items. Supports tags, categories, types (document/video/image/audio),
    datasource (collections), pagination, and sorting.

**Contacts** (alias: ``contacts``)
    Filters contacts/people. Supports tags, categories, pagination, and sorting.

**Accounts** (alias: ``accounts``)
    Filters accounts/organizations. Supports tags, categories, pagination, and sorting.

To create a custom ``SmartContentProvider``, see :doc:`/cookbook/smart-content-data-provider`.
