Security
========

Separate permissions can be applied to every collection. These permissions also
apply to the assets within the collection.

In the Sulu system (representing the administration interface), the `view`
permission is required to view the collection itself in the navigation and its
assets in any available view in the system. The `add` permission is needed to
upload new assets and add new subcollections to a collection. The `edit`
permission allows the user to edit the title and other attributes of the
collections and its containing assets, and the `delete` permission is
required to delete assets and entire collections.

The permissions for collections will also show the systems for each webspace if
configured with the ``permission-check`` flag set to ``true`` (see
:doc:`../../book/webspaces` for more information on how to do this). Then Sulu
will only show the media on this website (regardless of whether it is displayed via smart content or media selection) if the current visitor has the `view`
permission for this media.
