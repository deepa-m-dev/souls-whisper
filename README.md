# Soul's Whisper — A Collection of Poems

A personal poetry site with likes, comments, and a mailing-list signup — built as a single `index.html` file with no build tools, backed by Firebase (Firestore) for free.

## Features

- **Poem collection** with genre tabs, search, and sort (newest/oldest)
- **Poem of the Day** — a featured poem on the homepage, rotates daily
- **Likes** — from the card or inside a poem, no login required, unlimited likes per person
- **Comments** — name (optional) + message, shown live under each poem
- **Share** — native share sheet on mobile, or copies a direct link to that poem
- **Next / Previous** navigation inside the poem view
- **Dark mode** toggle (saved across visits)
- **Draw a Verse** — a random poem, flip-card style
- **Subscribe** — collects emails in Firestore for manual mailing later
- **Custom cursor** with a petal trail (mouse + touch, respects reduced-motion settings)

## Tech stack

- Plain HTML/CSS/JavaScript — no framework, no bundler, no npm
- [Firebase Firestore](https://firebase.google.com/docs/firestore) for likes, comments, and subscriber emails (free Spark plan is plenty for a personal site)
- Loaded entirely via CDN `<script type="module">` imports — nothing to install

## Project structure

```
index.html   ← everything: markup, styles, and script, in one file
```

## Adding a new poem

Poems are hardcoded in the `poems` array near the top of the `<script>` tag — no admin panel, no database entry needed. Open `index.html`, find the array, and copy an existing poem block:

```js
{
  id: 5,                     // must be unique — use the next number
  genre: "hope",             // one of: love | melancholy | faith | Philosophical | Introspective
  title: "Your Poem Title",
  excerpt: "First line or two,\nshown on the card preview.",
  body: "Full poem text.\nUse \\n for line breaks.",
  note: "Optional: what this poem means. Delete this line to hide the button."
},
```

Paste it just before the closing `]` of the array (remembering the comma after the previous poem), save, and push.

## Firebase setup (one-time)

1. **Enable Firestore**
   Firebase console → Build → Firestore Database → Create database → pick a nearby region → start in test mode.

2. **Publish these security rules** (Firestore → Rules tab):

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {

       match /likes/{poemId} {
         allow read: if true;
         allow create, update: if request.resource.data.count is int;
         allow delete: if false;
       }

       match /comments/{commentId} {
         allow read: if true;
         allow create: if request.resource.data.text is string
                       && request.resource.data.text.size() > 0
                       && request.resource.data.text.size() < 1000
                       && request.resource.data.name is string
                       && request.resource.data.poemId is string;
         allow update, delete: if false;
       }

       match /subscribers/{subscriberId} {
         allow read: if false;
         allow create: if request.resource.data.email is string
                       && request.resource.data.email.size() > 3
                       && request.resource.data.email.size() < 200;
         allow update, delete: if false;
       }
     }
   }
   ```

3. **Swap in your own Firebase config** at the top of the `<script>` tag (`firebaseConfig` object) — get this from Firebase console → Project settings → your web app.

No Firebase Authentication is used. Likes, comments, and subscriptions are all open to anonymous visitors by design, gated only by the rules above.

## Notifying subscribers of a new poem

There's no automated "email on publish" step — Firestore + a static page can't safely send email on their own. When you publish a new poem:

1. Go to Firebase console → Firestore Database → Data → `subscribers`
2. Copy the email addresses
3. Send a quick update yourself via any free email tool (Mailchimp, Brevo, MailerLite, or just your own inbox)

If this becomes a regular chore, a Cloud Function + an email API (SendGrid/Resend) can automate it — that requires Firebase's paid Blaze plan (usage would very likely stay in the free tier) and some backend code, which is a bigger step than this project currently takes.

## Local testing

Don't open `index.html` by double-clicking it — `file://` URLs block Firebase's network requests. Instead:

- **VS Code**: install the "Live Server" extension, right-click the file → "Open with Live Server"
- **Python**: `python -m http.server 8000` in the project folder, then visit `http://localhost:8000/index.html`
- Or just push to GitHub Pages and test the live URL

## Deploying

Push `index.html` (and this `README.md`) to a GitHub repository, then enable **GitHub Pages** in the repo's Settings → Pages, pointing at the branch and root folder. No build step required.

## Known limitations

- Likes and views have no per-person limit — anyone can like a poem multiple times
- Comments can't be edited or deleted by visitors, and there's no moderation panel — delete unwanted ones directly in the Firestore console if needed
- Subscriber notifications are manual (see above)
- No duplicate-check on subscriber emails — the same address can be submitted more than once
