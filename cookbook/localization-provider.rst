Adding Localizations with the Localization Provider
==================================================

If you create a bundle with its own localizations, they should be registered within the Sulu system.
This ensures you can use Sulu's security features and other locale-dependent functionalities.

You can add locales by simply defining a service using the ``LocalizationProvider`` and passing your custom
locales as arguments.

Example
-------

.. code-block:: xml

    <service id="sulu_product.localization_provider" class="Sulu\Component\Localization\Provider\LocalizationProvider">
        <argument>%sulu_product.locales%</argument>

        <tag name="sulu.localization_provider"/>
    </service>
