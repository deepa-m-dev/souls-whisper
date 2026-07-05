# Soul's Whisper

A personal collection of poems, written whenever a feeling got too big to keep quiet — sad, happy, or somewhere in between.

**[View the live site →](#)** *(add your GitHub Pages link here once it's live)*

## About

Soul's Whisper is a single-page poetry showcase with:

- **Genre filtering** — browse by Love, Sad, Faith, Nature, or Hope
- **Draw a Verse** — a shuffled, tarot-card-style reveal of a random poem
- **"Stuck? See what this poem means"** — an optional, collapsible explanation for readers who want a plain-English read on a poem's meaning
- Soft ambient motion (falling petals, ink-typewriter hero text), fully respecting `prefers-reduced-motion`

## Editing the poems

All poem content lives in one place — no need to touch HTML or CSS.

Open `index.html`, find the `poems` array near the top of the `<script>` tag, and edit the entries directly. Each poem follows this shape:

```js
{
  id: 1,
  genre: "love",          // one of: love | sad | faith | nature | hope
  title: "Where You Are",
  excerpt: "A short teaser line shown on the card.",
  body: `The full poem,
  line by line,
  exactly as you want it to appear.`,
  note: "An optional plain-English explanation of the poem's meaning."
}
```

To add a new poem, copy an existing block, give it a new `id`, and fill in your own text. To add a new genre, also add it to the `genreLabels` object and to the filter tabs in the HTML (`<nav class="genre-nav">`).

## Running locally

No build step — it's a single static HTML file.

```bash
git clone https://github.com/YOUR_USERNAME/souls-whisper.git
cd souls-whisper
open index.html   # or just double-click the file
```

## Publishing with GitHub Pages

1. Push this repo to GitHub.
2. Go to **Settings → Pages**.
3. Under **Source**, select the `main` branch and `/ (root)`.
4. Save — your site will be live at `https://YOUR_USERNAME.github.io/souls-whisper/` within a minute or two.

## License

This project's code is free to use and adapt. The poems themselves are personal writing — please don't republish them elsewhere without asking.
