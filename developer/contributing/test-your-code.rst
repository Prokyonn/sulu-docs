Testing Your Code
=================

If your tests require external dependencies (e.g., a database connection), they
are functional tests; otherwise, they are unit tests.

A key quality of unit tests is that they execute very quickly, whereas
functional tests tend to be slower.

Functional Tests
----------------

To run the tests, follow these steps:

1. Install Composer dependencies with ``composer install``.

2. Run the tests in one of the following ways.

Bundle Testing
~~~~~~~~~~~~~~

The test runner script is a PHP script that automates the execution of
**bundle** tests. It is used by the continuous integration server and is
useful for quickly running tests.

.. code-block:: bash

    $ ./bin/runtests -i -C

The command above initializes the database (``-i``) and runs all tests except
the component tests (``-C``).

``runtests`` has the following options:

   * ``-i``: Initialize the test setup (e.g., creating the database).
   * ``-t [Bundle]``: Run tests only for the specified bundle.
   * ``-a``: Run all tests.
   * ``-B``: Do not run bundle tests.
   * ``-C``: Do not run component tests.

Subsequently, you only need to run the tests, so you can omit the ``-i``
option.

.. code-block:: bash

    $ ./bin/runtests -a

You may also specify a specific bundle:

.. code-block:: bash

    $ ./bin/runtests -C -t SearchBundle

After the bundles have been initialized, you can also navigate to the
bundle root directory and use ``phpunit`` normally:

.. code-block:: bash

    $ cd src/Sulu/Bundle/SearchBundle
    $ phpunit

Component Testing
-----------------

Component tests may be executed using the ``runtests`` script or PHPUnit from
the root directory:

.. code-block:: bash

    $ ./bin/runtests -B
    $ phpunit

You can test a specific component with PHPUnit by specifying its path:

.. code-block:: bash

    $ phpunit src/Sulu/Component/Content

Jackrabbit Installation
-----------------------

By default, Sulu uses the Doctrine DBAL implementation for PHPCR in your local
test environment. If you need to test against the Jackrabbit backend, you can
install it with the following bash snippet:

.. code-block:: bash

    JACKRABBIT_VERSION=2.12.0
    if [ ! -f downloads/jackrabbit-standalone-$JACKRABBIT_VERSION.jar ]; then
        cd downloads
        wget http://archive.apache.org/dist/jackrabbit/$JACKRABBIT_VERSION/jackrabbit-standalone-$JACKRABBIT_VERSION.jar
        cd -
    fi

To start your Jackrabbit installation, run:

.. code-block:: bash

    java -jar downloads/jackrabbit-standalone-2.12.0.jar > /dev/null &

Now, run your tests with the ``jackrabbit`` backend enabled (omit the
initialization step [``-i``] after the first run):

.. code-block:: bash

    $ PHPCR_TRANSPORT=jackrabbit ./bin/runtests -i -a
