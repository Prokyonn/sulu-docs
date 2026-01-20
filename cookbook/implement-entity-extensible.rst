How to Implement an Extensible Entity?
=====================================

Sulu uses the PersistenceBundle to provide an easy way to replace or extend entities.
In this tutorial, we will implement our own extensible book entity.

1. Entity
---------

Our extensible entity is built upon two classes:

BookInterface
"""""""""""""
``(Sulu\Bundle\BookBundle\Entity\BookInterface, Interface)``

Defines the interface of our entity and is used as the type for variables of the entity.
Every entity that extends or replaces our book entity must implement this interface to ensure compatibility with
the rest of the system.

Book
""""
``(Sulu\Bundle\BookBundle\Entity\Book, implements BookInterface)``

Implements the book entity and serves as the base class for extending the entity.
This class is our default entity implementation and is mapped as a mapped-superclass in ``Book.orm.xml``.

.. note::

    To ensure full exchangeability, you must use ``BookInterface`` as the type for every variable,
    Doctrine relationship, and other usage of our book entity.

2. Repository
-------------

In addition to our entity classes, we need two repository classes to handle our entities:

BookRepositoryInterface
"""""""""""""""""""""""
``(Sulu\Bundle\BookBundle\Entity\BookRepositoryInterface, Interface, extends RepositoryInterface)``

Defines the interface of our repository and is used as the type for variables of the ``BookRepository``.
An interface for an extensible entity extends the ``RepositoryInterface`` of the PersistenceBundle.

The ``RepositoryInterface`` defines a ``createNew()`` method, which must be used to create new instances
of an entity instead of the constructor. Using the repository method for instance creation is necessary
to avoid creating instances of the wrong entity implementation when the implementation is changed.

BookRepository
""""""""""""""
``(Sulu\Bundle\BookBundle\Entity\BookRepository, implements BookRepositoryInterface, optionally extends EntityRepository)``

Implements the concrete repository for our entity. It is recommended that this class extends the
``EntityRepository`` class of the PersistenceBundle, which implements a dynamic ``createNew()`` method and 
always returns a new instance of the currently configured entity implementation.

.. note::

    To ensure full exchangeability, you must use ``BookRepositoryInterface`` as the type for every variable
    that holds an instance of our ``BookRepository``.

3. Configuration
----------------

Finally, we need to adjust three configuration files to register our entity as extensible.

After configuration, the PersistenceBundle will automatically set the following parameters/services in your container:

* ``sulu.model.book.class``: currently set entity implementation (Parameter)
* ``sulu.repository.book``: currently set repository implementation (Service)
* ``Sulu\Bundle\BookBundle\Entity\BookRepositoryInterface``: alias for the currently set repository implementation (Service)

``DependencyInjection/Configuration.php``
"""""""""""""""""""""""""""""""""""""""""

In the ``Configuration.php`` file, we set our default entity and repository implementation. These implementations are used
if no other bundle replaces or extends our entity.
We implemented the class ``Book`` as our default entity and the class ``BookRepository`` as our default repository,
so our configuration looks like the following code block:

.. code-block:: php

    <?php
    class Configuration implements ConfigurationInterface
    {
        public function getConfigTreeBuilder()
        {
            $treeBuilder = new TreeBuilder();
            $rootNode = $treeBuilder->root('sulu_book')
                (...)
                ->children()
                    ->arrayNode('objects')
                        ->addDefaultsIfNotSet()
                        ->children()
                            ->arrayNode('book')
                                ->addDefaultsIfNotSet()
                                ->children()
                                    ->scalarNode('model')->defaultValue('Sulu\Bundle\BookBundle\Entity\Book')->end()
                                    ->scalarNode('repository')->defaultValue('Sulu\Bundle\BookBundle\Entity\BookRepository')->end()
                                ->end()
                            ->end()
                        ->end()
                    ->end()
                ->end();

            return $treeBuilder;
        }
        (...)
    }

This results in the configuration path ``sulu_book.objects.book.model`` for the model class and
``sulu_book.objects.book.repository`` for the repository class.
These paths can be used to overwrite the implementations used.

``DependencyInjection/SuluBookExtension.php``
"""""""""""""""""""""""""""""""""""""""""""""

In the ``SuluBookExtension.php`` file, we must read the set configuration and define and map the respective services
to the container. Additionally, we add the repository interface as an alias for the configured repository implementation
to make the repository autowireable.
We use the ``configurePersistence()`` method of the ``PersistenceExtensionTrait`` class and
the ``addAliases()`` method of the ``ContainerBuilder`` to achieve this.
Therefore, your ``SuluBookExtension.php`` will look like this:

.. code-block:: php

    <?php
    class SuluBookExtension extends Extension
    {
        use PersistenceExtensionTrait;

        public function load(array $configs, ContainerBuilder $container)
        {
            $configuration = new Configuration();
            $config = $this->processConfiguration($configuration, $configs);
            (...)
            $this->configurePersistence($config['objects'], $container);
            $container->addAliases(
                [
                    'Sulu\Bundle\BookBundle\Entity\BookRepositoryInterface' => 'sulu.repository.book',
                ],
            );
        }
        (...)
    }

``SuluBookBundle.php``
""""""""""""""""""""""

In the ``SuluBookBundle.php`` file, we need to add a compiler pass to automatically resolve our interface to
the configured entity implementation when used in a Doctrine mapping.
To do this, we use the ``buildPersistence()`` method of the ``PersistenceBundleTrait`` class.
Afterward, your ``SuluBookBundle.php`` will look like this:

.. code-block:: php

    <?php
    class SuluBookBundle extends Bundle
    {
        use PersistenceBundleTrait;

        public function build(ContainerBuilder $container)
        {
            (...)
            $this->buildPersistence(
                [
                    'Sulu\Bundle\BookBundle\Entity\BookInterface' => 'sulu.model.book.class',
                ],
                $container
            );
        }
        (...)
    }
