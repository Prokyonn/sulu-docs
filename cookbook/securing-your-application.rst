Securing Your Application
=======================

Sulu includes two primary ways to protect parts of your application. The first is permission-based security via security contexts, which
allows you to restrict access to entire sections of your application or Sulu. These
permissions are managed at the role level. Additionally, the locales for which these permissions are valid must be
defined when assigning the role to a user.

The second way is to protect access on a per-object basis. These
permissions are set on specific objects. The user must still have the
correct locales assigned to gain access.

This tutorial demonstrates how to use Sulu's security functionality with your custom
application code.

Protecting Content Using a Security Context
-------------------------------------------

This section describes how to protect an entire part of your application (rather than
a specific object).

Defining Your Security Context
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

First, you must define the security context, which is represented by a
simple string. This is done in the ``Admin`` class of your bundle:

.. code-block:: php

    <?php

    namespace Acme\Bundle\ExampleBundle\Admin;

    use Sulu\Bundle\AdminBundle\Admin\Admin;
    use Sulu\Component\Security\Authorization\PermissionTypes;

    class AcmeExampleAdmin extends Admin
    {
        // ...

        public function getSecurityContexts(): array
        {
            return [
                self::SULU_ADMIN_SECURITY_SYSTEM => [
                    'Acme' => [
                        'sulu.acme.example' => [
                            PermissionTypes::VIEW,
                            PermissionTypes::ADD,
                            PermissionTypes::EDIT,
                            PermissionTypes::DELETE,
                        ],
                    ],
                ],
            ];
        }

        // ...
    }

This information is defined in the ``getSecurityContexts`` method, which should
return an array. The first level identifies the system to which the security
context applies—this would either be Sulu (for administration)
or a custom context that you have defined.

The second level defines the title for another category used in the
administration interface. The third level defines the name of the permissions
themselves. This name should follow a namespacing scheme based on previously used
names. This value is the key for an array containing all available
permission types for this security context.

.. note::

    Since the ``Admin`` class is registered as a service, you can utilize
    other services to define available security contexts. For example,
    the SuluPageBundle uses a service to create a security context for
    all available webspaces in the system.

Protecting Your Controller
~~~~~~~~~~~~~~~~~~~~~~~~~~

After defining a security context, you can use it to protect the actions
of your controllers. Implement the
``SecuredControllerInterface`` to tell the ``SuluSecurityListener`` which
security context and locale to use for the permission check:

.. code-block:: php

    <?php

    namespace Acme\Bundle\ExampleBundle\Controller;

    use FOS\RestBundle\Routing\ClassResourceInterface;
    use Sulu\Component\Security\SecuredControllerInterface;
    use Symfony\Component\HttpFoundation\Request;

    class ExampleController implements ClassResourceInterface, SecuredControllerInterface
    {
        public function cgetAction()
        {
            // code for your get action
        }

        public function postAction()
        {
            // code for your post action
        }

        // ...

        public function getLocale(Request $request)
        {
            return $request->get('locale');
        }

        public function getSecurityContext(): string
        {
            return 'sulu.acme.example';
        }
    }

The ``getLocale`` method returns the locale, which is typically determined
from the request, and the ``getSecurityContext`` method defines which
security context is required to access this type of resource.

The ``SuluSecurityListener`` automatically identifies which type of
permission (``view``, ``add``, ``edit``, ``delete``, ...) is required,
performs the check, and returns a page with a
status code of ``403`` if the user's permissions
are insufficient.

Protecting Specific Objects
---------------------------

For some parts of your application, you may want to protect specific objects.
This section describes how to achieve this using Sulu's built-in features.

Adding the Permission Tab to Your Form
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

First, you must add the permission tab to your form to allow users
to configure permissions. The permission tab displays a list of
available user roles and permission icons that can be selected.

To do this, update the ``Admin`` class for your business objects:

.. code-block:: php

    <?php

    namespace Sulu\Bundle\ExampleBundle\Admin;

    use Sulu\Bundle\AdminBundle\Admin\Admin;
    use Sulu\Bundle\AdminBundle\Admin\View\ToolbarAction;
    use Sulu\Bundle\AdminBundle\Admin\View\ViewBuilderFactoryInterface;
    use Sulu\Bundle\AdminBundle\Admin\View\ViewCollection;

    class ExampleAdmin extends Admin
    {
        public function __construct(private ViewBuilderFactoryInterface $viewBuilderFactory)
        {
        }

        public function configureViews(ViewCollection $viewCollection): void
        {
            // ...
            $viewCollection->add(
                $this->viewBuilderFactory
                    ->createFormViewBuilder('sulu_example.edit_form.permissions', '/permissions')
                    ->setResourceKey('permissions')
                    ->setFormKey('permission_details')
                    ->addRequestParameters(['resourceKey' => 'example'])
                    ->setTabCondition('_permissions.security')
                    ->setTabTitle('sulu_security.permissions')
                    ->addToolbarActions([new ToolbarAction('sulu_admin.save')])
                    ->setParent(static::EDIT_FORM_VIEW)
            );
        }
    }

The critical option here is the ``addRequestParameters`` call, which
defines which resource this permission form manages. For this to
work, the relationship between the ``resourceKey``, security context, and
security class must be configured:

.. code-block:: yaml

    resources:
        example:
            routes:
                list: 'get_examples'
                detail: 'get_example'
            security_context: 'sulu_admin.example'
            security_class: 'App\\Entity\\Example'

After adding this, the permission tab will be visible in the edit form.

Configuring the Controller
~~~~~~~~~~~~~~~~~~~~~~~~~~

Next, implement the ``SecuredObjectControllerInterface`` in the
controller handling the specific entities:

.. code-block:: php

    <?php

    namespace Acme\Bundle\ExampleBundle\Controller;

    use FOS\RestBundle\Routing\ClassResourceInterface;
    use Sulu\Component\Security\Authorization\AccessControl\SecuredObjectControllerInterface;
    use Sulu\Component\Security\SecuredControllerInterface;
    use Symfony\Component\HttpFoundation\Request;

    class ExampleController
        implements ClassResourceInterface, SecuredControllerInterface, SecuredObjectControllerInterface
    {
        public function cgetAction()
        {
            $listBuilder = $factory->create($this->container->getParameter('sulu.model.example.class'));
            $this->get('sulu_core.doctrine_rest_helper')->initializeListBuilder($listBuilder, $this->getFieldDescriptors());

            $listBuilder->setPermissionCheck($this->getUser(), PermissionTypes::VIEW);

            $listResponse = $listBuilder->execute();

            // Do something with $listResponse
        }

        public function postAction()
        {
            // code for your post action
        }

        // ...

        public function getLocale(Request $request)
        {
            return $request->get('locale');
        }

        public function getSecurityContext(): string
        {
            return 'sulu.acme.example';
        }

        public function getSecuredClass(): string
        {
            return Example::class;
        }

        public function getSecuredObjectId(Request $request)
        {
            return $request->get('id');
        }
    }

The ``SecuredObjectControllerInterface`` requires three methods. The
``getLocale`` method is identical to the one in ``SecuredControllerInterface``, and the
implementation can be shared. The ``getSecuredClass`` method must return the
same identifier for the object type used in the resources configuration.
Finally, ``getSecuredObjectId`` receives the request object and must return
the object ID.

The rest of the work is handled by the ``SuluSecurityListener`` in the same manner
as the security context checks.

Note that ``cgetAction`` requires special handling when using ``ListBuilder``. The ``ListBuilder`` includes a ``setPermissionCheck`` method that takes
a user and a permission. If provided, the query only returns rows
where the user has the specified permission.
