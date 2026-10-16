# TelegaSync

Remote, **read-only** configuration and catalog for Telega Beta.

Repository: https://github.com/kirovo389-creator/TelegaSync

## What this is

GitHub Raw serves versioned JSON files that Telega can download without embedding a GitHub token in the APK. This repository is a static data origin, **not a private database or authenticated API**. Public files are readable by everyone.

Base URL:
`https://raw.githubusercontent.com/kirovo389-creator/TelegaSync/main`

- `config/v1/manifest.json` — schema version and links to catalogs.
- `catalog/v1/unicode.json` — Unicode emoji metadata, keyed by standard Unicode sequence.
- `catalog/v1/custom_emoji.json` — Telega custom emoji registry schema; initially empty until verified document IDs and licensed/authorized media are supplied.

## Safety and limitations

- Never put Telegram session files, login codes, API hashes, access tokens, phone numbers, message text, or private settings in this public repository.
- The app must validate schema/version/size and use cached data if the remote is unavailable.
- A Telegram custom emoji document ID is only an identifier; it does not contain the media. The client still needs a valid source document and must respect Telegram's access rules.
- GitHub Raw/Pages cannot accept private per-user writes. Personal settings sync needs a separately authenticated backend or a deliberate account-based authorization design. Do not put a GitHub PAT in the APK.
- GitHub Pages is optional for a web status page; the app can read the Raw JSON URLs directly.

## Editing

Edit JSON via pull requests where possible. Keep the schema version stable for compatible additions; bump it only for breaking changes. Validate JSON before committing.
