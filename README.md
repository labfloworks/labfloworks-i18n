# FloWorks Internationalization (i18n)

Help us bring FloWorks to users worldwide in their native language.

## About the Project

FloWorks is developed by an independent team in Colombia. While our resources are lean, our ambition to build an accessible, global tool is huge. We recognize that automated or initial translations may contain inaccuracies, which is why community contributions are vital to improving the platform. 

To maintain system stability, we enforce strict schema alignment across all language files, ensuring the user interface renders reliably regardless of locale.

## Native Coverage & Community Impact

Our current set of supported locales covers approximately **50% to 55% of the global population in their native language (L1)**. In specialized technical, engineering, and scientific fields—where FloWorks operates—this native coverage reaches an estimated **75% to 80% of researchers, engineers, and developers worldwide**.

While our primary supported languages cover key global regions, native speakers who use FloWorks in their mother tongue experience significantly better accessibility, reduced cognitive load, and more natural workflows. Expanding and refining these locales helps lower barriers for technical communities in underrepresented regions.

## Currently Available Locales

FloWorks currently includes base translation files for the following 18 language codes:

- **cs** - Czech (Čeština)
- **de** - German (Deutsch)
- **el** - Greek (Ελληνικά)
- **en** - English
- **es** - Spanish (Español)
- **fr** - French (Français)
- **hi** - Hindi (हिन्दी)
- **it** - Italian (Italiano)
- **ja** - Japanese (日本語)
- **ko** - Korean (한국어)
- **nl** - Dutch (Nederlands)
- **pl** - Polish (Polski)
- **pt** - Portuguese (Português)
- **ro** - Romanian (Română)
- **ru** - Russian (Русский)
- **tr** - Turkish (Türkçe)
- **vi** - Vietnamese (Tiếng Việt)
- **zh** - Chinese (中文)

If your native language is missing—or if you notice inaccuracies in any of the above—your contribution will directly impact thousands of users in your language community.

## How to Contribute

### Adding a New Language

1. **Fork** this repository.
2. Navigate to the `locales/` directory.
3. Duplicate `en.json` (or `es.json`) to use as a baseline.
4. Rename the file using the standard 2-letter ISO language code (e.g., `ca.json` for Catalan).
5. Translate the values on the right side of the key-value pairs.

> **Mandatory requirement:** The new file must mirror the exact key structure of `en.json` / `es.json`. Every key present in the base files must exist in your submission. Structural accuracy and key parity are critical; absolute line counts may vary depending on formatting.

### Updating an Existing Language

1. Locate the target `.json` file inside `locales/`.
2. Refine or correct any inaccurate translation values.
3. Ensure the file remains fully synchronized with the base reference files in both structure and key inventory.

## Translation Guidelines

- **Preserve JSON Keys:** Modify only the string values (right side). Do not alter key identifiers.
  - *Incorrect:* `"btn_cancel": "Cancel·lar"` modified to `"btn_cancelar": "Cancel·lar"`
  - *Correct:* `"btn_cancel": "Cancel·lar"`
- **Retain Dynamic Placeholders:** Leave variable tokens like `{count}`, `{filename}`, or `%s` intact within the string.
- **Ensure Valid JSON Syntax:** Verify that the file parses correctly (e.g., watch out for trailing commas).
- **Encoding:** Save files strictly using **UTF-8** encoding.

## Submitting Your Work

Submit your updates by opening a Pull Request (PR) with a clear title format:

- `i18n: add Catalan translation (ca.json)`
- `i18n: update French locale`

Our maintainers will validate the JSON schema and syntax prior to merging into the main release.

Thank you for helping us make FloWorks globally accessible!
