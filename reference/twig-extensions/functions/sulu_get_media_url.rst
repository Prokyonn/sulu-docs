``sulu_get_media_url``
======================

Returns the relative URL to the given media.

.. code-block:: jinja

    {% set url = sulu_get_media_url(media, 'inline') %}

**Configuration**:

The following configuration is optional and means that the default `dispositionType`
is `attachment` for each file, and only if the `mimeTypes` of a file match
`application/pdf` or `image/jpeg` is the `dispositionType` `inline`.

If the default `dispositionType` were `inline` and some files should be
`attachment`, then the configuration of `mime_types_attachment` should be
filled and `mime_types_inline` should be empty.

.. code-block:: yaml

  sulu_media:
    disposition_type:
      default: "attachment"
      mime_types_inline: ["application/pdf", "image/jpeg"]
      mime_types_attachment: []

**Arguments**:

- **media**: *object* - The media object.
- **dispositionType**: *string* - Overrides the default configuration (`inline` or `attachment`) **(optional)**.

**Returns**: *string* - The relative URL.
