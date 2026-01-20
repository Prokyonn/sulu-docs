TestBundle
==========

Writing automated tests for a Sulu project is similar to testing a standard
Symfony project. Refer to the Symfony `test documentation`_.


Sulu's Kernel Context
---------------------

Sulu adds an additional layer to the service container of Symfony. This is
the kernel context, which separates the *admin* area from the *website* area.

When writing integration or functional tests, keep in mind
that some services are only available in either context.

Integration Tests
-----------------

In integration tests, you may have dependencies on other services. If a service
is only available in the website context you will encounter issues because the
kernel is in the admin context by default.

Therefore Sulu provides an extended `KernelTestCase`_ to specify the kernel
context.

.. code-block:: php
    
    // tests/Integration/Service/NewsletterGeneratorTest.php
    namespace App\Tests\Integration\Service;
    
    use Sulu\Bundle\TestBundle\Testing\KernelTestCase;
    use Sulu\Component\HttpKernel\SuluKernel;
    
    class NewsletterGeneratorTest extends KernelTestCase
    {
        public function testSomething()
        {
            self::bootKernel([
                'sulu.context' => SuluKernel::CONTEXT_WEBSITE
            ]);

            // ...
        }
    }

This will boot the kernel in the website context giving you access to
its services.

Functional Tests
----------------

Functional tests test your application from a higher level. Instead of
testing single methods or algorithms, you execute functions from the user's perspective.

As with integration tests, you must boot the kernel in the website
context if you want to test controllers and services living in that context.

Use Sulu's `SuluTestCase`_ to create a client in the website context and define
your expectations for the called action.

.. code-block:: php
    
    // tests/Functional/Controller/Website/RegistrationControllerTest.php
    namespace App\Tests\Functional\Controller\Website;
    
    use Sulu\Bundle\TestBundle\Testing\SuluTestCase;
    use Sulu\Component\HttpKernel\SuluKernel;
    
    class RegistrationControllerTest extends SuluTestCase
    {
        public function testIndexAction(): void
        {
            $client = static::createWebsiteClient();
            $crawler = $client->request('GET', '/registration/');

            $this->assertResponseIsSuccessful();
            $this->assertSelectorTextContains('h1', 'Join our Community');

            // ...
        }
    }

Calling ``$client = static::createWebsiteClient()`` is equivalent to:
    
.. code-block:: php
    
    $client = static::createClient([
        'sulu.context' => SuluKernel::CONTEXT_WEBSITE,
    ]);

See the `DOM Crawler`_ documentation to learn how to make
assertions.

Logging in Users (Authentication)
---------------------------------

If you need a logged-in user to test secured actions, Sulu provides a test
user, which you can use.

.. code-block:: php
    
    class ArticleAdminControllerTest extends SuluTestCase
    {
        public function testIndexAction(): void
        {
            $client = static::createClient();
        
            $user = $this->getTestUser();
            $client->loginUser($user);
        }
    }

The user you get is an entity of type ``Sulu\Bundle\SecurityBundle\Entity\User``,
has the role *ROLE_USER* and is automatically granted access during authorization checks.

Database purging
----------------

If you manipulate the database in your tests and need a clean state
before running them, use Sulu's database purging helper.

.. code-block:: php
    
    class ActivityRepositoryTest extends SuluTestCase
    {
        public function setUp(): void
        {
            static::purgeDatabase();
        }
    }

This purges the database before each test defined in the test class.
To purge the database only once before your tests run, use it in ``setUpBeforeClass()`` instead of ``setUp()``.


.. _test documentation: https://symfony.com/doc/current/testing.html
.. _KernelTestCase: https://github.com/sulu/sulu/blob/2.5/src/Sulu/Bundle/TestBundle/Testing/KernelTestCase.php
.. _SuluTestCase: https://github.com/sulu/sulu/blob/2.5/src/Sulu/Bundle/TestBundle/Testing/SuluTestCase.php
.. _DOM Crawler: https://symfony.com/doc/current/testing/dom_crawler.html
