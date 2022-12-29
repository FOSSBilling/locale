# Updating the .POT File
To update the source files from code, create a custom extractor and use the following command:
`xgettext -L PHP --keyword=__trans --add-comments=TRANSLATORS: --force-po -o %o %C %F`