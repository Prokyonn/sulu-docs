``sulu_breadcrumb``
===================

Returns the breadcrumb for a given node UUID.

**Example**:

.. code-block:: jinja

    {% for item in sulu_breadcrumb(uuid) %}
        <a href="{{ sulu_content_path(item.url) }}">{{ item.title }}</a>
    {% endfor %}

**Arguments**:

- **uuid**: *string* - The UUID of the page node for which to show the breadcrumb.

**Returns**:

An `array` containing:

- **id**: The ID of the page.
- **title**: The title of the page.
- **url**: The URL for the page.
- **nodeType**: The type of the node.
- **excerpt**: The excerpt.
