# TelegaSync

Static, versioned JSON catalog for Telega Beta, hosted in a separate repository.

Repository: https://github.com/kirovo389-creator/TelegaSync

## Public data endpoints

Base URL:

`https://raw.githubusercontent.com/kirovo389-creator/TelegaSync/main`

- `config/v1/manifest.json` — schema version, Unicode version, catalog URLs, and size limits.
- `catalog/v1/unicode.json` — full Unicode Emoji 18.0 test catalog (fully-qualified and alternate qualification entries, category, subgroup, code points, and display sequence).
- `catalog/v1/custom_emoji.json` — registry schema for curated Telegram custom-emoji document IDs and fallback emoji. This starts empty and must only be populated with IDs that the project is authorized to use.

Telega downloads the manifest and catalogs over HTTPS, validates the schema/version, caches the last successful catalog, applies fully-qualified Unicode sequences to native keyboard categories, and keeps its built-in list as an offline fallback until a remote catalog has been downloaded.

## Custom emoji IDs are not media

A `document_id` is an identifier, not an image or animation. The client still needs a valid Telegram document and the Telegram API to retrieve/render it; a GitHub catalog does not grant Premium or access to a pack. Do not invent IDs. Add only verified records, e.g. `{"document_id": 123456789, "fallback": "😀"}`, and validate that the referenced media is accessible before release.

## Privacy and writes

This repository is **public and read-only from the APK**. It does not receive personal settings from phones. Never place Telegram session files, login codes, API hashes, access tokens, phone numbers, private settings, messages, or account data in this repository. Personal cross-device settings sync requires a separately authenticated backend; a GitHub token must never be embedded in the APK.

GitHub Raw is a static JSON origin, not a dynamic database or authenticated write API. Catalog changes should be reviewed in GitHub and validated by `.github/workflows/validate-json.yml`.
