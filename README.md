# Zeka BIR Tech Solutions — website

Static site. No build step, no framework, no dependencies.
Upload the folder and it runs.

```
index.html                  Full site (HTML + CSS + JS in one file)
404.html                    Branded not-found page
assets/mark.png             Logo mark, transparent, 1024×1024
assets/logo-original.jpeg   Your original supplied artwork, kept for reference
assets/vikas-arora.jpg      Founder portrait, 900px
favicon.png                 256×256
apple-touch-icon.png        180×180
og-image.png                1200×630 social share card
robots.txt · sitemap.xml
netlify.toml · vercel.json  Security headers, caching, 404 routing
README.md
```

---

## 1. Two things to set before you publish

| Find in `index.html` | Replace with |
|---|---|
| `https://www.zekabir.com/` | Your live domain (canonical, OG tags, JSON-LD) |
| `var ENDPOINT = "";` | Your form endpoint — see section 2 |

Also change the domain in `sitemap.xml` and `robots.txt`.

Email and the Connaught Place address are already in the contact block, the
footer and the JSON-LD business record. **No phone number appears anywhere** —
removed from the contact section, the footer and the structured data.

## 2. Making the contact form deliver

With `ENDPOINT` empty the form validates input and opens the visitor's mail
client with everything pre-filled. Workable, but it loses enquiries from anyone
without a mail client set up. Pick one:

**Formspree (2 minutes)** — create a form, copy the endpoint, set
`var ENDPOINT = "https://formspree.io/f/xxxxxxx";`

**Netlify Forms (free on Netlify)** — add `name="contact" method="POST"
data-netlify="true"` to the `<form>` tag, add
`<input type="hidden" name="form-name" value="contact" />`, and set
`var ENDPOINT = "/";`

**Your own backend** — POST JSON to any URL returning 2xx. Payload keys:
`name, company, email, phone, interest, message`.

The `_gotcha` field is a honeypot: bots fill it, humans don't, and those
submissions are silently dropped.

## 3. Deploy

**Netlify** — drag this folder onto app.netlify.com/drop. Live in seconds.
**Vercel** — `npm i -g vercel && vercel --prod`
**GitHub Pages** — push, then Settings → Pages → Source: `main` / root.
**cPanel / FTP** — upload everything into `public_html/`.
**Cloudflare Pages** — connect the repo, blank build command, output `/`.

Each issues a free certificate. Force HTTPS, pick either `www` or the apex
domain, and keep the canonical tag matching whichever you choose.

## 4. Analytics (optional)

Paste before `</head>`:

```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date()); gtag('config', 'G-XXXXXXX');
</script>
```

---

## How the two grounds are used

The page alternates deliberately. Light is for **reading**; ink is for the
**moments that should land**. A visitor scrolling gets a rhythm rather than one
unbroken field, and nothing feels heavy.

| Section | Ground | Why |
|---|---|---|
| Hero | White | First impression should be open and confident, not heavy |
| About Us | Soft grey | Longest read on the page — easiest on light |
| **Focus** | **Ink** | The animated centrepiece. Gold line work only glows on dark |
| Services | White | A scannable grid reads fastest on white |
| **Call to action** | **Ink** | Punctuation before the form — it should stop you |
| Contact | Soft grey | Forms are easier to complete on light |
| Footer | Ink | Closes the page the way the CTA opened it |

Every component reads its colours from the same semantic tokens
(`--bg`, `--text`, `--accent`, `--line`…), and `.s-dark` simply swaps their
values. That is why buttons, rules, cards and all five diagrams look correct on
both grounds without a single duplicated style.

| Token | Light | Ink |
|---|---|---|
| `--bg` | `#FFFFFF` | `#12151B` |
| `--surface` | `#F4F6F5` | `#191D25` |
| `--text` | `#12151B` | `#F3F2EF` |
| `--accent` | `#9A7B33` | `#D7BD84` |

The gold shifts deeper on white and lighter on ink so contrast stays legible on
both — the same brass hue, tuned for its background.

**Type.** Newsreader for headlines, Inter for body, IBM Plex Mono for labels.
Nothing is set below 12.5px.

**Focus — the five points revolve.** They sit on an orbit around the central
mark. Every 5.2 seconds the ring turns exactly 72°, carrying the next point up
into the gold bracket at twelve o'clock; each label counter-rotates by the same
amount over the same 1.15s easing, so text stays upright the whole way round.
The live point lights up, and the panel beside it swaps to that item's title and
its geometric animation: columns growing under a trend line, reach radiating
from an origin, scattered inputs reorganising through a hub, a capital stack
filling, partnership ties drawing across.

Clicking any point jumps straight to it and takes the shortest way round; arrow
keys step through; hovering the orbit or the panel pauses the cycle; it only
runs while on screen; and the ring parks under `prefers-reduced-motion`.

**No icon sets.** Every graphic is inline SVG generated from real coordinates —
vector, sharp at any size, no extra requests.

**No perishable numbers.** Portfolio counts, percentages and mandate counts were
removed: a static site has nobody updating them, and stale figures cost more
credibility than absent ones. What remains stays true — *two decades* and
*30+ lender partnerships*.

## Editing common things

- **Focus items:** the `FOCUS` list at the top of the page source drives the
  orbit points, the titles and the panels — the node angles are generated at
  `360 / count` intervals, so keep the three sets in the same order.
- **Cycle speed:** `var DUR = 5200;` in the script (milliseconds).
- **Orbit radius:** `--r` on `.orbit` (a fraction of `--size`). Adding or removing
  a point means changing `STEP` in the script to `360 / count`.
- **Services:** the six `<article class="svc">` blocks.
- **Credentials:** the two `.cred` blocks in the hero, two `.stat` blocks in About.
- **Change a section's ground:** swap `s-light` for `s-dark` on the `<section>`.
  Nothing else needs touching.

## A note on client names

No client is named anywhere. If you later get written consent, a logo wall
belongs directly beneath the Services grid.
