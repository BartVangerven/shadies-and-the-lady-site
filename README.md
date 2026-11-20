# Shadies & The Lady Website

Official one-page website for **Shadies & The Lady**, a Belgian pop/rock cover band from Limburg.

## Live Website

https://www.shadiesandthelady.be/

## Project Overview

This website is built as a clean, maintainable static project with:

- HTML5
- CSS3
- Vanilla JavaScript
- GitHub Pages

No frameworks or build tools are required. Open `index.html` in a browser to preview locally.

## File Structure

```
index.html          Page structure, meta tags, JSON-LD band data
404.html            "Page not found" page (GitHub Pages shows it for unknown addresses)
favicon.ico         Fallback site icon for browsers and search engines
css/style.css       All styling
js/script.js        Translations, gig calendar, interactions
assets/images/      Hero photo, headshots, gallery photos, posters, logo
assets/fonts/       Self-hosted Bebas Neue and Manrope (SIL Open Font License)
assets/audio/       Teaser recording (MP3), played in the Repertoire section
```

## Main Features

- Responsive one-page layout
- Sticky navigation with smooth scrolling
- EN/NL bilingual interface (Dutch default)
- Hero section with a full-width band photo and booking call-to-action
- Band section with expandable member bios
- Repertoire in three clean song-list blocks (Classics, 90s/00s, Modern)
- Self-maintaining gig calendar with automatic "next show" highlight
- "Recently played" list built from past gigs
- Event posters that expand to full size when clicked
- Photo gallery with lightbox (arrows, swipe, Escape, keyboard support)
- Teaser player at the top of the Repertoire section, styled to match the site; the
  "listen" link in the hero scrolls to it and starts playback
- Custom 404 page
- Contact form integrated with Formspree
- Inline booking flow inside the form (3 steps)
- Email copy-to-clipboard button
- Social icon links (Facebook, Instagram, Linktree) with tooltips
- Footer with expandable Legal and Privacy/GDPR sections
- JSON-LD structured data for the band and upcoming events
- Respects the visitor's reduced-motion setting
- Self-hosted fonts (no requests to Google Fonts)
- Lightweight WebP thumbnails; the lightbox loads the full photo

## Updating the Gig Calendar

All gigs live in the `GIGS` array near the top of `js/script.js`. The cards on the
page and the search-engine event data are both generated from it, so this is the
only place that needs editing.

```js
{
  date: "2026-08-29",                          // ISO yyyy-mm-dd
  title: "2 Jaar De Stip",
  place: "Stevoort",                           // shown on the card
  venue: "De Stip",                            // used for search engines
  city: "Stevoort",                            // used for search engines
  poster: "assets/images/AfficheDeStip.jpeg",  // optional
  featured: true,                              // optional
  private: true                                // optional
}
```

Behaviour worth knowing:

- **Past gigs move to "Recently played" automatically.** A gig stays in the upcoming
  list for the whole of its own day and moves the next morning. The six most recent
  public gigs are shown, so keep past gigs in the array and delete the oldest ones
  whenever convenient.
- **The list sorts itself by date**, so new gigs can be added anywhere in the array.
- **The next upcoming gig is featured automatically.** Add `featured: true` only to
  override this and headline a different gig.
- **`poster`** is optional. Leave it out and no poster button appears, which is the
  normal case for private bookings. Sizing is automatic: large on the featured card,
  compact on the others.
- **`private: true`** keeps a gig visible on the site but out of the structured data,
  so it will not appear in Google event results. Private gigs are also never featured
  and are left out of "Recently played".
- If every gig has passed, the section shows a booking prompt, followed by the
  "Recently played" list.

To add a poster, drop the image in `assets/images/` and reference it in the `poster`
field. Filenames are case-sensitive on GitHub Pages, so they must match exactly.

## Updating Text

All visible text is translated through the `translations` object in `js/script.js`,
with one entry per language. Both `nl` and `en` must contain the same keys, otherwise
the raw key name will be shown to visitors.

Member bios use `\n` to separate paragraphs.

The Dutch text also appears in `index.html` (it is what search engines and visitors
without JavaScript see). When you change a Dutch text in `script.js`, change it in
`index.html` too so the two stay the same.

## Contact Form Setup

Form submissions are handled through Formspree.

Current endpoint used in the project:

`https://formspree.io/f/mwlevdzy`

### Anti-spam

This project currently uses:

- Formspree spam filtering
- Hidden honeypot field (`_gotcha`)

Captcha can be enabled later if spam volume increases.

## Bilingual Support

Language switcher supports:

- NL (default)
- EN

Translations are managed in `script.js` through a centralized dictionary. Language
preference is saved in browser local storage. Dates are formatted per language, so
month abbreviations do not need translating by hand.

## Audio

The teaser lives at `assets/audio/shadies-and-the-lady-teaser.mp3` and is played by
the player with `id="teaser"` at the top of the Repertoire section in `index.html`.
`script.js` replaces the browser's own audio controls with a styled play button and
progress bar. The `<audio>` element uses `preload="none"`, so the file only
downloads when someone presses play.

To replace or add a recording:

- Export or convert to MP3 (about 190 kbps VBR, 44.1 kHz). The teaser was made with
  `ffmpeg -i input.wav -ar 44100 -c:a libmp3lame -q:a 2 output.mp3`.
- Keep the original WAV files out of this folder. They are large (the teaser WAV is
  69 MB) and GitHub's upload page refuses files over 25 MB.
- Update the title, `data-duration` (in seconds) and the time shown in the player in
  `index.html`.

## Images

- Hero photo: `assets/images/band-live-960.webp`, `-1600.webp` and `-2048.webp`
  (three sizes of the same photo; the browser picks the right one)
- Social share preview: `assets/images/social-preview-band.jpg` (1200x630)
- Gallery photos: each photo has a small `-thumb.webp` version shown in the grid
  (about 800px, 20-60 KB) and the original JPG, which opens in the lightbox. When
  adding a photo, add both and set `src` to the thumbnail and `data-full` to the JPG.
- Headshots: `headshot-<name>.webp`, about 360px high
- Site icons: `Shadies-and-the-lady_favicon.png` (32px), `favicon-96.png`,
  `favicon-192.png`, `apple-touch-icon.png` (180px) and `/favicon.ico`. Google only
  shows an icon in search results when one of them is at least 48px.
- Posters: portrait orientation works best

After changing the share image, refresh the cached preview through the
[Facebook Sharing Debugger](https://developers.facebook.com/tools/debug/) and the
[LinkedIn Post Inspector](https://www.linkedin.com/post-inspector/).

## Repository Workflow

Recommended lightweight flow:

1. Make focused changes in one update batch.
2. Commit with a clear message.
3. Verify on GitHub Pages in both NL and EN.
4. Tag stable milestones with releases.

## Release Status

Current recommended release stage:

- `v1.4.0` - Gig Calendar & Poster Release

## Planned Next Improvements

- YouTube video
- Optional technical rider as downloadable PDF (on request)
- Optional captcha if spam appears
- Optional analytics/privacy banner if needed
- Final legal text review

## Legal and Privacy Note

Website contact data is used only to respond to booking requests and is not shared with third parties.

## Maintainer

Shadies & The Lady
