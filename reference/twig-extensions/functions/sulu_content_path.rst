``sulu_content_path``
=====================

Returns the absolute URL for the content at the given path. The domain
is taken from `config/webspaces/*.xml` and your current
environment. In case you have multiple URLs in one environment, you can
prioritize one by giving it `<url main="true">`.

.. code-block:: jinja

    <ul class="nav nav-justified">
        {% for item in content.snippets[0].internalLinks %}
            <li>
                <a href="{{ sulu_content_path(item.url, item.webspaceKey) }}" title="{{ item.title }}">{{ item.title }}</a>
            </li>
        {% endfor %}
    </ul>

**Arguments**:

- **url**: *string* - The URL to get the path.
- **webspaceKey**: *string* - If the item is not in the same webspace as the current
  content (**optional**).
- **locale**: *string* - If the item is not in the same locale as the current
  content (**optional**).
- **domain**: *string* - If a specific domain should be used to generate the URL
  (**optional**).
- **scheme**: *string* - If a different scheme (than the current scheme) should be
  used to generate the URL (**optional**).

**Returns**: *string* - The absolute URL.
