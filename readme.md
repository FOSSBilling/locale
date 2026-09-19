# Locale files for FOSSBilling

This repository contains the locale files for FOSSBilling. The files are in the [GNU gettext](http://www.gnu.org/software/gettext/) format.

## How to contribute

If you want to contribute to the translations, please join our [Crowdin project](https://translate.fossbilling.org) and use the web interface to translate the files. If you want to contribute to the translations but don't want to use Crowdin, you can also clone this repository and create a pull request with your changes. Please note that we will only accept pull requests for languages that are already available in Crowdin.

All contributors are expected to follow our [Code of Conduct](https://fossbilling.org/docs/contribution-handbook/code-of-conduct) and join our [Discord server](https://fossbilling.com/discord). If you have any questions, please ask them in the #localization channel.

## Adding a new language

To have a language added, please create a new issue in this repository or join our [Discord server](https://fossbilling.com/discord) and request your language in the `#localization` channel.

If we think think the language is relevant and will be used by a significant number of users, we will add it to Crowdin and you can start translating. A language will be considered relevant if there is an existing userbase that's using the language and willing to contribute to the translations in the long run.

## Release Tags

Translation releases are generated automatically and are tagged using the MD5 hash of the `messages.pot` file, allowing users to find and update to the most recent translations that match their FOSSBilling version.

## Maintenance

### Building the .MO files

The .MO files are automatically generated and pushed using GitHub Actions after each PR is merged. If you want to build the files manually, check what [the workflow](https://github.com/FOSSBilling/locale/blob/main/.github/workflows/generate-mo.yml) is doing and replicate it on your machine.

For convenience, you can just create a PR and the workflow will run automatically after it's merged.

### Updating the .POT file

`messages.pot` is regenerated automatically by the **Update translation template**
workflow (`.github/workflows/update-pot.yml`), which runs weekly and on manual
dispatch. It checks out `FOSSBilling/FOSSBilling@main:src`, runs the open-source
extractor at `FOSSBilling/.github/scripts/extract_pot.py`, and opens a review PR
here when the msgid set changed. The workflow never pushes to `main` directly.

What the extractor covers (mirroring the historical Poedit runs):

- Twig templates: string literals passed to the `|trans` filter.
- PHP sources: first-arg literals of `__trans()`, `__pluralTrans()` (args 1+2)
  and the exception keywords (`Exception`, `InformationException`,
  `Server_Exception`, `Registrar_Exception`, `Payment_Exception`),
  including `new X('...')` — but skipping calls whose message is built at
  runtime (variables, `sprintf()`, concatenation, `$"..."` interpolation).
- Scope: `src/` minus `vendor/`, `install/`, `data/`, `load.php`, `*/tests/*`
  and the `*.js` / `*.html` / `*.css` / `*.scss` / `*.md` extensions.

Deliberate policies:

- **Obsolete msgids are dropped.** Strings no longer present in source are
  removed from the `.pot`; Crowdin keeps them in translation memory, so no
  translator work is lost.
- **Msgids are matched exactly (case-sensitive) by gettext.** Casing-only
  changes (e.g. a Title-Case migration) invalidate existing translations for
  those strings until they are retranslated — the update PR body calls these
  out explicitly, so review it before merging.

If you change what counts as translatable in the FOSSBilling source (new
helpers, new keywords), update the extractor script in the main repo alongside
it. To regenerate the `.pot` outside the schedule, use
Actions → *Update translation template* → *Run workflow* here, or run locally:

```bash
git clone --depth 1 https://github.com/FOSSBilling/FOSSBilling fossbilling
python3 fossbilling/.github/scripts/extract_pot.py fossbilling/src messages.pot
```
