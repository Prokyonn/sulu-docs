Smart Content Providers
=======================

SmartContentProviders are used to load data for SmartContent. They return data filtered
by a configuration array. This array can be configured via an overlay in the
backend form.

This configuration array includes the following values:

.. list-table::
    :header-rows: 1

    * - Name
      - Description
    * - ``dataSource``
      - Additional constraint, such as a page "folder."
    * - ``tags``
      - Multiple selection of tags that an item should have.
    * - ``tagOperator``
      - Determines if the item must have any or all of the selected tags.
    * - ``categories``
      - Multiple selection of categories that an item should have.
    * - ``categoryOperator``
      - Determines if the item must have any or all of the selected categories.
    * - ``types``
      - Multiple selection of types (e.g., templates) that an item should have.

Tags (``websiteTags``) and Categories (``websiteCategories``) can also be "injected" via
GET parameters from the website. These can be handled separately from the
admin-selected filters. Different operators (``websiteTagsOperator`` and
``websiteCategoryOperator``) are also available.

Additional features that can be provided by a SmartContentProvider:

.. list-table::
    :header-rows: 1

    * - Name
      - Description
    * - ``presentAs``
      - A value used in the website for display options (e.g., one or two
        columns). These values can be freely configured by developers.
    * - ``page`` & ``pageSize``
      - Pagination of items.
    * - ``limit``
      - Maximum number of items to display across all pages or overall (if pagination is active).

How to Create a Custom SmartContentProvider?
--------------------------------------------

To create a custom SmartContentProvider, create a service that
implements the ``Sulu\Bundle\AdminBundle\SmartContent\SmartContentProviderInterface``.
This interface provides functions to resolve the configured filters and return
the matching entities. The provider also returns a configuration object to enable
or disable features.

Below are the steps to create a SmartContentProvider for an ``ExampleEntity`` using
the Builder pattern for configuration.

1. Repository
~~~~~~~~~~~~~

Your repository needs methods to query and count entities based on filters. A
typical implementation provides ``findByFilters`` and ``countByFilters`` methods:

.. code-block:: php

    <?php

    declare(strict_types=1);

    namespace App\Repository;

    use Doctrine\ORM\EntityRepository;

    class ExampleRepository extends EntityRepository
    {
        public function findByFilters(array $filters, array $sortBys, array $params = []): array
        {
            $queryBuilder = $this->createQueryBuilder('example');

            // Apply filters (tags, categories, types, etc.)
            if (isset($filters['tags']) && !empty($filters['tags'])) {
                $queryBuilder->join('example.tags', 'tag')
                    ->andWhere('tag.id IN (:tags)')
                    ->setParameter('tags', $filters['tags']);
            }

            // Apply sorting
            foreach ($sortBys as $sortBy) {
                $queryBuilder->addOrderBy('example.' . $sortBy['column'], $sortBy['direction']);
            }

            // Apply pagination
            if (isset($params['limit'])) {
                $queryBuilder->setMaxResults($params['limit']);
            }
            if (isset($params['offset'])) {
                $queryBuilder->setFirstResult($params['offset']);
            }

            return $queryBuilder->getQuery()->getResult();
        }

        public function countByFilters(array $filters): int
        {
            $queryBuilder = $this->createQueryBuilder('example')
                ->select('COUNT(example.id)');

            // Apply same filters as findByFilters
            if (isset($filters['tags']) && !empty($filters['tags'])) {
                $queryBuilder->join('example.tags', 'tag')
                    ->andWhere('tag.id IN (:tags)')
                    ->setParameter('tags', $filters['tags']);
            }

            return (int) $queryBuilder->getQuery()->getSingleScalarResult();
        }
    }

.. note::

    Creating a separate repository is optional. You can also implement the query
    logic directly in your SmartContentProvider's ``findFlatBy`` and ``countBy``
    methods.

2. SmartContentProvider
~~~~~~~~~~~~~~~~~~~~~~~

Create a SmartContentProvider by implementing the ``SmartContentProviderInterface``:

.. code-block:: php

    <?php

    declare(strict_types=1);

    namespace App\SmartContent;

    use Sulu\Bundle\AdminBundle\SmartContent\Configuration\Builder;
    use Sulu\Bundle\AdminBundle\SmartContent\Configuration\ProviderConfigurationInterface;
    use Sulu\Bundle\AdminBundle\SmartContent\SmartContentProviderInterface;
    use App\Repository\ExampleRepository;

    class ExampleSmartContentProvider implements SmartContentProviderInterface
    {
        public function __construct(
            private ExampleRepository $repository,
        ) {
        }

        public function getConfiguration(): ProviderConfigurationInterface
        {
            return Builder::create()
                ->enableTags()
                ->enableCategories()
                ->enableLimit()
                ->enablePagination()
                ->enableSorting([
                    ['column' => 'created', 'title' => 'sulu_admin.created'],
                    ['column' => 'title', 'title' => 'sulu_admin.title'],
                ])
                ->enableTypes([
                    ['type' => 'example-type-1', 'title' => 'app.example_type_1'],
                    ['type' => 'example-type-2', 'title' => 'app.example_type_2'],
                ])
                ->enableView('app.example_edit_form', ['id' => 'id'])
                ->getConfiguration();
        }

        public function countBy(array $filters, array $params = []): int
        {
            return $this->repository->countByFilters($filters);
        }

        public function findFlatBy(array $filters, array $sortBys, array $params = []): array
        {
            return $this->repository->findByFilters($filters, $sortBys, $params);
        }

        public function getType(): string
        {
            return 'examples';
        }

        public function getResourceLoaderKey(): string
        {
            return 'examples';
        }
    }

Configuration Builder Methods
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ``Builder`` class provides these methods to configure your provider:

.. list-table::
    :header-rows: 1

    * - Method
      - Description
    * - ``enableTags()``
      - Enables tag filtering functionality.
    * - ``enableCategories()``
      - Enables category filtering functionality.
    * - ``enableLimit()``
      - Allows limiting the output items to a specified number.
    * - ``enablePagination()``
      - Enables pagination and specifying items per page.
    * - ``enablePresentAs()``
      - Enables multiple options for the view. These options can be
        configured in the template configuration of the SmartContent property.
    * - ``enableSorting(array $sorting)``
      - Enables sorting functionality. The sorting options must be passed
        to this method.
    * - ``enableTypes(array $types)``
      - Enables type filtering functionality. The selectable types must be
        passed to this method.
    * - ``enableDatasource(string $resourceKey, string $listKey, string $adapter)``
      - Allows choosing a source for the request. This is useful in tree
        structures because it allows filtering, for example, for pages below a certain
        parent page.
    * - ``enableAudienceTargeting()``
      - Enables filtering through audience targeting.
    * - ``enableProperties(array $properties)``
      - Defines default property names to be exposed in HTML templates. Use this only
        if your provider returns ``ContentRichEntities``.
    * - ``enableView(string $view, array $resultToView)``
      - Defines which view the application should navigate to when
        clicking on a resulting item. The first parameter describes the view
        defined in an ``Admin`` class and the second parameter is a mapping from
        a JSON pointer. The mapping defines how the values of the clicked item
        should be sent to the view's path.

3. Service Definition
~~~~~~~~~~~~~~~~~~~~~

Register the provider as a service with the ``sulu.smart_content.data_provider`` tag:

.. code-block:: yaml

    # config/services.yaml
    services:
        App\SmartContent\ExampleSmartContentProvider:
            arguments:
                - '@App\Repository\ExampleRepository'
            tags:
                - { name: 'sulu.smart_content.data_provider', alias: 'examples' }

The ``alias`` must match the value returned by ``getType()``.

Afterward, you can use your new SmartContentProvider in your templates by setting
the ``provider`` parameter to ``examples``.
