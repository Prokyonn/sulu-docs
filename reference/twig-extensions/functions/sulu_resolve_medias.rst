``sulu_resolve_medias``
=======================

Returns the resolved media with the needed properties for a given media array.

.. code-block:: jinja

    {% set medias = sulu_resolve_medias(contact.medias, 'de') %}
    {% for media in medias %}
        <img src="{{ media.thumbnails['100x100'] }}" title="{{ media.title }}" />
    {% endfor %}

**Arguments**:

- **media**: *object[]|int[]* - The media objects or media IDs.
- **locale**: *string* - The locale to resolve metadata.

**Returns**: *object[]* - An array of objects with all the needed properties, like `thumbnails`, `title`, `description`, and `url`.
