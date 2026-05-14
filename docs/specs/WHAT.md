# WHAT: Getting started with Red Hat Showroom — Design Specification

## Overview

A 30-45 minute hands-on showroom that teaches Red Hat associates how to create their own Showroom workshops and demos. This is the only showroom that teaches showroom creation — a "meta-showroom" where the output of the lab is itself a working showroom.

- **Published site**: https://rhpds.github.io/showroom-getting-started-showroom/
- **Repository**: https://github.com/rhpds/showroom-getting-started-showroom
- **Template source**: https://github.com/rhpds/showroom_template_nookbag

## Audience

- **Who**: Red Hat associates only — solution architects, technical marketing managers, product managers, developer advocates, SSAs, and anyone who needs to create workshops or demos for Red Hat products.
- **Experience level**: Beginner. No prior AsciiDoc, Antora, or Showroom experience required.
- **Assumed skills**: Basic terminal usage, a GitHub account with `rhpds` org access, Claude Code installed.

## Delivery model

This showroom uses a **local laptop workflow**, which is different from typical RHDP-hosted showrooms:

- Learners read instructions in their browser (via GitHub Pages) and run commands in their own local terminal.
- There is no embedded Wetty terminal, no split-pane with execute blocks sending commands to a terminal.
- Code blocks use `[source,bash]` (copy-to-clipboard) instead of `[source,role="execute"]` (send-to-terminal).
- The `ui-config.yml` sets `default_mode: page` with reference documentation tabs instead of a Terminal tab.

**Why this model**: Creating showroom content is inherently local work — cloning repos, editing files, running Claude Code, previewing with Podman, and pushing to GitHub. An embedded terminal would add deployment complexity without matching the actual authoring workflow.

**Note for learners**: The index page explains this difference upfront and includes a NOTE block explaining that RHDP-deployed showrooms provide the full split-pane experience with embedded terminals.

## Story arc

### Fictional product

- **Product**: Red Hat Widget Server (fictional — deliberately generic)
- **Version**: 3.2
- **New feature**: Automated Widget Pipelines — a capability that lets users define, test, and deploy widget configurations through a declarative pipeline.
- **Why fictional**: "Widget Server" is obviously not a real product, so nobody will confuse the lab content with actual product documentation. The feature name is specific enough to generate realistic content but generic enough to map onto any real product feature (OpenShift Pipelines, AAP workflows, RHEL Image Builder, etc.).

### Protagonist and scenario

- **Protagonist**: A Red Hat associate. Role is explicitly left open — "Maybe you're a solution architect, a technical marketing manager, a product manager, or a developer advocate — your exact role doesn't matter."
- **The ask**: Manager says: "The Widget Server team just shipped Automated Widget Pipelines in version 3.2. We need a Showroom so the field team can demo the new feature to customers. Can you build one this week?"
- **Why this story works**: Every Red Hat associate has been in the "we need a demo for the new feature" situation. It naturally motivates every module.

### Story thread across modules

- **Module 1**: Understand what a Showroom is (explore this very lab as an example)
- **Module 2**: Create the Widget Server showroom repo and configure it
- **Module 3**: Generate a lab module for Automated Widget Pipelines using `/showroom:create-lab`
- **Module 4** (optional): Generate a presenter-led demo module using `/showroom:create-demo`
- **Conclusion**: "Replace Widget Server with your real product — the process is identical"

## Content architecture

### File list

| Path | Title | Purpose | Duration |
|------|-------|---------|----------|
| `index.adoc` | Getting started with Red Hat Showroom | Landing page: what you'll learn, two paths, prerequisites, estimated time | — |
| `01-overview.adoc` | Workshop overview | Widget Server story, success criteria, why Showroom, what you'll build | 2 min |
| `02-details.adoc` | Workshop details | Timing table, tool requirements, setup verification, troubleshooting | 2 min |
| `03-module-01-showroom-in-action.adoc` | Module 1: Showroom in action | Explore this showroom, view source on GitHub, understand directory structure, learn about labs vs demos | 5 min |
| `04-module-02-create-and-configure.adoc` | Module 2: Create and configure your Showroom | Create repo from nookbag template, clean template pages with Claude Code, configure site.yml/antora.yml/ui-config.yml | 10 min |
| `05-module-03-generate-lab-content.adoc` | Module 3: Generate lab content and publish | Run `/showroom:create-lab`, verify with `/showroom:verify-content`, preview with Podman, publish to GitHub Pages | 15 min |
| `06-module-04-create-a-demo.adoc` | Module 4: Create a presenter-led demo (optional) | Labs vs demos comparison, run `/showroom:create-demo`, review Know/Show structure, publish | 15 min |
| `07-conclusion.adoc` | Conclusion and next steps | What you learned, key takeaways, Widget Server to real product, next steps, references | — |

### Navigation structure

```
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

### Two paths

- **Lab path** (Modules 1-3, required): ~30 minutes. Learner creates a showroom repo, generates a lab module, and publishes.
- **Demo path** (Module 4, optional): ~15 additional minutes. Learner adds a presenter-led demo module to the same repo.

## Module designs

### Module 1: Showroom in action (5 min, required)

**Learning objectives**:
- Describe what a Showroom is and the two delivery models (RHDP and GitHub Pages)
- Identify the two content types: hands-on labs and presenter-led demos
- View the AsciiDoc source behind a rendered Showroom page
- Understand how content maps from source files to what learners see

**Exercises**:
1. **Explore this Showroom** — navigate sidebar, pagination, observe content formatting
2. **View the source** — open this repo on GitHub, find the AsciiDoc file for this module, compare rendered vs source, identify nav.adoc, site.yml, ui-config.yml
3. **Understand the content structure** — directory tree diagram, four key config files explained

**Additional content**: "Two types of Showroom content" section explaining labs vs demos with bullet-point descriptions of each

**Verification**: Learner can identify where content lives, how navigation is defined, how the site is configured

### Module 2: Create and configure your Showroom (10 min, required)

**Learning objectives**:
- Create a Showroom repository from the official nookbag template
- Understand the purpose of each key configuration file
- Configure site.yml, antora.yml, and ui-config.yml for your workshop
- Prepare the repository for content generation

**Exercises**:
1. **Create your repository** — `gh auth status`, `gh repo create rhpds/my-first-showroom --template rhpds/showroom_template_nookbag --public --clone`, cd into repo. TIP about naming convention.
2. **Clean out the template pages** — start Claude Code, give it a multi-step instruction to delete template pages, create dirs, update config files with Widget Server title, exit Claude Code
3. **Understand what you configured** — review what each file does and why it was changed

**Claude Code instruction used in Exercise 2** (the learner pastes this):
```
Clean this Showroom repo for a new workshop called "Widget Server automated pipelines":
1. Delete all .adoc files in content/modules/ROOT/pages/
2. Create directories: content/modules/ROOT/assets/images and content/modules/ROOT/partials
3. Update site.yml: change title to "Widget Server automated pipelines" and url to this repo's GitHub URL
4. Update content/antora.yml: set title to "Widget Server automated pipelines", remove all example attributes (guid, bastion_*, ssh_*, openshift_*), keep experimental and page-pagination
5. Update ui-config.yml: replace the Antora tab with a Terminal tab (name: Terminal, path: /wetty, port: 443)
6. Replace content/modules/ROOT/nav.adoc contents with just: * xref:index.adoc[Home]
```

**Verification**: Empty pages directory, correct title in site.yml, minimal nav.adoc

### Module 3: Generate lab content and publish (15 min, required)

**Learning objectives**:
- Generate a complete hands-on lab module using `/showroom:create-lab`
- Validate content quality with `/showroom:verify-content`
- Preview your Showroom locally using Podman
- Publish your Showroom to GitHub Pages

**Exercises**:
1. **Generate your lab module** — cd into repo, start Claude Code, run `/showroom:create-lab content/modules/ROOT/pages/ --new`, answer skill questions using Widget Server answers (table of suggested answers provided)
2. **Validate your content** — run `/showroom:verify-content`, fix issues
3. **Preview locally** — `podman run --rm --name antora -v $PWD:/antora:z -p 8080:8080 -i -t ghcr.io/juliaaano/antora-viewer`, check rendering at localhost:8080
4. **Publish to GitHub Pages** — `git add -A && git commit`, `git push`, `gh api repos/rhpds/my-first-showroom/pages -X POST -f build_type=workflow`, monitor with `gh run list`

**Skill answer table**:

| Question | Suggested answer |
|----------|-----------------|
| Workshop name | Widget Server automated pipelines |
| Learning goal | Learn to define, test, and deploy widget configurations using Automated Widget Pipelines |
| Target audience | Developers and platform engineers, beginner level |
| Learning outcomes | Create a widget pipeline, configure pipeline stages, test and deploy a widget configuration |
| Business scenario | Your team needs to automate widget deployments that are currently done manually, causing delays and inconsistencies |
| Duration | 30 minutes |
| Technical environment | Widget Server 3.2 (use version placeholders), single module |

**Verification**: Files generated, nav updated, verification passes, local preview renders, GitHub Pages site loads

### Module 4: Create a presenter-led demo (15 min, optional)

**Learning objectives**:
- Understand the difference between lab content and demo content
- Generate a presenter-led demo module using `/showroom:create-demo`
- Explain the Know/Show structure used in demos
- Add a demo module to an existing Showroom repository

**Exercises**:
1. **Generate a demo module** — cd into repo, start Claude Code, run `/showroom:create-demo content/modules/ROOT/pages/`, answer skill questions using Widget Server demo answers (table provided)
2. **Review the Know/Show structure** — open generated file, identify Know sections (concept + talking points) and Show sections (live demo steps + presenter notes)
3. **Publish the updated Showroom** — commit, push, verify both lab and demo appear in navigation

**Additional content**: Labs vs demos comparison table (audience, structure, pacing, content, use case), "When to use labs vs demos" decision guide

**Verification**: Demo file exists, nav includes it, Know/Show sections present, site renders with both lab and demo in sidebar

## Configuration

### site.yml
```yaml
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

**Rationale**: Title and URL are the only fields changed from the nookbag template defaults. Extensions (mermaid, tabs) are kept for future use even though current content doesn't use them.

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

**Rationale**: All example attributes (guid, bastion_*, openshift_*) removed — this showroom has no environment-specific variables. `experimental` enables keyboard/button macros. `page-pagination` adds Next/Previous links.

### ui-config.yml
```yaml
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

**Rationale**: `default_mode: page` because there is no terminal — content fills the full width. Tabs are reference documentation links, not interactive tools. `default_width: 50` gives equal space if someone toggles split view.

## README

The `README.adoc` is the first thing people see on GitHub. It should orient visitors to the project, link to the rendered workshop, and provide quick-start instructions for contributors.

### What the README should contain

1. **Project title and description** — what this showroom is and who it's for (one paragraph)
2. **Rendered workshop link** — direct URL to the published GitHub Pages site
3. **Workshop modules** — list of all modules with one-line descriptions, durations, and required/optional flag
4. **Specs reference** — point to `docs/specs/WHAT.md` and `docs/specs/HOW.md` for spec-driven development
5. **Local development** — Podman command for local preview (carried over from the nookbag template)
6. **RHDP Showroom resources** — links to template docs, quick-start guide, and AI content creation tools (carried over from the nookbag template)

### What NOT to include

- The template's generic tagline ("0% cruft", "Showroom Template")
- The "create a repo from this template" link (this IS the repo, not a template)
- Instructions for creating a new showroom (that's what the workshop itself teaches)

### What to carry over from the nookbag template README

- The Podman local preview command with SELinux `:z` note
- The link to https://rhpds.github.io/showroom_template_nookbag/ (full Showroom documentation)
- The link to RHDP Skills Marketplace and AI content creation documentation

## AsciiDoc conventions

- **Code blocks**: Use `[source,bash]` for commands learners run in their terminal. Never use `[source,role="execute"]` — there is no embedded terminal to send to.
- **External links**: Always use `^` caret to open in new tab: `link:https://example.com[text^]`
- **Internal links**: Use `xref:` without caret: `xref:04-module-02.adoc[text]`
- **Numbered steps**: Use `.` prefix for sequential exercise steps
- **Bullet points**: Use `*` prefix for information lists, learning objectives, verification criteria
- **Headings**: Sentence case ("Create your repository" not "Create Your Repository")
- **No em dashes**: Use commas, periods, or `--` instead of `—`
- **Admonitions**: `[NOTE]`, `[TIP]`, `[IMPORTANT]` blocks for callouts
- **Tables**: `[cols="1,2",options="header"]` for structured comparisons
- **Verification sections**: `=== Verify` subsection after each exercise

## Design decisions

| Decision | Choice | Reasoning |
|----------|--------|-----------|
| Delivery model | Local laptop + GitHub Pages | Creating showrooms is local work; an embedded terminal adds complexity without matching the authoring workflow |
| Story product | Fictional "Widget Server" | Product-agnostic; obviously fictional so no confusion with real docs; feature name ("Automated Widget Pipelines") is specific enough to generate realistic content |
| Content creation | AI skill-first (`/showroom:create-lab`) | Everyone in the audience has Claude Code and the skills; manual AsciiDoc authoring is unnecessary friction |
| Lab vs demo path | Sequential: lab required, demo optional | Lab teaches the core workflow; demo is additive and uses a different skill (`/showroom:create-demo`) |
| Execute blocks | Not used | No embedded terminal in GitHub Pages model; code blocks are copy-to-clipboard only |
| Module count | 4 (3 required + 1 optional) | Keeps total under 30 min for lab path, 45 min for lab+demo; each module has a single focused outcome |
| Inception handling | Module 1 uses this showroom as the live example | "This lab" vs "your Showroom" terminology; Module 2 has IMPORTANT callout at Module 1 end marking the transition |

## Out of scope

- AgnosticV catalog item creation and RHDP deployment
- Multi-user showroom setup with keycloak/htpasswd
- Advanced AsciiDoc features (mermaid diagrams, tabs extension, nested includes)
- FTL (Full Test Lifecycle) solve/validate playbooks
- Writing AsciiDoc by hand (skill-first approach)
- Content for real Red Hat products (Widget Server is deliberately fictional)
- Showroom platform development (architecture, deployer, nookbag UI)
