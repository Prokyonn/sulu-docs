``sulu_resolve_media``
======================

Returns the resolved media with the needed properties for a given media object.

.. code-block:: jinja

    {% set media = sulu_resolve_media(contact.avatar, 'de') %}
    <img src="{{ media.thumbnails['100x100'] }}" title="{{ media.title }}" />

**Arguments**:

- **media**: *object|int* - The media object or media ID.
- **locale**: *string* - The locale to resolve metadata.

**Returns**: *object* - An object with all the needed properties, like `thumbnails`,
                        `title`, `description`, and `url`.
