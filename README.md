# Mandala Language Packs — compatibility catalog

Language development has moved to [mandala-plugins](https://github.com/maktub-ai/mandala-plugins). Submit translations and improvements using its [language contribution guide](https://github.com/maktub-ai/mandala-plugins/blob/main/docs/languages.md) and [English reference dictionary](https://github.com/maktub-ai/mandala-plugins/blob/main/messages.en.json).

This repository remains an entry point for older Mandala applications. Its `catalog.json` preserves the `language:arabic-starter` identity and pins the same language payload now hosted in `mandala-plugins`. Existing historical package files and commits remain available; no history has been rewritten. The Arabic translation is still partial and has not been reviewed by a native Arabic speaker.

New applications browse the unified data plugin catalog. Migration-aware applications can explicitly replace the official legacy Arabic package with its plugin while preserving the selected locale. Merely reading installed data does not rewrite it. Installed translations continue working offline.

Do not delete this repository while older binaries request its catalog or users rely on fixed historical package URLs. Keep it private until the owner separately approves publication. Private catalogs cannot be read by Market's anonymous downloader.
