# amandaondata.com

Personal site for Dr Amanda J. Williamson. Single-file static site (`index.html`) deployed via GitHub Pages from this repo's `main` branch. Custom domain: `amandaondata.com`.

## Repo contents

- `index.html` — the entire site
- `AmandaProfile.jpeg` — headshot used in hero + Open Graph card (EXIF stripped)
- `favicon.svg` — brand mark
- `CNAME` — custom-domain declaration for GitHub Pages
- `.gitignore` — keeps workspace files, `.DS_Store`, secrets, `.claude/` etc out of the repo

## Privacy invariant — IMPORTANT

This repo is **public**. Amanda's Obsidian knowledge base must **never** appear here in any form — not file contents, not paths, not references. The previous workspace file leaked the KB path; that's why it's now in `.gitignore` and was deleted from this folder. Stay vigilant.

## Workflow: add a newly published LinkedIn article

When Amanda says "add this LinkedIn article: <url>" (or similar), follow this exactly:

### 1. Fetch the article metadata

```
WebFetch <url>
prompt: Extract title, og:description, published date, og:image. Return JSON.
```

LinkedIn serves OG tags publicly on `/pulse/` URLs without login. No auth needed.

### 2. Strip tracking params from the URL

LinkedIn share URLs include `?trackingId=...` — remove everything from `?` onwards before saving. Also drop the trailing `/`. The URL stored in HTML should match the format of existing entries in `index.html`.

### 3. Decide placement

- **Top 4 featured cards** = the 4 most recent published articles (newest first, left-to-right then top-to-bottom)
- **"More articles" expandable list** = everything else, also newest first

If the new article is more recent than any of the top 4 (it usually is — that's why we're adding it), insert it at the top of the featured cards and demote the oldest of the existing 4 down into the "More articles" list (also at the top of that list). Maintain newest-first order in both sections.

### 4. Write a blurb in Amanda's voice

Look at existing blurbs in [index.html](index.html) for tone reference. They are:
- Short (1-2 sentences)
- Punchy and declarative
- Often lead with a vivid concrete fact or named source
- Avoid corporate hedge-speak

Examples:
- "FlyZoo's AI-run hotel works. The lesson isn't about AI — it's about integration."
- "80% adoption. Zero process change. What's wrong with how we measure AI success."
- "Advertising executives are teaching robots to sell toothpaste. What that means for everyone else."

If you can't write a blurb you'd be confident she'd like, **show your draft to Amanda before committing**.

### 5. Read time

Use the read-time estimate from LinkedIn if available. Default to 5 min if unknown — most of her posts are in the 4-7 min range.

### 6. Match the existing HTML pattern exactly

Featured card pattern:
```html
<a href="..." target="_blank" rel="noopener noreferrer" class="writing-card block bg-white rounded border border-stone p-5 cursor-pointer border-l-2 border-l-mustard/40">
  <h3 class="font-body font-semibold text-[15px] text-charcoal leading-snug">Title</h3>
  <p class="mt-2 text-warmgray text-sm leading-body">Blurb.</p>
  <div class="mt-3 flex items-center gap-2">
    <span class="text-xs font-semibold text-cobalt">N min</span>
    <span class="text-stone">|</span>
    <span class="text-xs text-warmgray">Mon YYYY</span>
  </div>
</a>
```

More-articles list pattern:
```html
<a href="..." target="_blank" rel="noopener noreferrer" class="view-all-link flex items-baseline justify-between gap-4 py-3 text-warmgray cursor-pointer border-b border-stone/40">
  <span class="text-sm leading-snug">Title</span>
  <span class="text-xs whitespace-nowrap opacity-60">Mon YYYY</span>
</a>
```

Use em-dashes (`&mdash;`) for parenthetical breaks, matching existing entries.

### 7. Commit and push

One commit, with a descriptive message:
```
git add index.html
git commit -m "Add: <article title>"
git push
```

GitHub Pages rebuilds within ~1 minute. The change is live at https://amandaondata.com after that.

### 8. Don't touch anything else

This workflow only adds an article entry. Don't simultaneously refactor the site, restructure sections, or push unrelated changes. Each "add article" should be a single focused commit.

## Workflow: add a media appearance

Same shape as the article workflow above, but the entry goes in the "In the Media" section. The "Recent" group at the top holds the 5 newest; older ones go into the expandable "Earlier media appearances" section grouped by year.

Pattern for media entries:
```html
<a href="..." target="_blank" rel="noopener noreferrer" class="media-entry flex items-start justify-between gap-4 py-3.5 px-3 -mx-3 rounded cursor-pointer border-b border-stone/50">
  <div class="flex-1 min-w-0">
    <span class="text-xs text-warmgray">Mon YYYY · <span class="font-semibold tracking-label uppercase text-[10px]">Outlet Name</span></span>
    <p class="text-sm text-charcoal leading-snug mt-0.5">Headline / topic</p>
  </div>
  <span class="arrow-icon text-cobalt mt-1 flex-shrink-0 text-sm" aria-hidden="true">&rarr;</span>
</a>
```

## Workflow: add a talk

Talks live in the "More talks" section as cards with YouTube thumbnails. The featured TEDx talk is its own section above. Pattern for the talk grid:
```html
<a href="https://www.youtube.com/watch?v=VIDEO_ID" target="_blank" rel="noopener noreferrer" class="talk-card block rounded overflow-hidden border border-stone bg-white cursor-pointer">
  <div class="relative aspect-video bg-stone/30 overflow-hidden">
    <img src="https://img.youtube.com/vi/VIDEO_ID/hqdefault.jpg" alt="..." class="w-full h-full object-cover" loading="lazy"/>
    <div class="absolute inset-0 bg-gradient-to-t from-charcoal/40 to-transparent"></div>
  </div>
  <div class="p-4">
    <h3 class="font-body font-semibold text-sm text-charcoal leading-snug">Talk title</h3>
    <p class="mt-1.5 text-xs text-warmgray">Mon YYYY</p>
  </div>
</a>
```

## Design system reference

Defined inline in `index.html`:
- Colours: charcoal `#1C1C1E`, warmgray `#6B6560`, stone `#E8E4DE`, mustard `#D4A017`, cobalt `#2B4D9B`, crimson `#B22234`
- Fonts: Playfair Display (display/serif), Open Sans (body)
- Layout: single column, `max-w-2xl mx-auto`, generous vertical rhythm
- Tailwind via CDN (intentional — keeps the repo to a single static file)

Keep new sections consistent with this system.
