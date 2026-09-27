# Courtney Lawson — portfolio site

A single-page site with no build step, no framework, and no backend. One HTML file plus an assets folder.

```
index.html                         the whole site (styles, content, renderer)
assets/
  favicon.svg                      browser tab icon
  courtney-lawson-resume.pdf       [ADD] your résumé
  social-card.png                  [ADD] 1200×630 link-preview image
  projects/
    tipout/                        TipOut screenshots go here
    lead-intake/
    llm-evaluation/
    responsible-ai/
README.md                          this file
```

Inside `index.html` there are three clearly marked parts:

1. **Styles** — the design system. You should not need to touch this.
2. **CONTENT** — a banner reads `CONTENT — everything Courtney edits lives in this one object`. Everything you edit is here, in 14 numbered sections.
3. **RENDERER** — a banner reads `RENDERER — no content below this line`. Leave it alone.

Every unfinished item on the site is a searchable placeholder in `[SQUARE BRACKETS]`. Search the file for `[ADD` to find all of them.

---

## Where each thing lives

| # | Section in CONTENT | Controls |
|---|---|---|
| 1 | `site` | Name, email, LinkedIn, GitHub, location, résumé file |
| 2 | `nav` | Menu items and order — Home / Projects / About / Contact |
| 3 | `home` | Headline, intro, hero card, the four hero columns, every homepage section intro |
| 4 | `lifecycle` | Evaluate → Build → Implement → Govern |
| 5 | `projectsPage` | Projects page heading, intro, filter toggle |
| 6 | `projects` | All projects **and** their case studies — each one has a `published` field, see below |
| 7 | `inProgress` | The "Currently building" list under Projects |
| 8 | `skills` | Tools and competencies |
| 9 | `about` | About narrative and the three-part progression |
| 10 | `experience` | Jobs — shown on the About page (which now includes what used to be a separate Résumé page) |
| 11 | `education`, `credentials` | Degrees and certifications |
| 12 | `contact` | Contact heading and intro |
| 13 | `diagrams` | The three SVG diagrams |

About and Résumé are now a single page (`/about`). There is no standalone Résumé page or nav item — the About page ends with a "Download résumé →" button that points at `site.resumeUrl`.

---

## How to edit my headline / bio

**Headline** — section 3, `home`. It is split into three parts so the italic word stays italic:

```js
headlineLead: "Building better",
headlineEm:   "systems",        // this word renders in italic aubergine
headlineTail: "with AI.",
intro: "I work across AI quality, systems, product…"
```

**The four columns under the hero** — `home.strip`. Four objects, each `{ k: "label", v: "sentence" }`. Keep them short; they are the 10-second read.

**Bio** — section 9, `about.paragraphs`. An array of paragraphs; add or remove entries freely.

---

## How to edit contact information

Section 1, `site`. Fill in the blanks:

```js
email: "you@domain.com",
linkedin: "https://www.linkedin.com/in/yourprofile",
github: "https://github.com/yourhandle",
location: "Atlanta, GA · Open to remote",
resumeUrl: "assets/courtney-lawson-resume.pdf",
resumeUpdated: "Updated February 2026"
```

Anything left as `""` shows a `[ADD …]` placeholder on the Contact page, and clicking it pops a reminder rather than opening a dead link. Fill one field and every button and link that uses it updates at once.

---

## How to add a project

Section 6. Copy one entire project object — from its `{` to the matching `},` — paste it into the array, and edit:

```js
{
  num: "05",                                  // the number shown next to the title
  slug: "my-project",                         // must match the assets folder name
  route: "/projects/my-project",              // unique, starts with /projects/
  title: "My Project",
  published: false,                           // see "Publishing a project" below
  tags: ["Evaluation","AI Systems"],          // small pills on the card
  category: "Evaluation",                     // used by the optional filters
  accent: "#6E5C91",                          // tints the small section numbers down the case study
  cardBlurb: "One or two sentences. Used on Projects, Home, and the About page.",
  lede: "One sentence under the case-study title.",
  cardImage: { src:"assets/projects/my-project/hero.png", alt:"…", placeholder:"[ADD HERO IMAGE]", kind:"wide" },
  meta: [                                     // renders as ROLE / … DEPLOYMENT / …
    { k:"Role",   v:"What you did" },
    { k:"Method", v:"How" },
    { k:"Stack",  v:"Tools" },
    { k:"Stage",  v:"Where it stands" }
  ],
  sections: [ … ]                             // see the block list below
}
```

Routing, the Projects page, the Home previews, the About page's project list, and the "Next —" link at the bottom of every case study all pick it up automatically. There is nothing to register.

Then create `assets/projects/my-project/` for its images.

### Publishing a project

`published: true` or `published: false` on each project is the single on/off switch for the whole site. A project set to `false`:

- Never appears on Home, Projects, or About
- Has no route — its URL simply doesn't exist on the public site
- Is never a "Next —" link at the bottom of another case study
- Is not counted anywhere

Everything else about it — its full case study, images, diagrams, content — stays exactly as written in the file. Nothing needs to be rewritten later; flipping `published` to `true` is the entire publish step.

Right now: TipOut is `published: true`. AI Lead Intake, LLM Evaluation Study, and Responsible AI Implementation Assessment are `published: false` because they're still in progress — their case studies are fully drafted and ready to go the moment each project is actually finished.

---

## How to edit a project

A case study is a list of `sections`. Each has a `kicker` (the label in the left margin), an optional `tone`, and a list of `blocks`.

```js
{ kicker:"The problem", tone:"lav", blocks:[ … ] }
```

`tone` sets the section's background and is how the page gets visual rhythm:
- omit it → normal milk background
- `"lav"` → lavender band
- `"dark"` → deep aubergine band

Use one or two per case study, not every section.

### Block types

```js
{ t:"prose", html:"<p>Paragraphs, <h3>subheads</h3>, <ul><li>lists</li></ul>, <strong>bold</strong>.</p>" }
{ t:"lede", text:"A larger pull paragraph. Plain text." }
{ t:"note", html:"<b>Callout.</b> Lavender box with a left rule." }
{ t:"todo", text:"[ADD SOMETHING]" }
{ t:"checklist", items:["Line one","Line two"] }
{ t:"spec", rows:[ {k:"Framework", v:"React Native"} ] }
{ t:"specPair", left:[{k:"E1",v:"…"}], right:[{k:"E5",v:"…"}] }
{ t:"table", head:["Field","Type","Why"], rows:[["a","b","c"]] }
{ t:"images", layout:"trio", items:[ … ] }      // layout: "trio" = 3 across, "pair" = 2 across
{ t:"diagram", key:"leadIntake", legend:[{fill:"var(--lav-milk)",stroke:"var(--lav)",label:"AI step"}] }
```

Only `html` fields accept HTML. Everything else is plain text and safely escaped.

Only include the sections a project actually needs — case studies do not have to match each other.

---

## How to replace TipOut screenshots

1. Export the screenshots. Put them in `assets/projects/tipout/` with clear names: `dashboard.png`, `shift-entry.png`, `earnings.png`, `expenses.png`, `history.png`, `subscription.png`.
2. In section 6, project `01`, find the **Product screens** section. Each image is one object:

```js
{ src:"", alt:"TipOut earnings dashboard", placeholder:"[ADD TIPOUT DASHBOARD SCREENSHOT]", kind:"device", caption:"Dashboard — net at a glance" }
```

Fill in `src`:

```js
{ src:"assets/projects/tipout/dashboard.png", alt:"TipOut earnings dashboard", placeholder:"[ADD TIPOUT DASHBOARD SCREENSHOT]", kind:"device", caption:"Dashboard — net at a glance" }
```

The moment `src` has a value, the placeholder disappears and the real image renders. Leave `placeholder` in place — it costs nothing and it's the fallback.

3. Do the same for `cardImage` at the top of the TipOut object — that's the image on the Projects page and homepage.
4. Set `accent` to TipOut's real brand colour (currently `#8A3AE7`, sampled from the real screenshots). It tints the small `01`, `02`... numbers that run down the left margin of the case study, which is how each product's own colour enters the site.

`kind` controls the frame: `"device"` is a tall phone shape, `"wide"` is 16:10, `"detail"` is 4:3.

Always write a real `alt` description. Every image is click-to-enlarge and keyboard-openable with Enter.

---

## How to add project images

Same pattern for any project. Put files in `assets/projects/<slug>/`, then either fill a `src` in an existing `images` block or add a new block:

```js
{ t:"images", layout:"pair", items:[
  { src:"assets/projects/lead-qualification/workflow.png", alt:"n8n orchestration workflow", placeholder:"[ADD N8N WORKFLOW SCREENSHOT]", kind:"wide", caption:"Orchestration workflow" }
]}
```

Keep images under about 500 KB each. PNG for interface screenshots, JPG for photographs.

### Dropping in the Lead Qualification screenshots

The "AI-Assisted Lead Qualification & Lifecycle Automation" case study (`assets/projects/lead-qualification/`) is written and published, with four named image slots waiting for the real screenshots:

| Section | What goes there | `alt` / caption already written |
|---|---|---|
| System design | The full n8n workflow, wide | "Artifact A — the complete workflow…" |
| Qualification architecture | Normalized input + score + tier + AI analysis | "Artifact B — normalized input…" |
| Implementation | The high-priority Slack alert | "Artifact C — the internal Slack alert…" |
| Implementation | The personalized follow-up email | "Artifact D — a follow-up generated…" |

Crop or redact each screenshot before adding it — trim out browser chrome and any real names, emails, phone numbers, or other contact details — then just fill in that block's `src` the same way as any other image (see above). The placeholder disappears the moment `src` has a value. The homepage/Projects card for this project currently falls back to its architecture diagram (via `diagram:"leadIntake"` on `cardImage`) — once you have a real hero screenshot, set `cardImage.src` and it'll use that instead.

---

## How to update experience

Section 10. Each job is one object:

```js
{
  org: "Invitation Homes",
  role: "Assistant Portfolio Manager",
  dates: "2022 — Present",      // "" renders [ADD DATES]
  summary: "One paragraph — shown on About.",
  bullets: ["Shown on the About page", "One line each"]
}
```

Add a job by copying an object. The newest goes first.

---

## How to update education / certifications

Section 11. Replace each `v: ""` with the real text; the `placeholder` only shows while `v` is empty. Delete rows you don't need, copy a row to add one.

```js
education: [
  { k:"Degree", v:"B.S. Finance, Western Governors University (in progress)", placeholder:"[ADD DEGREE, INSTITUTION, STATUS]" }
],
```

---

## How to replace my résumé PDF

1. Save the PDF as `assets/courtney-lawson-resume.pdf`.
2. Set `site.resumeUrl: "assets/courtney-lawson-resume.pdf"` and `site.resumeUpdated: "Updated March 2026"`.

Every "Download résumé" button on the site now points at it — including the one at the end of the About page, which is the only place résumé download lives now (there's no separate Résumé page). To swap in a newer version later, just overwrite the file — keep the same filename and nothing else needs to change.

---

## How to run the site locally

Double-clicking `index.html` works for most things. To get images and the PDF loading exactly as they will in production, run a tiny local server from the project folder:

```bash
# Python (already installed on macOS)
python3 -m http.server 8000
```

Then open `http://localhost:8000`. Stop it with Ctrl-C.

No `npm install`, no build, no dependencies.

---

## How to deploy

**GitHub → Vercel**

1. Create a GitHub repository (e.g. `courtney-portfolio`) and upload the whole folder: `index.html`, `assets/`, `README.md`. The GitHub web uploader is fine — drag the files in.
2. Go to [vercel.com](https://vercel.com), sign in with GitHub, click **Add New → Project**, and pick the repo.
3. Vercel will ask for framework and build settings. Set:
   - Framework Preset: **Other**
   - Build Command: *(leave empty)*
   - Output Directory: *(leave empty, or `.`)*
4. Click **Deploy**. You get a live `*.vercel.app` URL in under a minute.

**To update the site after that:** edit `index.html` in GitHub (or upload a new copy) and commit. Vercel redeploys automatically within a minute. There is no separate publish step.

Netlify and Cloudflare Pages work the same way if you prefer them.

---

## How to connect a custom domain

1. Buy the domain (Namecheap, Cloudflare, Google Domains — any registrar).
2. In Vercel: your project → **Settings → Domains → Add**, and type the domain.
3. Vercel shows you the DNS records to create. Usually an `A` record for the root domain and a `CNAME` for `www`. Copy them into your registrar's DNS settings.
4. Wait — usually minutes, occasionally a few hours. Vercel issues the HTTPS certificate automatically.
5. Once live, update two things in `index.html` for correct link previews: the `<link rel="canonical">` and `<meta property="og:url">` tags near the top, both currently `[ADD SITE URL]`.

---

## Editing safely

The CONTENT block is JavaScript, so two rules matter:

- Every `{ … }` object in a list needs a comma after it, except the last one.
- Strings need matching quotes. If your text contains a single quote (`don't`), wrap the string in double quotes.

If the page goes blank after an edit, that is almost always one of those two. Open the browser console (F12 → Console) and the error will name the line number.

Keep a copy of the working file before a big edit. `index-backup.html` costs nothing.

---

## Before sending this to a recruiter

- [ ] `site.email`, `site.linkedin`, `site.resumeUrl` filled in
- [ ] Résumé PDF in `assets/`
- [ ] Employment dates added to both jobs
- [ ] Education and certifications filled in, unused rows deleted
- [ ] Real TipOut screenshots in place (at minimum: dashboard, shift entry, earnings)
- [ ] TipOut `accent` set to the real brand colour
- [ ] `social-card.png` added and `[ADD SITE URL]` replaced in the two meta tags
- [ ] Search the file for `[ADD` — nothing should remain that a recruiter would see
- [ ] Open it on your phone and click through all five pages
