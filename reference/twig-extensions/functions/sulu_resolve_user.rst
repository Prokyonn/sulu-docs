``sulu_resolve_user``
=====================

Returns a user entity.

.. code-block:: jinja

    {{ sulu_resolve_user(changer).contact.fullName }}

**Arguments**:

- **id**: *int* - The ID of the requested user.

**Returns**: `User` - An object with all the needed properties.
