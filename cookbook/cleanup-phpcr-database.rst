Cleaning up the PHPCR Database
==============================

If you have an older installation of Sulu, you may be dealing with a database cluttered with outdated PHPCR properties.
This is particularly common if you have made significant changes to your templates or content types over time.
To tidy up your database, run the following command:

.. code-block:: bash

    php bin/console sulu:document:phpcr-cleanup

The command is quite powerful, so exercise caution when using it in a production environment.
It will remove all properties that are not currently used in your templates. To avoid potential pitfalls, ensure you have a
backup of your database in place before executing this command.
