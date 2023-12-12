# Locale files for FOSSBilling

This repository contains the locale files for FOSSBilling. The files are in the [GNU gettext](http://www.gnu.org/software/gettext/) format.

## How to contribute

If you want to contribute to the translations, please join our [Crowdin project](https://translate.fossbilling.org) and use the web interface to translate the files. If you want to contribute to the translations but don't want to use Crowdin, you can also clone this repository and create a pull request with your changes. Please note that we will only accept pull requests for languages that are already available in Crowdin.

All contributors are expected to follow our [Code of Conduct](https://fossbilling.org/docs/contribution-handbook/code-of-conduct) and join our [Discord server](https://fossbilling.com/discord). If you have any questions, please ask them in the #localization channel.

## Adding a new language

To have a language added, please create a new issue in this repository or join our [Discord server](https://fossbilling.com/discord) and request your language in the `#localization` channel.

If we think think the language is relevant and will be used by a significant number of users, we will add it to Crowdin and you can start translating. A language will be considered relevant if there is an existing userbase that's using the language and willing to contribute to the translations in the long run.

## Maintenance

### Building the .MO files

The .MO files are automatically generated and pushed using GitHub Actions after each PR is merged. If you want to build the files manually, check what [the workflow](https://github.com/FOSSBilling/locale/blob/main/.github/workflows/generate-mo.yml) is doing and replicate it on your machine.

For convenience, you can just create a PR and the workflow will run automatically after it's merged.

### Updating the .POT file

1. Ensure you have the pro version of Poedit and that the twig extractor is enabled
2. Create a custom extractor with the following command:
   1. `xgettext -L PHP --keyword=__trans --keyword=__pluralTrans:1,2 --keyword=InformationException --keyword=Exception --keyword=Server_Exception --keyword=Registrar_Exception --keyword=Payment_Exception --add-comments=TRANSLATORS: --force-po -o %o %C %F`
3. Use Poedit's "Update from code" option to load the latest translations from the FOSSBilling source code.
   1. You may need to edit the project settings to correct the translation source location. Make sure to select the 'src' directory.
   2. You should also ensure that you either don't have the vendor folder, or that it is excluded from the source list when updating the translations for FOSSBilling.
4. Save the updated .pot file and commit it to the repository. Crowdin will automatically detect the changes and update all translations.
