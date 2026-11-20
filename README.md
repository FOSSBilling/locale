# Locale Files for FOSSBilling

This repository contains the locale files for FOSSBilling. The files are in the [GNU gettext](http://www.gnu.org/software/gettext/) format.

## How to Contribute

If you want to contribute to the translations, please join our [Crowdin project](https://translate.fossbilling.org) and use the web interface to translate the files.

All contributors are expected to follow our [Code of Conduct](https://fossbilling.org/docs/contribution-handbook/code-of-conduct). If you have any questions, please ask them in the #localization channel.

## Adding a New Language

To have a language added, please create a new issue in this repository or join our [Discord](https://fossbilling.com/discord) and request your language in the `#localization` channel.

If we think the language is relevant and will be used by a significant number of users, we will add it to Crowdin and you can start translating. A language will be considered relevant if there is an existing userbase that's using the language and willing to contribute to the translations in the long run.

## How Translations Flow

Translations live in Crowdin and are synced here automatically:

- A daily synchronization opens a pull request with the latest translations, which is auto-merged.
- Only translations that are at least 25% complete ship in releases. Less complete languages are listed in the release notes as excluded until they catch up.
- The `messages.pot` template is regenerated from the FOSSBilling source weekly (see Maintenance below), so new strings appear in Crowdin shortly after they land in the app.

## Core and Module Projects

The existing Crowdin project **4** will remain the core translation project, with the planned display name **FOSSBilling Core**. Its configuration stays in [`crowdin.yml`](./crowdin.yml); the project ID, translation URL and `messages.pot` file mapping remain unchanged.

The existing synchronization workflow uses Crowdin's `main` branch and GitHub's `l10n_main` branch. Preserve those identities and the existing integration setup when separating module translations.

Optional module translations, starting with Branding and Cookieconsent, will use a separate **FOSSBilling Modules** project configured in [`FOSSBilling/modules`](https://github.com/FOSSBilling/modules). Project creation, translation imports and activation of module synchronization are separate steps. The one-time translation migration will be carried out directly.

Keep the combined core template and translations until compatible module packages and their translation exports are verified. After the modules are removed from core, regenerate `messages.pot` and review the removed strings; strings still used by core must remain.

This preparation does not rename the live project or change the existing synchronization and release workflows.

## Release Tags

Translation releases are generated automatically and are tagged `pot-<sha256>` using the SHA-256 hash of the `messages.pot` file, allowing users to find and update to the most recent translations that match their FOSSBilling version. (The `pot-` prefix is required because GitHub rejects bare 40/64-character hex strings as tag names.) Each release carries a `translations.zip` asset alongside a per-locale completion report.

## Licensing

This repository carries two licenses, split by content type:

| Content | License | File |
|---|---|---|
| Code (`locales.php`, workflows) | Apache License 2.0 | `LICENSE.md` |
| Translations (`*.po`, `*.pot`, `*.mo`) | CC BY 4.0 — attribution to the FOSSBilling contributors via the [Crowdin project](https://translate.fossbilling.org) | `LICENSE.translations` |

Both license files ship inside every translation release.

## Maintenance

### Building the .MO Files

The .MO files are automatically generated and pushed using GitHub Actions after each PR is merged. If you want to build the files manually, check what [the workflow](https://github.com/FOSSBilling/locale/blob/main/.github/workflows/generate-mo.yml) is doing and replicate it on your machine.

### Updating the .POT file

`messages.pot` is regenerated automatically by the **Update Translation Template** workflow (`.github/workflows/update-pot.yml`), which runs weekly and on manual dispatch. It checks out `src` and `tools/i18n` from `FOSSBilling/FOSSBilling@main`, installs the standalone tooling dependencies, runs `tools/i18n/bin/console extract`, and opens a review PR here when the `msgid` set changed. The workflow never pushes to `main` directly.

Extraction rules and CLI commands are documented in the [translation tooling guide](https://github.com/FOSSBilling/FOSSBilling/blob/main/tools/i18n/README.md).

Deliberate policies:

- **Obsolete msgids are dropped.** Strings no longer present in source are removed from the `.pot`; Crowdin keeps them in translation memory, so no translator work is lost.
- **Msgids are matched exactly (case-sensitive) by gettext.** Casing-only changes (e.g. a Title-Case migration) invalidate existing translations for those strings until they are retranslated — the update PR body calls these out explicitly, so review it before merging.

If you change what counts as translatable in the FOSSBilling source (new helpers, new keywords), update the extractor script in the main repo alongside it. To regenerate the `.pot` outside the schedule, use Actions → *Update Translation Template* → *Run workflow* here, or run locally with PHP 8.5 (including tokenizer and mbstring) and Composer:

```bash
git clone --depth 1 https://github.com/FOSSBilling/FOSSBilling fossbilling
composer install --working-dir=fossbilling/tools/i18n --no-dev
fossbilling/tools/i18n/bin/console extract fossbilling/src messages.pot
```
