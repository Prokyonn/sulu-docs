Text editor
===========

Description
-----------

Displays a rich text editor capable of formatting text. The output of the
editor is stored as HTML in a string field.

Example
-------

.. code-block:: xml

    <property name="description" type="text_editor">
        <meta>
            <title lang="en">Description</title>
        </meta>
    </property>

Twig
----

When outputting the text editor field in Twig, the `raw filter`_ must be used:

.. code-block:: twig

    {{ content.description|raw }}

.. _raw filter: https://twig.symfony.com/doc/3.x/filters/raw.html

What About Images in the Text Editor?
-------------------------------------

A common question is how to handle images in a text editor property.
The answer is: you do not handle them within the text editor.

In Sulu, the text editor is intended only for text formatting. Unlike other
CMS systems where all content lives inside one large WYSIWYG editor,
Sulu follows the principle of separating content from presentation.

Therefore, images should be handled as separate media properties. This approach gives developers and designers full
control over how images are presented on the website. It makes it easier to provide content in
different formats through headless APIs for apps and other services. It also simplifies website
redesigns, as content outlives the design.

A typical Sulu page uses the :doc:`block type <block>` to allow editors
to create flexible pages. See the :doc:`block type documentation <block>` for more
information.
