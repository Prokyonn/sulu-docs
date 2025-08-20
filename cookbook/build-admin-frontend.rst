Building the Administration Interface Front-End Application
===========================================================

The administration interface of Sulu is implemented as a single-page application using React. The code of this
application is built using Webpack and stored in the `public/build/admin` directory of the project.
If you update your Sulu version or add custom JavaScript code to your project, you need to update the build in
the `public/build/admin` directory. There are several ways to do this:

Solution 1: Update Command (Recommended)
----------------------------------------

Sulu is shipped with a built-in command to update the build.

1. Run the Update Build Command

.. code-block:: bash

    php bin/adminconsole sulu:admin:update-build

.. note::

    The update build command will download the build for your project from the `sulu/skeleton` repository if possible.
    If you have added custom JavaScript code to your project, the command will automatically clean up leftovers from
    previous builds and build the JavaScript code manually on your system. Make sure that you have installed Node.js if
    your project requires a manual build.

Solution 2: Build Manually with Docker
--------------------------------------

1. Start a Node container with the desired version and map the current directory into the `/var/project` folder in the container.

.. code-block:: bash

    docker run --rm --interactive --tty --volume ${PWD}:/var/project node:14.16.0 /bin/bash

    # For completion: using another Node.js version is possible by adjusting the tag of the Node image.
    # docker run --rm --interactive --tty --volume ${PWD}:/var/project node:12.21.0 /bin/bash

2. Clean up previously created `node_modules` folders and `package-lock.json` files

.. code-block:: bash

    cd /var/project
    rm -rf assets/admin/node_modules && rm -rf vendor/sulu/sulu/node_modules && rm -rf vendor/sulu/sulu/src/Sulu/Bundle/*/Resources/js/node_modules
    rm -rf assets/admin/package-lock.json && rm -rf vendor/sulu/sulu/package-lock.json && rm -rf vendor/sulu/sulu/src/Sulu/Bundle/*/Resources/js/package-lock.json

3. Create the administration interface build

.. code-block:: bash

    cd /var/project/assets/admin
    npm install
    npm run build

Solution 3: Build Manually Locally
----------------------------------

1. Install Node

If not yet installed on your computer, you will need to install Node.js.

2. Clean up previously created `node_modules` folders and `package-lock.json` files

.. code-block:: bash

    cd /var/project
    rm -rf assets/admin/node_modules && rm -rf vendor/sulu/sulu/node_modules && rm -rf vendor/sulu/sulu/src/Sulu/Bundle/*/Resources/js/node_modules
    rm -rf assets/admin/package-lock.json && rm -rf vendor/sulu/sulu/package-lock.json && rm -rf vendor/sulu/sulu/src/Sulu/Bundle/*/Resources/js/package-lock.json

3. Create the administration interface build

.. code-block:: bash

    cd /var/project/assets/admin
    npm install
    npm run build

Solution 4: Build Manually Locally with Bun
-------------------------------------------

As an alternative to Node.js/npm, Sulu also supports using Bun to build the administration interface.
Support for Bun is experimental and may be removed in future versions of Sulu.

1. Install Bun

If not yet installed on your computer, you will need to install Bun.

2. Clean up previously created `node_modules` folders and `bun.lockb` files

.. code-block:: bash

    cd /var/project
    rm -rf assets/admin/node_modules && rm -rf vendor/sulu/sulu/node_modules && rm -rf vendor/sulu/sulu/src/Sulu/Bundle/*/Resources/js/node_modules
    rm -rf assets/admin/bun.lockb && rm -rf vendor/sulu/sulu/bun.lockb && rm -rf vendor/sulu/sulu/src/Sulu/Bundle/*/Resources/js/bun.lockb

3. Create the administration interface build

.. code-block:: bash

    cd /var/project/assets/admin
    bun run preinstall
    bun install
    bun run build

Common Errors
-------------

If the installation of the npm dependencies or the Webpack build fails, you might want to try the following things:

1. Check Your Node.js and npm Version

You can check the officially supported and tested Node.js and npm versions by looking at the `Test Application workflow`_ of the `sulu/sulu` package.
At the time of writing, this includes Node.js 12, Node.js 14, and npm 6.

.. warning::

    Because of a breaking change for linked packages, Sulu is not compatible with npm v7 at the moment. Have a look at the `issue in the sulu/skeleton repository`_ for more information about this.

2. Clear the npm Cache on Your Machine

The Webpack build might fail because of leftovers from previous builds or outdated packages.
To prevent this, you should remove all the `package-lock.json` files and `node_modules` directories in your project root before installing the npm dependencies:

.. code-block:: bash

   rm -rf assets/admin/node_modules && rm -rf vendor/sulu/sulu/node_modules && rm -rf vendor/sulu/sulu/src/Sulu/Bundle/*/Resources/js/node_modules
   rm -rf assets/admin/package-lock.json && rm -rf vendor/sulu/sulu/package-lock.json && rm -rf vendor/sulu/sulu/src/Sulu/Bundle/*/Resources/js/package-lock.json

If this does not solve the problem, you can try to clean the npm cache on your machine to prevent installing cached packages:

.. code-block:: bash

    npm cache clean --force

.. _issue in the sulu/skeleton repository: https://github.com/sulu/skeleton/issues/88
.. _Test Application workflow: https://github.com/sulu/sulu/blob/2.x/.github/workflows/test-application.yaml
.. _sulu/skeleton repository: https://github.com/sulu/skeleton
.. _node: https://nodejs.org/en/
.. _bun: https://bun.sh/
