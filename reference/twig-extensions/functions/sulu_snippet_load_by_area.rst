``sulu_snippet_load_by_area``
=============================

Returns the content of the default snippet for the given :doc:`snippet area <../../../cookbook/default-snippets>`.

.. code-block:: jinja

    {% set snippets = sulu_snippet_load_by_area('sidebar_overview') %}
    {{ snippets.content.title }}

**Arguments**:

- **area**: *string* - The area in which to search for the snippet.
- **webspaceKey**: *string* - The optional webspace from which to get area snippet settings.
- **locale**: *string* - The optional locale to load the snippet.

**Returns**:

.. include:: _snippet_structure.inc
