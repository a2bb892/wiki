## Pontoon

Translations can be added via Pontoon: https://pontoon.mozilla.org/projects/webthings-gateway/

## Old Instructions

- Start in the WebThings Gateway repository: https://github.com/mozilla-iot/gateway
- Copy `static/fluent/en-US/main.ftl` to `static/fluent/<your language>/main.ftl`
- Translate the text in that file
- Perform the following code changes

Add the language to `availableLanguages` in `static/js/fluent.js`:
```js
const availableLanguages = {
  de: ['/fluent/de/main.ftl'],
  'en-US': ['/fluent/en-US/main.ftl'],
  en: ['/fluent/en-US/main.ftl'],
  fr: ['/fluent/fr/main.ftl'],
  'fy-NL': ['/fluent/fy-NL/main.ftl'],
  it: ['/fluent/it/main.ftl'],
  ja: ['/fluent/ja/main.ftl'],
  nl: ['/fluent/nl/main.ftl'],
  pl: ['/fluent/pl/main.ftl'],
  ru: ['/fluent/ru/main.ftl'],
  sr: ['/fluent/sr/main.ftl'],
  // your language
};
```

Add the language to `l10n.toml`:
```toml
locales = [
  "de",
  "fr",
  "fy-NL",
  "it",
  "ja",
  "nl",
  "pl",
  "ru",
  "sr",
  # your language
]
```

Modify the country code in that file's `load()` function if you want to test it out
```js
  language = 'your language'; // response.current || navigator.language || 'en-US';
```