Upgrading from Sulu 1.6 to 2.0
==============================

We provide an extensive `UPGRADE.md file`_ in the ``sulu/sulu`` repository detailing almost every breaking change
made during the development of Sulu 2.0. However, this is a long list and most projects will not be affected by the
majority of these breaking changes. Therefore, this guide provides a list of the most important changes. By following
the steps in this guide, you should be able to update most Sulu 1.6 projects to Sulu 2.0. Please note that this
guide does not cover cases where your project overrides Sulu's internal mechanisms.

**1. Create a new** `sulu/skeleton`_ **project and import your project's** ``.git`` **folder**

In our experience, it is more efficient and will save you future headaches if you start with a new project based on
`sulu/skeleton`_, which is the preferred project template for Sulu 2.0. The old `sulu/sulu-minimal`_
and the deprecated `sulu/sulu-standard`_ templates are not recommended for Sulu 2.0 projects. You
could restructure an existing `sulu/sulu-minimal`_ project to match the `sulu/skeleton`_ repository,
but that is typically more work than starting fresh.

The recommended approach is to create a new project using ``composer create-project`` and copy the old
``.git`` folder to the root of the new project.

.. code-block:: bash

    composer create-project sulu/skeleton <project-name>
    cp <old-project-path>/.git <project-name>

Afterward, a ``git status`` in the new folder should reveal all changes from your old repository to the new
`sulu/skeleton`_ installation, as Git is very effective at identifying file changes.

.. note::

    Theoretically, these changes could be committed now, but we recommend waiting until the end of this guide.
    This will result in a single commit containing the complete upgrade.

**2. Copy your old project files to the new project**

This is the most critical part of the upgrade. You must move all project-specific files from the old
project folder to the new one.

These files are not always located in the same place as before, because we have adapted to the folder structure
introduced by `Symfony Flex`_. Use the table below as a guide:

.. list-table::
    :header-rows: 1

    * - Old folder
      - New folder
      - Note
    * - ``src``
      - ``src``
      - This folder changed from the empty namespace to the ``App`` namespace. Update all classes accordingly.
    * - ``app/config``
      - ``config``
      - The structure changed due to Symfony Flex. Instead of ``admin`` and ``website`` subfolders, files are now
        suffixed with ``admin`` and ``website``.
    * - ``app/Resources/templates``
      - ``config/templates``
      -
    * - ``app/Resources/views``
      - ``templates``
      -
    * - ``app/Resources/webspaces``
      - ``config/webspaces``
      -

.. note::

    Please note that we have also updated several dependencies, which may include breaking changes. If your code
    no longer functions, it may be related to changes within these dependencies.

.. note::

    When copying configuration files, ensure you also copy the ``security.encoders`` values.
    If the new installation uses different values than the old one, you must update them;
    otherwise, you will be unable to log in. You will find this configuration in ``config/packages/security_admin.yaml`` in the
    new project and in ``app/config/admin/security.yml`` in the old project.

**3. Upgrade existing templates to use the new content types**

We have normalized content type names; therefore, you may need to update the ``types`` in your templates
located in ``config/templates``. The following table shows the name changes:

.. list-table::
    :header-rows: 1

    * - Old type
      - New type
    * - ``contact``
      - ``contact_account_selection``
    * - ``category_list``
      - ``category_selection``
    * - ``snippet``
      - ``snippet_selection``
    * - ``internal_links``
      - ``page_selection``
    * - ``single_internal_link``
      - ``single_page_selection``
    * - ``multiple_select``
      - ``select``
    * - ``tag_list``
      - ``tag_selection``

**4. Change the encoding of your MySQL database to utf8mb4**

.. note::

    You should create a backup of your database before executing the commands below.

The following commands change the charset to ``utf8mb4`` only for standard Sulu tables. If you have added
custom entities or tables, you must include additional statements for them. Refer to the guide on
`switching to utf8mb4`_ for more information.

.. note::

    These steps are only necessary for MySQL; PostgreSQL is not affected by this issue.

.. code-block:: sql

    ALTER DATABASE <database_name> CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
    USE <database_name>;
    ALTER TABLE me_format_options CHANGE format_key format_key VARCHAR(191) NOT NULL;
    ALTER TABLE me_collections CHANGE collection_key collection_key VARCHAR(191) DEFAULT NULL;
    ALTER TABLE me_collection_types CHANGE collection_type_key collection_type_key VARCHAR(191) DEFAULT NULL;
    ALTER TABLE me_media_types CHANGE name name VARCHAR(191) NOT NULL;
    ALTER TABLE me_file_versions CHANGE mimeType mimeType VARCHAR(191) DEFAULT NULL;
    ALTER TABLE se_users CHANGE email email VARCHAR(191) DEFAULT NULL;
    ALTER TABLE se_role_settings CHANGE settingKey settingKey VARCHAR(191) NOT NULL;
    ALTER TABLE se_permissions CHANGE context context VARCHAR(191) NOT NULL;
    ALTER TABLE se_access_controls CHANGE entityClass entityClass VARCHAR(191) NOT NULL;
    ALTER TABLE ca_categories CHANGE category_key category_key VARCHAR(191) DEFAULT NULL;
    ALTER TABLE ca_keywords CHANGE keyword keyword VARCHAR(191) NOT NULL;
    ALTER TABLE ta_tags CHANGE name name VARCHAR(191) NOT NULL;
    ALTER TABLE we_domains CHANGE url url VARCHAR(191) NOT NULL;
    ALTER TABLE we_analytics CHANGE webspace_key webspace_key VARCHAR(191) NOT NULL;
    ALTER TABLE ro_routes CHANGE path path VARCHAR(191) NOT NULL, CHANGE entity_class entity_class VARCHAR(191) NOT NULL, CHANGE entity_id entity_id VARCHAR(191) NOT NULL;
    ALTER TABLE me_collection_meta CHANGE title title VARCHAR(191) NOT NULL;
    ALTER TABLE me_file_version_meta CHANGE title title VARCHAR(191) NOT NULL;
    ALTER TABLE me_file_versions CHANGE name name VARCHAR(191) NOT NULL;
    ALTER TABLE ca_categories CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE ca_category_meta CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE ca_category_translations CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE ca_category_translations_keywords CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE ca_keywords CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE category_translation_media_interface CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_account_addresses CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_account_bank_accounts CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_account_categories CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_account_contacts CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_account_emails CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_account_faxes CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_account_medias CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_account_notes CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_account_phones CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_account_social_media_profiles CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_account_tags CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_account_urls CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_accounts CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_address_types CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_addresses CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_bank_account CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_contact_addresses CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_contact_bank_accounts CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_contact_categories CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_contact_emails CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_contact_faxes CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_contact_locales CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_contact_medias CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_contact_notes CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_contact_phones CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_contact_social_media_profiles CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_contact_tags CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_contact_titles CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_contact_urls CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_contacts CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_countries CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_email_types CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_emails CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_fax_types CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_faxes CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_notes CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_phone_types CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_phones CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_positions CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_social_media_profile_types CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_social_media_profiles CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_url_types CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE co_urls CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE me_collection_meta CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE me_collection_types CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE me_collections CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE me_file_version_categories CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE me_file_version_content_languages CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE me_file_version_meta CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE me_file_version_publish_languages CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE me_file_version_tags CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE me_file_versions CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE me_files CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE me_format_options CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE me_media CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE me_media_types CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE ro_routes CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE se_access_controls CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE se_group_roles CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE se_groups CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE se_permissions CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE se_role_settings CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE se_roles CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE se_security_types CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE se_user_groups CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE se_user_roles CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE se_user_settings CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE se_users CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE ta_tags CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE we_analytics CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE we_analytics_domains CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE we_domains CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

If you are using Jackalope with doctrine-dbal instead of Jackrabbit, you must also execute the following statements
to update the Jackalope tables:

.. code-block:: sql

    ALTER TABLE phpcr_binarydata CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE phpcr_internal_index_types CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE phpcr_namespaces CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE phpcr_nodes CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE phpcr_nodes_references CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE phpcr_nodes_weakreferences CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE phpcr_type_childs CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE phpcr_type_nodes CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE phpcr_type_props CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE phpcr_workspaces CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    ALTER TABLE phpcr_namespaces CHANGE prefix prefix VARCHAR(191) NOT NULL;
    ALTER TABLE phpcr_workspaces CHANGE name name VARCHAR(191) NOT NULL;
    ALTER TABLE phpcr_nodes CHANGE path path VARCHAR(191) NOT NULL, CHANGE parent parent VARCHAR(191) NOT NULL, CHANGE local_name local_name VARCHAR(191) NOT NULL, CHANGE namespace namespace VARCHAR(191) NOT NULL, CHANGE workspace_name workspace_name VARCHAR(191) NOT NULL, CHANGE identifier identifier VARCHAR(191) NOT NULL, CHANGE type type VARCHAR(191) NOT NULL;
    ALTER TABLE phpcr_internal_index_types CHANGE type type VARCHAR(191) NOT NULL;
    ALTER TABLE phpcr_binarydata CHANGE property_name property_name VARCHAR(191) NOT NULL, CHANGE workspace_name workspace_name VARCHAR(191) NOT NULL;
    ALTER TABLE phpcr_nodes_references CHANGE source_property_name source_property_name VARCHAR(191) NOT NULL;
    ALTER TABLE phpcr_nodes_weakreferences CHANGE source_property_name source_property_name VARCHAR(191) NOT NULL;
    ALTER TABLE phpcr_type_nodes CHANGE name name VARCHAR(191) NOT NULL;
    ALTER TABLE phpcr_type_props CHANGE name name VARCHAR(191) NOT NULL;

**5. Execute the following SQL statements to migrate your data**

In Sulu 2.0, we slightly adjusted our database schema. Therefore, you must execute the following statements to synchronize
your database schema:

.. code-block:: sql

    ALTER TABLE co_accounts ADD note LONGTEXT DEFAULT NULL;
    ALTER TABLE co_contacts ADD note LONGTEXT DEFAULT NULL;
    CREATE TABLE ca_category_translation_keywords (idKeywords INT NOT NULL, idCategoryTranslations INT NOT NULL, INDEX IDX_D15FBE37F9FC9F05 (idKeywords), INDEX IDX_D15FBE3717CA14DA (idCategoryTranslations), PRIMARY KEY(idKeywords, idCategoryTranslations)) DEFAULT CHARACTER SET UTF8 COLLATE UTF8_unicode_ci ENGINE = InnoDB;
    ALTER TABLE ca_category_translation_keywords ADD CONSTRAINT FK_D15FBE37F9FC9F05 FOREIGN KEY (idKeywords) REFERENCES ca_keywords (id);
    ALTER TABLE ca_category_translation_keywords ADD CONSTRAINT FK_D15FBE3717CA14DA FOREIGN KEY (idCategoryTranslations) REFERENCES ca_category_translations (id);
    CREATE TABLE ca_category_translation_medias (idCategoryTranslations INT NOT NULL, idMedia INT NOT NULL, INDEX IDX_39FC41BA17CA14DA (idCategoryTranslations), INDEX IDX_39FC41BA7DE8E211 (idMedia), PRIMARY KEY(idCategoryTranslations, idMedia)) DEFAULT CHARACTER SET UTF8 COLLATE UTF8_unicode_ci ENGINE = InnoDB;
    ALTER TABLE ca_category_translation_medias ADD CONSTRAINT FK_39FC41BA17CA14DA FOREIGN KEY (idCategoryTranslations) REFERENCES ca_category_translations (id) ON DELETE CASCADE;
    ALTER TABLE ca_category_translation_medias ADD CONSTRAINT FK_39FC41BA7DE8E211 FOREIGN KEY (idMedia) REFERENCES me_media (id) ON DELETE CASCADE;
    INSERT INTO ca_category_translation_keywords (idKeywords, idCategoryTranslations) SELECT keyword_id, category_meta_id FROM ca_category_translations_keywords;
    DROP TABLE ca_category_translations_keywords;
    INSERT INTO ca_category_translation_medias (idCategoryTranslations, idMedia) SELECT category_translation_id, media_interface_id FROM category_translation_media_interface;
    DROP TABLE category_translation_media_interface;
    UPDATE co_phone_types SET name="sulu_contact.work" WHERE name="phone.work";
    UPDATE co_phone_types SET name="sulu_contact.private" WHERE name="phone.home";
    UPDATE co_phone_types SET name="sulu_contact.mobile" WHERE name="phone.mobile";
    UPDATE co_email_types SET name="sulu_contact.work" WHERE name="email.work";
    UPDATE co_email_types SET name="sulu_contact.private" WHERE name="email.home";
    UPDATE co_address_types SET name="sulu_contact.work" WHERE name="address.work";
    UPDATE co_address_types SET name="sulu_contact.private" WHERE name="address.home";
    UPDATE co_url_types SET name="sulu_contact.work" WHERE name="url.work";
    UPDATE co_url_types SET name="sulu_contact.private" WHERE name="url.private";
    UPDATE co_fax_types SET name="sulu_contact.work" WHERE name="fax.work";
    UPDATE co_fax_types SET name="sulu_contact.private" WHERE name="fax.home";
    UPDATE we_analytics SET type="matomo" WHERE type="piwik";
    ALTER TABLE `se_roles` CHANGE `system` `securitySystem` VARCHAR(60) NOT NULL;
    UPDATE `se_users` SET `locale` = 'en' WHERE `locale` NOT IN ('en', 'de');
    ALTER TABLE co_addresses ADD countryCode VARCHAR(5) DEFAULT NULL;
    UPDATE co_addresses INNER JOIN co_countries ON co_addresses.idCountries = co_countries.id SET co_addresses.countryCode = co_countries.code, co_addresses.idCountries = NULL WHERE co_addresses.idCountries IS NOT NULL;
    ALTER TABLE co_addresses DROP FOREIGN KEY FK_26E9A614A18CC0FB;
    DROP INDEX IDX_26E9A614A18CC0FB ON co_addresses;
    ALTER TABLE co_addresses DROP idCountries;
    DROP TABLE co_countries;
    UPDATE se_permissions SET context = REPLACE(context, 'sulu.webspace_settings.', 'sulu.webspace.') WHERE context LIKE 'sulu.webspace_settings.%';

**6. Execute PHPCR migrations**

We have provided migrations for data stored in PHPCR. To update your data, execute the following command:

.. code-block:: bash

    php bin/console phpcr:migrations:migrate

.. _UPGRADE.md file: https://github.com/sulu/sulu/blob/2.0.12/UPGRADE.md
.. _sulu/skeleton: https://github.com/sulu/skeleton
.. _sulu/sulu-minimal: https://github.com/sulu/sulu-minimal
.. _sulu/sulu-standard: https://github.com/sulu/sulu-standard
.. _Symfony Flex: https://symfony.com/doc/current/setup/flex.html
.. _switching to utf8mb4: https://mathiasbynens.be/notes/mysql-utf8mb4
