---
title: "Translations / i18n"
description: i18n, language specific configurations and the translation files
redirect_from:
  - /documentation/developer/translations.html
  - /documentation/translations/
  - /v1/translations.html
---

Kimai can be localized to any language and is already translated to
{% include features/multi-language.md %}   

Please be invited to contribute to the Kimai translations  – we’re looking for translators and would appreciate your support!

You find the translations at [https://hosted.weblate.org/projects/kimai/](https://hosted.weblate.org/projects/kimai/) and can start translating right away.
You can also click the little image in the table below for your language. Thank you for your help 👍🏻
 
| Language               | Status                                                                                                                                    |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
{%- for locale in site.data.multi-language %}
| {{ locale[1] }}        | [![Translation status](https://hosted.weblate.org/widgets/kimai/{{ locale[0] }}/svg-badge.svg)](https://hosted.weblate.org/engage/kimai/{{ locale[0] }}/)           |
{%- endfor %}
{: .table }
 
All translations in Kimai are managed at Weblate and should be changed there exclusively! Do not chang ethe source files of Kimai.

If you want to change certain keys in your installation, you can use the [Translation plugin]({% link _store/keleo-translation-bundle.md %}) for that.

{% include alert.html type="info" alert="The following documentation is not meant for end-users or translators. It is a technical documentation for the folks working on the Kimai code." %}

## Language files

We try to separate translations in logical units, in order to make it easier to identify the location of application messages.

- If you add a new key, you have to add it at least in the `en` version (as english is the fallback language for all missing keys in any language)
- It's very likely that you want to edit the file `messages` as it holds the most important application translations 

The files in `translations/` as a quick overview:

- `about` - the about screen with license information
- `actions` - the "action" dropdowns in all data-tables
- `email` - contents for the system emails generated by Kimai (eg. user registration and password reset)
- `daterangepicker` - the date range picker dialog to choose a timeframe in screens with data-tables
- `exceptions` - error pages and exception handlers
- `flashmessages` - success and error messages (alerts), which will be shown after submitting data
- `invoice-calculator` - invoice calculator types (see `Adding invoice calculator` in [developers]({% link _documentation/developers.md %})-section)
- `invoice-numbergenerator` - invoice calculator (see `Adding invoice-number generator ` in [developers]({% link _documentation/developers.md %})-section)
- `invoice-renderer` - holds translations of all invoice templates ([read more]({% link _documentation/invoices.md %}))
- `messages` - most of the visible application translations (like menu, buttons and forms)
- `plugins` - the plugin screen
- `system-configuration` - all system configuration, which can be changed through the UI
- `tags` - the tags administration screen
- `teams` - the team administration screen
- `validators` - related to violations/validation of submitted form data (or API calls)

{% capture cache %}
If you apply changes to any files mentioned on this page, you have to [clear the cache]({% link _documentation/cache.md %}).
{% endcapture %}
{% assign cache = cache|markdownify %}
{% include alert.html type="danger" alert=cache %}

## Authentication screens

The authentication screens (login, registration, register account) are translated through the theme bundle which is used in Kimai.
The bundle can be [found here](https://github.com/kevinpapst/TablerBundle) and the translations [in this directory](https://github.com/kevinpapst/TablerBundle/tree/main/translations).

When you create a new translation, please open a Pull Request in this repository as well.

## Adding a new language

This example assumes you are creating the (not existing) locale `xx`. 

Copy each translation from it's english version `translations/*.en.xlf` and rename them to `translations/*.xx.xlf`.

Adjust the `target-language` attributes in the file header, as example for the new file `exceptions.xx.xlf`:
```yml
<file source-language="en" target-language="xx" datatype="plaintext" original="exceptions.en.xlf">`
```

## Adding a language variant

For a language variant `xx_YY`, the fallback will always be the base language `xx` (eg. `de` for `de_CH`). 

Only some specific keys may need to be changed for this variant, and it's possible to add only the respective files like i.e. `translations/messages.de_CH.xlf` including only the changed translations:

```xml
<?xml version="1.0" encoding="utf-8"?>
<xliff version="1.2" xmlns="urn:oasis:names:tc:xliff:document:1.2">
    <file source-language="de" target-language="de-CH" datatype="plaintext" original="messages.en.xlf">
        <body>
            <trans-unit resname="action.close">
                <source>action.close</source>
                <target>Schliessen</target>
            </trans-unit>
        </body>
    </file>
</xliff>
```

## Configure locale formats

Locale formats are derived from the file `config/locales.php` which is auto-generated with the command:
 
```bash
bin/console kimai:reset:locales
```

## Register locale

You need to activate a new locale (so it can be used in routing) in the file `config/services.yaml` at `parameters.app_locales` divided by a pipe:

```yaml
parameters:
    locale: en
    app_locales: en|de|fr|ru|vi|zh_CN
```

## Number formats

The number formats are determined from the user locale.

## Date and time formats

The date and time formats are determined from the user locale.

There are configurations in place to convert between javascript components (e.g. the date-picker) and the PHP backend. 

## AM/PM format

Whether Kimai displays data in 24 hour or AM/PM format depends on the user locale.

## Generate correct ID and resname

Run the following command, which will generate the correct XLIFF attributes for you:

```bash
bin/console kimai:translation --resname
```

## Validate your changes

This will validate if the technical changes are okay / if the changed and new files can be used by Kimai:

```
bin/console lint:xliff translations
```

## Check for missing translations

You can search for missing keys by issuing this command (replace `xx` with your locale):
```bash
bin/console debug:translation --only-missing de
```

or
```
bin/console translation:update --dump-messages --force de
```
