# Contributing language packs

## Language package format

Create `packages/fr.mandala-language.json`, for example:

```json
{
  "format": "mandala-language",
  "version": 1,
  "locale": "fr",
  "name": "Français",
  "direction": "ltr",
  "messages": {
    "市场": "Marché",
    "安装 {name}": "Installer {name}"
  }
}
```

This is a minimal format example, not a complete French translation. The Chinese keys are App identifiers, while the values are French. Start with the keys you can translate confidently, or copy entries from [`messages.en.json`](messages.en.json). That file is a dictionary, **not** an installable pack: wrap your translated dictionary in the `messages` field above.

- Use a canonical BCP 47 locale such as `fr`, `pt-BR`, or `ar`. Built-in `en` and `zh-CN` cannot be overridden.
- Use a recognizable native language name. Set `direction` to `rtl` for right-to-left languages and `ltr` otherwise. This describes text reading direction; it does not mirror the sidebar, drawers, control order, or layout alignment.
- Keep every source key unchanged. Preserve the exact placeholder names, including braces; you may reorder them within a sentence.
- Omit untranslated entries so the App can fall back to English. Empty translation strings are invalid.
- Values are plain text; HTML, CSS, and JavaScript are not interpreted.
- The dictionary must contain 1–10,000 entries. Source keys are limited to 4,000 characters and values to 8,000 characters. The entire file must fit within 4 MiB.
- Describe coverage and native-speaker review honestly. Verify punctuation, numbers, and mixed-direction text; code and terminals remain left-to-right. Compare navigation, settings rows, and Market details before and after switching languages, including narrow windows and a restart.

There is currently no local language-file import or visual translation editor. Run the parser check below before a PR; report linguistic review separately. The maintainer verifies the installation/application flow after catalog publication in an isolated profile. A partial pack is acceptable if its scope is explicit.

To refresh the English reference, maintainers run this from the current open-mana checkout after installing dependencies:

```sh
node --import tsx scripts/export-language-template.ts /absolute/path/to/mandala-language-packs/messages.en.json
```

Some dynamically constructed sentences do not yet have stable template keys. The template is not a promise of complete coverage of every UI string.

## Submit a package

1. Fork this repository on GitHub and create a branch.
2. Add your JSON file under `packages/` (or update the existing file for a new version).
3. Follow the validation and preview steps below.
4. Open a pull request to this repository's `main` branch. The PR template asks for the package ID, display name, English description, publisher, release version, minimum App version, license, and validation results.
5. A maintainer reviews the file, merges it, and publishes a separate catalog change. **A merged package PR alone does not make an item appear in Market.**

For a first contribution, you only need to submit the package and fill in the PR template. Leave `catalog.json` to the maintainer: the final package commit does not exist until your PR is merged. Squash and rebase merges can change commit IDs.

An alternative is to keep the package in your own public GitHub repository. Submit a catalog-only PR here with the existing full commit SHA, file path, SHA-256, and a link to its license. The package must remain publicly downloadable at that commit. Market discovers only entries in this repository's catalog; it does not scan arbitrary GitHub repositories.

Use English for documentation, PR titles and descriptions, commit messages, and catalog descriptions. Localized language names, translation keys, and translated values are intentional exceptions.

## Validate with Mandala's parser

Use a local checkout of [open-mana](https://github.com/MaktubCN/open-mana), with its dependencies installed (`npm ci`) and the Node version required by its `package.json`. Run the following **from the open-mana checkout**, replacing the absolute path:

```sh
PACK_language="language" PACK_FILE="/absolute/path/to/ar.mandala-language.json" node --import tsx --input-type=module <<'JS'
import { readFileSync } from 'node:fs';
import { parseExtensionData } from './shared/ui-extensions.ts';
parseExtensionData(readFileSync(process.env.PACK_FILE, 'utf8'), process.env.PACK_language);
console.log('Package format is valid.');
JS
```

This checks the actual App format. It does not establish translation accuracy, visual quality, licensing, or successful installation. Include your validation result and the App build used in the PR.

## Maintainer publication

Publication uses two commits: the package commit first, then a catalog commit that references it. No release archive, hosted website, or cloud workflow is needed.

1. Review the package, license, and PR evidence. Check for duplicate IDs; for languages, prefer improving an existing locale instead of introducing a second pack for the same locale (both cannot be installed together).
2. Merge the package PR. Fetch the resulting `main` and select the actual commit containing the reviewed bytes, rather than the contributor's pre-merge SHA.
3. Derive the hash from those committed bytes. Example, run in this repository:

   ```sh
   git fetch origin main
   PACKAGE_REVISION=$(git rev-parse origin/main)
   PACKAGE_PATH="packages/ar.mandala-language.json"
   git show "$PACKAGE_REVISION:$PACKAGE_PATH" | shasum -a 256
   ```

4. Add or update an item in `catalog.json`. Copy the existing item as a structural example and replace these fields:

   | Field | Meaning |
   | --- | --- |
   | `id` | Stable unique ID, 1–60 lowercase letters, digits, or hyphens; start with a letter or digit. |
   | `name` | Display name, at most 100 characters. |
   | `description` | English description, at most 2,000 characters; disclose incomplete translations. |
   | `publisher` | Author or organization, at most 100 characters. |
   | `version` | Package release version, such as `1.0.0`; increase for a new release. |
   | `minAppVersion` | Minimum supported App version in `x.y.z` form. |
   | `package.repository` | Public `owner/repository` containing the file. |
   | `package.revision` | Existing full 40-character lowercase commit SHA; never `main`, a tag, or a future SHA. |
   | `package.path` | Repository-relative JSON path, such as `packages/ar.mandala-language.json`. |
   | `package.sha256` | 64-character lowercase SHA-256 of the exact file bytes at that commit. |

   Keep the catalog's `format`, schema `version`, and `kind` unchanged. The catalog entry's release `version` is separate from the package's schema `version: 1`.

5. Validate the candidate catalog with `parseExtensionCatalog` from `shared/ui-extensions.ts`; fetch the pinned public file and check its digest and `parseExtensionData` result. Inspect the diff and publish the catalog change to `main`.
6. In a separate test profile, browse the published item in Market, install it, explicitly apply it, restart Mandala, and confirm it still works. Record the result; investigate or revert a faulty catalog entry. Never use personal screenshots or private backgrounds as public PR evidence.

The App rejects malformed packages, checksum mismatches, and unsupported minimum versions. Each package is limited to 4 MiB; each catalog to 500 entries. Admission to the catalog is not an additional guarantee about a third-party author's identity or work.

## Updates and discovery

Keep the same catalog ID for updates. Commit the new package first, then raise its release version and update the pinned revision and hash in a catalog PR. Do not change the bytes behind a release by reusing its version.

Once the catalog change reaches `main`, users can reopen Market, enable `Mandala Languages` in **Manage sources** if needed, and find the item under **Language packs**. Open its details, select **Install**, then **Use this language**. Installation alone does not change preferences. Existing users can reopen the item from **Browse** to check for an **Update**; updates are not installed automatically. Network access to GitHub is required for discovery and installation. Installed packages remain usable without GitHub access while the local Mandala service is running.
