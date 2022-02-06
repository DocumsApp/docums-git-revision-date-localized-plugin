# Customize a theme

You can [customize an existing theme](https://khanhduy1407.github.io/docums/user-guide/styling-your-docs/#customizing-a-theme) by overriding blocks or partials. You might want to do this because your theme is not natively supported, or you would like more control on the formatting. Below are two examples to help get you started.

## Example: default `docums` theme

To add a revision date to the default `docums` theme, add a `overrides/partials` folder to your `docs` folder and update your `docums.yml` file. 
Then you can extend the base `docums` theme by adding a new file `docs/overrides/content.html`:

=== ":octicons-file-code-16: docums.yml"

    ```yaml
    theme:
        name: docums
        custom_dir: docs/overrides
    ```

=== ":octicons-file-code-16: docs/overrides/content.html"

    ```html
    <!-- Overwrites content.html base docums theme, taken from 
    https://github.com/khanhduy1407/docums/blob/master/docums/themes/docums/content.html -->

    {% if page.meta.source %}
        <div class="source-links">
        {% for filename in page.meta.source %}
            <span class="label label-primary">{{ filename }}</span>
        {% endfor %}
        </div>
    {% endif %}

    {{ page.content }}

    <!-- This section adds support for localized revision dates -->
    {% if page.meta.git_revision_date_localized %}
        <small>Last update: {{ page.meta.git_revision_date_localized }}</small>
    {% endif %}
    {% if page.meta.git_created_date_localized %}
        <small>Created: {{ page.meta.git_created_date_localized }}</small>
    {% endif %}
    ```

## Example: `docums-material` theme

[docurial](https://khanhduy1407.github.io/docurial/) has built-in support for `git_revision_date_localized` and `git_created_date_localized`. You can see that when viewing their [`source-file.html`](https://github.com/khanhduy1407/docurial/blob/master/src/partials/source-file.html) partial. 

If you want, you can customize further by [extending the docurial theme](https://khanhduy1407.github.io/docurial/customization/#extending-the-theme) and overriding the `source-file.html` partial as follows:

=== ":octicons-file-code-16: docums.yml"

    ```yaml
    theme:
        name: 'material'
        custom_dir: docs/overrides
    ```

=== ":octicons-file-code-16: docs/overrides/partials/source-file.html"

    ```html
    {% import "partials/language.html" as lang with context %}

    <!-- taken from 
    https://github.com/khanhduy1407/docurial/blob/master/src/partials/source-file.html -->
    
    <hr />
    <div class="md-source-file">
    <small>

        <!-- docums-git-revision-date-localized-plugin -->
        {% if page.meta.git_revision_date_localized %}
        {{ lang.t("source.file.date.updated") }}:
        {{ page.meta.git_revision_date_localized }}
        {% if page.meta.git_creation_date_localized %}
            <br />
            {{ lang.t("source.file.date.created") }}:
            {{ page.meta.git_creation_date_localized }}
        {% endif %}

        <!-- docums-git-revision-date-plugin -->
        {% elif page.meta.revision_date %}
        {{ lang.t("source.file.date.updated") }}:
        {{ page.meta.revision_date }}
        {% endif %}
    </small>
    </div>
    ```

