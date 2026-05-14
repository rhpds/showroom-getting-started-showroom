# HOW: Getting started with Red Hat Showroom — Implementation Guide

This document contains everything needed to recreate the showroom from scratch or make targeted changes. A fresh Claude Code session should be able to read this file and rebuild the entire showroom.

Read `WHAT.md` first for design context and rationale.

## Prerequisites

### Tools

- **GitHub CLI** (`gh`): https://cli.github.com
- **Git**: https://git-scm.com
- **Podman**: https://podman.io
- **Claude Code**: https://claude.ai/claude-code

### RHDP Skills Marketplace plugin

```
/plugin marketplace add rhpds/rhdp-skills-marketplace
/plugin install showroom@rhdp-marketplace
```

Skills used:
- `/showroom:create-lab` — generates hands-on lab modules
- `/showroom:create-demo` — generates presenter-led demo modules
- `/showroom:verify-content` — validates content against Red Hat quality standards

### Access

- GitHub account with write access to https://github.com/rhpds

## Step 1: Create the repository

```bash
gh repo create rhpds/showroom-getting-started-showroom \
  --template rhpds/showroom_template_nookbag \
  --public \
  --clone

cd showroom-getting-started-showroom
```

Template repo: https://github.com/rhpds/showroom_template_nookbag (GitHub template repo, `is_template: true`)

## Step 2: Clean the template

Delete all template documentation pages from `content/modules/ROOT/pages/`:

```bash
rm content/modules/ROOT/pages/agnosticv-config.adoc
rm content/modules/ROOT/pages/architecture.adoc
rm content/modules/ROOT/pages/attribute-example.adoc
rm content/modules/ROOT/pages/content-repo.adoc
rm content/modules/ROOT/pages/contributing.adoc
rm content/modules/ROOT/pages/deployer.adoc
rm content/modules/ROOT/pages/images.adoc
rm content/modules/ROOT/pages/index.adoc
rm content/modules/ROOT/pages/nookbag.adoc
rm content/modules/ROOT/pages/ocp-integration.adoc
rm content/modules/ROOT/pages/ocp4-role-reference.adoc
rm content/modules/ROOT/pages/quick-start.adoc
rm content/modules/ROOT/pages/ui-config.adoc
rm content/modules/ROOT/pages/user-data.adoc
rm content/modules/ROOT/pages/vm-role-reference.adoc
```

Create required directories:

```bash
mkdir -p content/modules/ROOT/assets/images
mkdir -p content/modules/ROOT/partials
```

**Do NOT delete** the `examples/` directory — the AI skills use it as reference.

## Step 3: Configure the repository

### site.yml

```yaml
---
site:
  title: Getting started with Red Hat Showroom
  url: https://github.com/rhpds/showroom-getting-started-showroom
  start_page: modules::index.adoc

content:
  sources:
    - url: .
      start_path: content

ui:
  bundle:
    url: https://github.com/rhpds/rhdp_showroom_theme/releases/download/latest/ui-bundle.zip
    snapshot: true

antora:
  extensions:
    - require: '@sntke/antora-mermaid-extension'
      mermaid_library_url: https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.esm.min.mjs
      script_stem: header-scripts
      mermaid_initialize_options:
        start_on_load: true
    - require: '@andrew-jones/antora-tabs-extension'

output:
  dir: ./www
```

### content/antora.yml

```yaml
name: modules
title: Getting started with Red Hat Showroom
version: ~
nav:
  - modules/ROOT/nav.adoc

asciidoc:
  attributes:
    experimental: true
    page-pagination: true
```

### ui-config.yml

```yaml
---
type: showroom

default_width: 50
persist_url_state: true

view_switcher:
  enabled: true
  default_mode: page

tabs:
  - name: Showroom Docs
    url: 'https://rhpds.github.io/showroom_template_nookbag/'
  - name: Skills Marketplace
    url: 'https://rhpds.github.io/rhdp-skills-marketplace/'
```

### content/modules/ROOT/nav.adoc

```asciidoc
* xref:index.adoc[Home]
* xref:01-overview.adoc[Workshop overview]
* xref:02-details.adoc[Workshop details]

.Modules
* xref:03-module-01-showroom-in-action.adoc[1. Showroom in action]
* xref:04-module-02-create-and-configure.adoc[2. Create and configure]
* xref:05-module-03-generate-lab-content.adoc[3. Generate lab content]
* xref:06-module-04-create-a-demo.adoc[4. Create a demo (optional)]
* xref:07-conclusion.adoc[Conclusion]
```

## Step 4: Create README.adoc

Replace the generic nookbag template README with project-specific content.

**Structure**:

```
= Getting started with Red Hat Showroom

One-paragraph description: 30-45 minute workshop for Red Hat associates,
teaches how to create Showroom workshops and demos using AI-assisted
authoring, local laptop workflow, Widget Server fictional product.

Link to rendered workshop.

== Workshop modules

Table with columns: Module, Topic, Duration, Required?
- Module 1: Showroom in action (5 min, Yes)
- Module 2: Create and configure (10 min, Yes)
- Module 3: Generate lab content (15 min, Yes)
- Module 4: Create a demo (15 min, Optional)

== Specs

Point to docs/specs/WHAT.md (design) and docs/specs/HOW.md (implementation).
Explain spec-driven development: specs are the source of truth,
Claude Code reads them to make changes.

== Local development

Podman preview command (from template):
  podman run --rm --name antora -v $PWD:/antora -p 8080:8080 -i -t ghcr.io/juliaaano/antora-viewer
SELinux note about :z suffix.

== RHDP Showroom resources

- Showroom template documentation link
- Quick-start guide link
- AI content creation tools link (Skills Marketplace)
```

## Step 5: Create content files (AsciiDoc)

All files go in `content/modules/ROOT/pages/`. Each file description below includes the complete content outline — section headings, exercise structure, key content points, and verification criteria. This is detailed enough to regenerate each file faithfully.

### index.adoc

**Title**: `= Getting started with Red Hat Showroom`

**Sections**:
- "A different kind of Showroom" — explain local laptop workflow, NOTE block about RHDP providing embedded terminals
- "What you'll learn" — 6 bullet points: understand Showroom, create repo, configure, generate lab, optionally generate demo, publish
- "Two paths" — labs (Modules 1-3, required) and demos (Module 4, optional) with brief descriptions
- "Who this is for" — Red Hat associates, beginner level, list of roles
- "Prerequisites" — GitHub account with rhpds access, gh CLI, Podman, Claude Code with RHDP Skills Marketplace, terminal
- "Estimated time" — 30 min lab only, 45 min lab + demo
- "Let's get started!" — call to action

### 01-overview.adoc

**Title**: `= Workshop overview`
**navtitle**: `Workshop overview`

**Sections**:
- "Your role and mission" — protagonist is a Red Hat associate, manager's message about Widget Server 3.2 Automated Widget Pipelines, explain Widget Server is fictional, assignment statement
- "Success criteria" — 5 bullet outcomes: working repo, lab module, optional demo module, published site, skills to repeat for real products
- "Why Showroom?" — 4 current pain points (inconsistent demos, fragile setups, no version control, wasted effort), how Showroom solves them
- "What you'll build" — 6 numbered steps matching the module flow, closing line: "replace Widget Server with your real product"

### 02-details.adoc

**Title**: `= Workshop details`
**navtitle**: `Workshop details`

**Sections**:
- "Timing and schedule" — table with 4 modules (Module 4 marked Optional), total 30-45 min
- "What you need on your laptop" — tool list with install links, setup verification command (`gh --version && git --version && podman --version && claude --version`), RHDP Skills Marketplace installation commands
- "Troubleshooting" — 3 common problems: gh auth, GitHub Pages 404, Podman blank page
- "Authors" — workshop name, platform, skills attribution

### 03-module-01-showroom-in-action.adoc

**Title**: `= Module 1: Showroom in action`
**navtitle**: `1. Showroom in action`
**Duration**: 5 min

**Learning objectives**: 4 items — describe Showroom and delivery models, identify content types (labs/demos), view AsciiDoc source, understand content-to-rendered mapping

**Sections**:
- "What is Showroom?" — two delivery models table (RHDP vs GitHub Pages) with columns: Model, What the learner gets, When to use
- "Exercise 1: Explore this Showroom" — navigate sidebar, pagination, observe formatting. Verify: can navigate between pages.
- "Exercise 2: View the source" — open repo on GitHub, navigate to this module's .adoc file, compare rendered vs source, identify AsciiDoc syntax patterns (headings, steps, bullets, code fences, links), look at nav.adoc, site.yml, ui-config.yml. Verify: can identify where content lives and how config works.
- "Exercise 3: Understand the content structure" — directory tree diagram, four key config files with one-line descriptions
- "Two types of Showroom content" — Labs (4 bullets: self-paced, step-by-step, verification, best for workshops) and Demos (4 bullets: presenter-led, Know/Show, presenter notes, best for customer demos)
- "Learning outcomes" — 5 items
- "What's next" — transition to building their own

### 04-module-02-create-and-configure.adoc

**Title**: `= Module 2: Create and configure your Showroom`
**navtitle**: `2. Create and configure`
**Duration**: 10 min

**Learning objectives**: 4 items — create repo from template, understand config files, configure them, prepare for content generation

**Sections**:
- "Getting started" — open terminal, context about Widget Server 3.2, introduce nookbag template
- "Exercise 1: Create your repository" — `gh auth status`, `gh repo create rhpds/my-first-showroom --template rhpds/showroom_template_nookbag --public --clone`, TIP about naming convention, cd into repo. Verify: `git remote -v` shows correct URL.
- "Exercise 2: Clean out the template pages" — start Claude Code, paste multi-step cleanup instruction (exact text in WHAT.md Module 2 section), review changes, exit Claude Code. Verify: empty pages dir, correct title in site.yml, minimal nav.adoc.
- "Exercise 3: Understand what you configured" — explain each file's changes and rationale
- "Learning outcomes" — 4 items, including note about keeping `examples/` intact

### 05-module-03-generate-lab-content.adoc

**Title**: `= Module 3: Generate lab content and publish`
**navtitle**: `3. Generate lab content`
**Duration**: 15 min

**Learning objectives**: 4 items — generate lab with skill, validate, preview, publish

**Sections**:
- "Time to create content" — introduce `/showroom:create-lab` skill and what it does
- "Exercise 1: Generate your lab module" — cd into repo, start Claude Code, run `/showroom:create-lab content/modules/ROOT/pages/ --new`, table of suggested Widget Server answers (7 rows), skill generates 4 files + updates nav. Verify: `ls` shows files, `cat nav.adoc` shows entries.
- "Exercise 2: Validate your content" — run `/showroom:verify-content`, list what it checks (4 bullets), fix issues, exit Claude Code. Verify: checks pass.
- "Exercise 3: Preview locally" — Podman command with `:z` SELinux note, check rendering at localhost:8080, 4 things to verify. Verify: content renders correctly.
- "Exercise 4: Publish to GitHub Pages" — `git add -A && git commit`, `git push`, `gh api` to enable Pages, `gh run list` to monitor. Verify: site loads, title correct, modules in sidebar.
- "What you've accomplished" — summary of what was built, "Your manager asked for a Showroom — the lab is ready."
- "What's next" — continue to Module 4 for demo, or skip to Conclusion
- "Learning outcomes" — 4 items

### 06-module-04-create-a-demo.adoc

**Title**: `= Module 4: Create a presenter-led demo (optional)`
**navtitle**: `4. Create a demo (optional)`
**Duration**: 15 min

**Learning objectives**: 4 items — difference between lab and demo, generate demo with skill, Know/Show structure, add demo to existing repo

**Sections**:
- "Labs vs demos" — comparison table with 5 rows (audience, structure, pacing, content, use case), note about coexistence in one repo
- "Exercise 1: Generate a demo module" — cd into repo, start Claude Code, run `/showroom:create-demo content/modules/ROOT/pages/`, table of suggested answers (7 rows), explain what the skill generates (Know sections, Show sections, presenter notes), exit Claude Code. Verify: `ls` shows new file, `cat nav.adoc` shows entry.
- "Exercise 2: Review the Know/Show structure" — example code blocks showing Know section format (concept + presenter note) and Show section format (demo steps + presenter note), explanation of the pattern. Verify: demo has Know sections, Show sections, presenter notes.
- "Exercise 3: Publish the updated Showroom" — commit, push, monitor deployment. Verify: both lab and demo in sidebar.
- "When to use labs vs demos" — 3 sections: create a lab when (4 bullets), create a demo when (4 bullets), create both when (2 bullets)
- "Learning outcomes" — 4 items

### 07-conclusion.adoc

**Title**: `= Conclusion and next steps`
**navtitle**: `Conclusion`

**Sections**:
- "What you've learned" — 7 bullet summary including conditional "(If you completed Module 4)" item
- "Key takeaways" — 5 numbered items: AsciiDoc in Git, nookbag template, AI skills, two content types one repo, two delivery models one repo
- "From Widget Server to your real product" — 5 numbered steps showing the identical process with a real product, closing: "The only difference is the reference materials you feed the skills."
- "Next steps" — add more modules (re-run skills), deploy on RHDP (link to quick-start), explore advanced AsciiDoc (mermaid, tabs, execute blocks, kbd macros), use other RHDP skills (verify-content, blog-generate, catalog-builder)
- "References" — 3 categories: Showroom documentation (template docs, nookbag repo), RHDP Skills Marketplace (marketplace repo, marketplace docs), AsciiDoc and Antora (AsciiDoc docs, Antora docs)
- "Share your feedback" — 3 questions, Slack link to #forum-demo-developers
- "Thank you!" — closing

## Step 6: Publish

```bash
git add -A
git commit -m "Initial showroom content"
git push origin main

# Enable GitHub Pages (one-time)
gh api repos/rhpds/showroom-getting-started-showroom/pages \
  -X POST -f build_type=workflow
```

Monitor: `gh run list --repo rhpds/showroom-getting-started-showroom --limit 1`

Published at: https://rhpds.github.io/showroom-getting-started-showroom/

## Verification checklist

- [ ] All 8 .adoc files exist in `content/modules/ROOT/pages/`
- [ ] `nav.adoc` has entries for all 8 pages in correct order
- [ ] No references to "Meridian Solutions" in any file
- [ ] Widget Server story is consistent across overview, module 2 (Claude Code instructions), module 3 (skill answers), module 4 (demo answers)
- [ ] GitHub Pages build passes (check Actions tab)
- [ ] Published site loads at https://rhpds.github.io/showroom-getting-started-showroom/
- [ ] Navigation sidebar shows all pages
- [ ] Pagination (Next/Previous) works between pages
- [ ] `docs/specs/` directory exists with WHAT.md and HOW.md (not published to the site — outside content/ path)

## How to modify

### Change the story arc

1. Read WHAT.md "Story arc" section for current story
2. Update `01-overview.adoc` — protagonist, product name, feature name, manager's ask
3. Update `04-module-02-create-and-configure.adoc` — Claude Code cleanup instruction (the quoted block learners paste)
4. Update `05-module-03-generate-lab-content.adoc` — skill answers table
5. Update `06-module-04-create-a-demo.adoc` — skill answers table and Know/Show examples
6. Update `07-conclusion.adoc` — "From X to your real product" section
7. Grep for the old product name to catch any remaining references
8. Update WHAT.md to reflect the new story

### Change the audience

1. Update `index.adoc` — "Who this is for" section and prerequisites
2. Update `01-overview.adoc` — protagonist description
3. Update `02-details.adoc` — tool requirements if they change
4. Update WHAT.md "Audience" section

### Add a new module

1. Create `0X-module-0Y-slug.adoc` with next sequential number
2. Follow the module structure pattern (see below)
3. Add entry to `nav.adoc` in correct position
4. Renumber the conclusion file if needed (currently `07-conclusion.adoc`)
5. Update `02-details.adoc` timing table
6. Update `index.adoc` estimated time
7. Update WHAT.md content architecture and module designs sections
8. Update HOW.md content files section

### Module structure pattern

Every module follows this template:

```asciidoc
= Module N: Title
:navtitle: N. Short title

== Learning objectives

* Objective 1
* Objective 2
* Objective 3
* Objective 4

== Context paragraph

Brief intro tying to the story arc.

== Exercise 1: Name

. Step 1
. Step 2
. Step 3

=== Verify

Verification criteria.

== Exercise 2: Name

...

== Learning outcomes

* Outcome 1
* Outcome 2
* Outcome 3
* Outcome 4

== What's next

Transition to next module.
```

### Update the README

1. Read WHAT.md "README" section for what should and should not be in the README
2. Update `README.adoc` to reflect any changes to modules, audience, description, or links
3. Ensure the module list matches `nav.adoc` and `02-details.adoc` timing table
4. Keep the Podman local preview command and RHDP Showroom resources links

### Change prerequisites

1. Update `index.adoc` — "Prerequisites" section
2. Update `02-details.adoc` — "What you need on your laptop" section and verification command
3. Update HOW.md "Prerequisites" section

### Change the theme

1. Update `site.yml` — change `ui.bundle.url` to a different release tag from https://github.com/rhpds/rhdp_showroom_theme/releases
2. Available themes: `rh-one-2025`, `rh-summit-2025`, `latest`

## Dependencies

| Dependency | URL | Purpose |
|-----------|-----|---------|
| Nookbag template | https://github.com/rhpds/showroom_template_nookbag | GitHub template repo for `gh repo create --template` |
| RHDP Skills Marketplace | https://github.com/rhpds/rhdp-skills-marketplace | Claude Code plugin with content generation skills |
| Skills Marketplace docs | https://rhpds.github.io/rhdp-skills-marketplace/ | Reference tab in ui-config.yml |
| Showroom template docs | https://rhpds.github.io/showroom_template_nookbag/ | Reference tab in ui-config.yml |
| Showroom theme | https://github.com/rhpds/rhdp_showroom_theme | Antora UI bundle in site.yml |
| Antora viewer | ghcr.io/juliaaano/antora-viewer | Container image for local preview |
| GitHub Pages workflow | `.github/workflows/gh-pages.yml` (from template) | Auto-deploys on push to main |
