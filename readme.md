# Locale files for FOSSBilling
This repository contains the locale files for FOSSBilling. The files are in the [GNU gettext](http://www.gnu.org/software/gettext/) format.

## How to contribute
If you want to contribute to the translations, please join our [Crowdin project](https://translate.fossbilling.org) and use the web interface to translate the files. If you want to contribute to the translations but don't want to use Crowdin, you can also clone this repository and create a pull request with your changes. Please note that we will only accept pull requests for languages that are already available in Crowdin.

All contributors are expected to follow our [Code of Conduct](https://fossbilling.org/docs/contribution-handbook/code-of-conduct) and join our [Discord server](https://fossbilling.com/discord). If you have any questions, please ask them in the #localization channel.

## Adding a new language
To have a language added, please create a new issue in this repository or join our [Discord server](https://fossbilling.com/discord) and request your language in the #localization channel.

If we think think the language is relevant and will be used by a significant number of users, we will add it to Crowdin and you can start translating. A language will be considered relevant if there is an existing userbase that's using the language and willing to contribute to the translations in the long run.

## Maintenance
### Building the .MO files
The .MO files are automatically generated and pushed using GitHub Actions after each PR is merged. If you want to build the files manually, check what [the workflow](https://github.com/FOSSBilling/locale/blob/main/.github/workflows/generate-mo.yml) is doing and replicate it on your machine.

For convenience, you can just create a PR and the workflow will run automatically after it's merged.

### Updating the .POT files
To update the source files from code, create a custom extractor and use the following command:
`xgettext -L PHP --keyword=__trans --add-comments=TRANSLATORS: --force-po -o %o %C %F`