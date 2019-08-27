- Copy `static/fluent/en-US/main.ftl` to `static/fluent/<your language>/main.ftl`
- Translate the text in that file
- Perform the following code changes

Add the language to availableLanguages in `static/js/fluent.js`:
```
const availableLanguages = {
  'en-US': ['/fluent/en-US/main.ftl'],
  'es-MX': ['/fluent/es-MX/main.ftl'],
  'your-language': ['/fluent/your-language/main.ftl'],
};
```

Modify the country code in that file's `load()` function if you want to test it out
```
  let language = 'en-US'; // navigator.language;
```