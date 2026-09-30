# tech-stack-signatures

Detect a website's CMS, ecommerce platform, JS framework, analytics and tag managers,
CDN/hosting, payment and live-chat widgets from one fetched page: response headers,
the `generator` meta tag, script sources and a few HTML markers. No headless browser.

Shared by these Apify actors so the signatures are defined in one place:

- [Website Tech Stack Checker](https://github.com/timmKal01/website-tech-stack-detector) (where this code started)
- [Website Gap Finder](https://github.com/timmKal01/website-gap-finder)

## Install

Pin a release tarball. It installs without git or an npm account, including inside
Apify's `apify/actor-node` build image:

```json
"dependencies": {
    "tech-stack-signatures": "https://github.com/timmKal01/tech-stack-signatures/archive/refs/tags/v1.0.0.tar.gz"
}
```

## Use

`detectTechStack` takes the response headers, the raw HTML and a loaded
[cheerio](https://cheerio.js.org/) document:

```js
import * as cheerio from 'cheerio';
import { detectTechStack } from 'tech-stack-signatures';

const res = await fetch('https://example.com');
const html = await res.text();
const { detected, server, poweredBy, generator } = detectTechStack({
    headers: Object.fromEntries(res.headers),
    html,
    $: cheerio.load(html),
});
// detected: { cms: [], ecommerce: [], jsFrameworks: [], analytics: [], cdnHosting: [], payment: [], liveChat: [] }
```

Also exported: `SIGNATURES` (the list of `{ name, category, test }` rules) and `CATEGORIES`.

## Changing signatures

1. Edit `index.js` and add a fixture plus a test under `test/`.
2. Run `npm test`.
3. Bump the version in `package.json`, commit, and tag it (`git tag v1.1.0 && git push --tags`).
4. In each actor, point the dependency at the new tag and run `npm install`.

Actors keep working on the tag they pin until they're moved to a new one.
