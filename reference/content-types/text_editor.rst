Text Editor
===========

Description
-----------

Shows a rich text editor, which is also capable of formatting text. The output of the
editor will be stored as HTML in a string field.

Example
-------

.. code-block:: xml

    <property name="description" type="text_editor">
        <meta>
            <title lang="en">Description</title>
        </meta>
    </property>

Twig
-----

When outputting the text editor field in Twig, the `raw` filter needs to be used:

.. code-block:: twig

    {{ content.description|raw }}

.. _raw filter: https://twig.symfony.com/doc/3.x/filters/raw.html
