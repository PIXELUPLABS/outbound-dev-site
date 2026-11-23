# outbound.dev homepage

Final build, 23 November 2026.

The redesigned outbound.dev homepage as one self-contained file. There is no build step, framework or package to install, and the page makes no requests to other sites: the font and the security badges are embedded in the file.

| File | What it is |
|---|---|
| `index.html` | The whole page: markup, styles, scripts, the Geist font and the four badge images. 211 KB, about 92 KB once the host compresses it. |
| `README.md` | These notes. |

## Deploy

It is a static page, so any static host works. Put `index.html` at the root of the site.

- **Netlify:** drag this folder onto https://app.netlify.com/drop
- **Vercel:** run `npx vercel --prod` inside this folder.
- **Cloudflare Pages:** create a project with Direct Upload and upload this folder.
- **Your own server, S3 + CloudFront or GitHub Pages:** serve `index.html` at `/`.

**Keeping the current Next.js app?** Either port the markup into `app/page.tsx`, or copy the file to `public/home.html` and add a `beforeFiles` rewrite in your Next.js config so `/` serves it:

```js
async rewrites() {
  return { beforeFiles: [{ source: '/', destination: '/home.html' }] };
}
```

## Before you go live

The page links to a few files that are on outbound.dev today. If this page replaces the current site, keep these paths working:

| Path | Used for |
|---|---|
| `/llms.txt` | Linked from the agents section and the footer |
| `/terms`, `/privacy` | Footer links |
| `/opengraph-image.png` | Link previews (`og:image`, `twitter:image`) |
| `/icon-512.png` | Logo in the structured data (JSON-LD) |

Keep your existing `robots.txt`, `sitemap.xml` and `manifest.webmanifest` too.

The page uses a built-in copy of the logo mark as its favicon, so it works on any domain. To use your existing favicon files instead, replace the `<link rel="icon">` line in `<head>`.

## Privacy

- The page loads nothing from other sites, so visitors' IP addresses go only to your host.
- There are no analytics, cookies or trackers. The only thing stored in the browser is the visitor's light or dark choice, in `localStorage` under `ob-theme`.

## Security headers (optional)

The page uses inline scripts and styles. If you send a Content-Security-Policy, this one has been tested with the page and blocks nothing it needs:

```
default-src 'none'; script-src 'unsafe-inline'; style-src 'unsafe-inline'; font-src data:; img-src data:; base-uri 'none'; form-action 'none'; frame-ancestors 'none'
```

It allows nothing from other sites. If you later add analytics or other outside resources, add their origins to the policy.

## Making changes

Everything lives in `index.html`. Edit it and redeploy.

- **Copy:** plain HTML. Each section starts with a banner comment holding its name in capitals, so searching for `PRICING`, `FAQ` or `FOOTER` takes you there.
- **Colours:** CSS variables at the top of the `<style>` block. The dark theme is on `:root` and the light theme on `:root[data-theme="light"]`.
- **Savings calculator:** the competitor and outbound.dev prices are in the script, next to the comment that starts "Their stack, public list prices". When they change, also update the date in the footnote under the calculator ("read on November 10, 2026").
- **Rates elsewhere:** prices also appear as text in How it works, Pricing, the FAQ, the closing call to action and the sample screens. Search the file for the old figure when a rate changes.
- **Embedded files:** the font sits at the top of the `<style>` block and the badges in the Security section, as long `data:` strings. Leave those lines as they are.

## Font

Geist 1.800, by The Geist Project Authors, embedded under the SIL Open Font License 1.1 (https://openfontlicense.org). The copyright and licence notice are in a comment above the `@font-face` rule and inside the font file itself.

Only the Latin subset is embedded, which covers English and Western European text. Characters outside it, such as the ↗ arrow in the mobile menu or Cyrillic letters, show in the device's system font. If you add copy in other alphabets, embed the matching Geist subset the same way.

## How it behaves

- Dark theme by default. The sun and moon button switches it, and the choice is remembered.
- Follows the visitor's "reduce motion" setting: animations stop and their end states show.
- Works from the keyboard, with visible focus on every control.
- With JavaScript off, all the content still shows. The theme switch, calculator and animations need JavaScript.
- Tested in Chrome from 390 to 1908 px wide, in both themes, served over HTTP with the policy above. It made no outside requests. Check Safari and Firefox before launch.
