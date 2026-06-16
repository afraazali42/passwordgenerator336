# Password Generator 336

A fast, private password generator. One static HTML file. No build step, no
dependencies, no backend.

## What it does

- Generates strong passwords entirely in the browser using the
  [Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/Crypto/getRandomValues).
- Customizable: length (4-64), character sets (with editable contents per set),
  exclude characters, start-with-letter, no-duplicates, no-sequential, auto-select.
- Bulk generation up to 100 at a time.
- Optional history (stored locally in your browser only, never sent anywhere).
- Shareable settings URL: bookmark your preferred configuration.

## Run locally

It's one file. Open `index.html` in any modern browser.

For a local dev server:

```sh
python3 -m http.server 8765
```

Then open <http://localhost:8765>.

## Deploy

Drop `index.html` at the root of any static host (Cloudflare Pages, GitHub
Pages, Netlify, your own VPS, etc.). There is nothing to build.

## Privacy

All password generation happens in your browser. No password is ever sent over
the network. There is no analytics, no tracking, and no third-party scripts.
The only data written to local storage is the optional history list, and only
when you explicitly enable it.

## License

[MIT](LICENSE)
