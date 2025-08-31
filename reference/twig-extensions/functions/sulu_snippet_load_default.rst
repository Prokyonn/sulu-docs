``sulu_snippet_load_default``
=============================

.. note::

    This method is deprecated. Use :doc:`sulu_snippet_load_by_area` instead.

Returns the content of the default snippet for the given snippet type.

.. code-block:: jinja

    {% set snippets = sulu_snippet_load_default('default') %}
    {{ snippets[0].content.title }}

.. note::

    The system currently only supports one default per type.

**Arguments**:

- **snippetType**: *string* - The type for which to search for default snippets.
- **webspaceKey**: *string* - The optional webspace from which to get the default snippet
                                        settings.
- **locale**: *string* - The optional locale to load the snippet.

**Returns**:

An array of:

.. include:: _snippet_structure.inc
