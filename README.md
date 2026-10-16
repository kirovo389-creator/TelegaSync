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

A `document_id` is an identifier, not an image or animation. The client still needs a valid Telegram document and the Telegram API to retrieve/render it; a GitHub catalog does not grant Premium or access to a pack. Do not invent IDs. Add only verified records, e.g. `{"document_id": "5875465628285931233", "fallback": "✈️"}`, and validate that the referenced media is accessible before release.

## Premium/custom emoji data

The current `catalog/v1/custom_emoji.json` includes 1,923 curated fallback-to-ID mappings imported from [abbosahmad/telegram-premium-emojis](https://github.com/abbosahmad/telegram-premium-emojis), file `catalog/all_emojis.json`, under its MIT license. Attribution/license text is preserved in `LICENSES/telegram-premium-emojis-MIT.txt`.

IDs are stored as decimal strings so 64-bit values are not rounded by JavaScript tooling. The client may look up a registered ID by its fallback character; the Telegram client/API still has to retrieve the associated document. This catalog is community-curated rather than an official guarantee that each ID remains accessible.

## Privacy and writes

This repository is **public and read-only from the APK**. It does not receive personal settings from phones. Never place Telegram session files, login codes, API hashes, access tokens, phone numbers, private settings, messages, or account data in this repository. Personal cross-device settings sync requires a separately authenticated backend; a GitHub token must never be embedded in the APK.

GitHub Raw is a static JSON origin, not a dynamic database or authenticated write API. Catalog changes should be reviewed in GitHub and validated by `.github/workflows/validate-json.yml`.

## Shared profile style sync

Public registry: `profiles/v1/profiles.json` at `https://raw.githubusercontent.com/kirovo389-creator/TelegaSync/main/profiles/v1/profiles.json`. It stores only public style metadata: Telegram user ID, accent color, and background emoji document ID. Never put messages, phone numbers, session files, login codes, API hashes, or access tokens in it.

The `.github/workflows/profile-style-sync.yml` workflow can update this registry from issues titled with `[Telega profile sync]`. The issue body must contain `<!-- telega-profile-style-v1 -->` followed by a JSON object, for example:

```json
{"telegram_user_id":"123456789","accent_color":16711680,"background_emoji_id":"0"}
```

Before it can write, configure the repository Actions variable `TELEGA_SYNC_ALLOWED_USERS` as a comma-separated list of trusted GitHub usernames. The workflow uses GitHub's short-lived repository token; no owner token is embedded in the APK.

This is not an anonymous HTTP write API: a client needs a signed-in GitHub account to submit an issue, and the Android app still needs code to read the registry and initiate submissions. The allowlist limits submitters but does not prove that a submitted Telegram ID belongs to the submitter. Treat this as a friends-only feature, not authentication.
