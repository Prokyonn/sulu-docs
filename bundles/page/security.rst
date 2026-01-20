Security
========

Every page has its own permission tab, integrated as described in
:doc:`../../cookbook/securing-your-application`. The permissions for the Sulu
system in this tab influence the UI of Sulu. The UI might also be
adapted due to the permissions defined in Security Contexts.

The form adapts its toolbar based on permissions. The delete option
in the edit dropdown is only available if the user has ``delete`` permission on
this page. The other toolbar items are only available if the user has
``edit`` permission.

The column navigation for content is slightly more complex. The entire
tree is always visible, but available functionality changes based on
the permissions of the page. The `delete` permission is tied to the delete
item in the option dropdown. The `edit` permission is required to move and sort
the page in the content tree. For sorting the page the `edit` permission for all
siblings and the parent page is also required.

For copying a page the user has to have the `view` permission for the source
page.

Additionally, the icon that appears when hovering over an item
in the content tree depends on its permissions. If the user is allowed to edit the page
a pencil shows up, in case the user has only the permission to view the page
an eye is shown instead. In case the user has no permission at all,
no icon will appear on hover.

In addition to the Sulu system, the webspace system for this page is also
shown on the permission tab, if the webspace has a configured system (see
:doc:`../../book/webspaces` for more information on how to do this). If
the webspace system is configured with the ``permission-check`` flag set to
``true``, Sulu will automatically check if the current visitor
is allowed to view the current page and only show pages in the navigation and
smart content listings if the visitor has sufficient permissions.
