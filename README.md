# Mandala Language Packs

The community language-pack catalog for [Mandala](https://github.com/MaktubCN/open-mana). Market reads this repository's [`catalog.json`](catalog.json) and downloads reviewed JSON packages from pinned GitHub commits.

## Create and share a language pack

1. Use [`messages.en.json`](messages.en.json) as the source-key and English-reference dictionary.
2. Create a `.mandala-language.json` file with your locale, native language name, writing direction, and translations. Keep the source keys unchanged and preserve placeholders such as `{name}`. See the [copyable example and format guide](CONTRIBUTING.md#language-package-format).
3. Validate the file, fork this repository, and submit it under `packages/` in a PR. Maintainers review the package and publish its catalog entry.
4. Once listed, other users select **Market → Language packs → Install → Use this language**. Installed packs also appear in the language selector in Settings.

See [CONTRIBUTING.md](CONTRIBUTING.md) for validation, publication, updates, and review expectations. The App currently has no visual translation editor or local language-file import button; editing a JSON file and contributing it through GitHub is the supported authoring path.

Installation does not switch languages. Missing translations fall back to English. Applying a pack changes this device's UI language and text reading direction. Sidebar and drawer placement, control order, and layout alignment stay unchanged; RTL text does not mirror the application. Agent response language, memory language, and existing conversations stay unchanged. Uninstalling the active pack returns the UI to English.

## Arabic starter

[`ar.mandala-language.json`](packages/ar.mandala-language.json) is a partial starter covering navigation, Market, and selected settings. It has not been reviewed by a native Arabic speaker and is not a complete localization. Translation improvements and RTL layout feedback are welcome.

## English repository content

Documentation, PR descriptions, commit messages, and catalog descriptions use English. Source message keys currently use Chinese because they are stable identifiers in the App; do not translate or rename them. Translation values and native language names use the target language.

Use a Mandala build that includes theme and language-pack support in Market. An older build labeled `0.1.1` may not include this feature. The example packages and catalog use the [MIT license](LICENSE); externally hosted packages must declare their own redistribution license.

Before launch, this repository is private. Market downloads require the catalog and pinned packages to be public. The linked Mandala source documentation requires access until Mandala is open-sourced.
