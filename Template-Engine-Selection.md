The gateway needs to make use of a template engine. Ideally, the engine would plug directly into express such that express can render templates directly. The [express documentation](https://expressjs.com/en/guide/using-template-engines.html) suggests [Pug](https://pugjs.org/api/getting-started.html), [Mustache](https://github.com/janl/mustache.js), or [EJS](https://github.com/mde/ejs). Another option is [consolidate.js](https://github.com/tj/consolidate.js), which wraps a large number of templating engines to make them compatible with express.

* **Pug**
  * **Pros**
    * Default templating engine for express
    * Implementations available in some other languages
  * **Cons**
    * Templates are pseudo-HTML
    * High learning curve
    * Client-side support is brand new and is a massive .js file
    * Can only generate HTML
* **Mustache**
  * **Pros**
    * Templates are HTML with simple template tags inside
    * Client- and server-side support
    * Bindings in a huge number of non-JS languages
    * Can generate any output file type
    * Highly flexible without need for extra control-flow elements
  * **Cons**
    * ??
* **EJS**
  * **Pros**
    * Templates are HTML-like
    * Lots of extra control-flow support in templates
  * **Cons**
    * Essentially writing JS
    * Verbose
    * Not widely used
* **consolidate.js**
  * **Pros**
    * Large variety of templating engines available
  * **Cons**
    * Seems like a lot of extra overhead
    * Server-side only
