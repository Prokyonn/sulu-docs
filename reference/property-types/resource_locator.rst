Resource locator
================

Description
-----------

Displays a text line with a non-editable prefix, which represents the path to
this position in the content tree. The segment for the current page can be edited
in the available text line. Additionally, there is a button showing the URL history
of the current page, where historical URLs can be deleted or
reactivated.

Tags
----

.. list-table::
    :header-rows: 1

    * - Tag
      - Description
    * - ``sulu.rlp``
      - The resource locator with this tag defines the URL for a specific page.
    * - ``sulu.rlp.part``
      - Fields marked with this tag are used to generate the URL for a specific page.
        If more than one field is marked, their values will be concatenated into the resource locator.

Parameters
----------

No parameters available.

Example
-------

.. code-block:: xml

    <property name="title" type="text_line">
        <tag name="sulu.rlp.part"/>
    </property>

    <property name="subtitle" type="text_line">
        <tag name="sulu.rlp.part"/>
    </property>

    <property name="url" type="resource_locator">
        <meta>
            <title lang="en">Resource locator</title>
        </meta>

        <tag name="sulu.rlp"/>
    </property>

Twig
----

You must use the :doc:`../twig-extensions/functions/sulu_content_path` Twig extension
to render the full URL.

.. code-block:: twig

    {{ sulu_content_path(content.url) }}
