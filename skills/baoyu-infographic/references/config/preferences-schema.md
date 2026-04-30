---
name: preferences-schema
description: EXTEND.md YAML schema for baoyu-infographic user preferences
---

# Preferences Schema

## Full Schema

```yaml
---
version: 1

preferred_layout: null    # any of the 12 layouts (see Layout Gallery in SKILL.md) or null
preferred_style: null     # any of the 9 styles (see Style Gallery in SKILL.md) or null
preferred_aspect: null    # landscape|portrait|square|null  (custom W:H also accepted)

language: null            # zh|en|ja|ko|null (null = auto-detect from source)

preferred_image_backend: auto   # auto|ask|<backend-id>
image_model: gpt-image-2        # default image model passed to backend's model param

custom_styles:            # extra style definitions merged with the built-ins
  - name: my-brand
    description: "Short description shown in Step 3 recommendations"
    prompt_fragment: "Style traits to inject into Step 5 prompt"
---
```

## Field Reference

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `version` | int | 1 | Schema version |
| `preferred_layout` | string\|null | null | Pre-selected layout — surfaces as the top recommendation in Step 3 |
| `preferred_style` | string\|null | null | Pre-selected style — surfaces as the top recommendation in Step 3 |
| `preferred_aspect` | string\|null | null | Default aspect for Step 4 (named preset or W:H string) |
| `language` | string\|null | null | Output language (null = auto-detect from source content) |
| `preferred_image_backend` | string | `auto` | Image backend selection. `auto` = prefer runtime-native tool, fall back to the only installed backend, ask if multiple non-native are present. `ask` = always confirm on every run. `<backend-id>` (e.g., `codex-imagegen`, `baoyu-imagine`, `image_generate`) = pin this backend when available; fall back to `auto` when it isn't. Absent = `auto`. |
| `image_model` | string | `gpt-image-2` | Default image model id passed to the backend's `model` parameter when the backend supports explicit model selection (e.g., Codex `imagegen`, OpenAI/Azure image API, DashScope). If the backend does not accept a `model` arg, this field is ignored — do NOT silently fall back to a different model. |
| `custom_styles` | array | [] | Additional styles available alongside the built-ins |

Backend resolution logic is documented in the `## Image Generation Tools` section of `SKILL.md`. This doc only defines the field.

All fields in this schema are defaults only — they shape Step-3 recommendations and Step-4 defaults but never bypass Step 4 confirmation (see the `## Confirmation Policy` section in SKILL.md).

Example backend ids:

| Value | Meaning |
|-------|---------|
| `codex-imagegen` | Codex built-in `imagegen` tool |
| `baoyu-imagine` | `baoyu-imagine` skill / script backend |
| `image_generate` | Generic runtime image tool such as Hermes |

## Layout Options

See the **Layout Gallery (12)** table in `SKILL.md` for the canonical list. Common picks:

| Value | Best For |
|-------|----------|
| `bento-grid` | General default — overview, multiple topics |
| `structural-breakdown` | Code/system architecture walkthrough |
| `dashboard` | Project status, KPIs |
| `linear-progression` | Timelines, processes, SOPs |
| `dense-modules` | High-density modules, data-rich guides |
| `hub-spoke` | Concept evangelism with related items |

## Style Options

See the **Style Gallery (9)** table in `SKILL.md` for the canonical list. Common picks:

| Value | Description |
|-------|-------------|
| `corporate-memphis` | Flat vector, vibrant (default — safest for management reports) |
| `technical-schematic` | Blueprint, engineering — code/architecture |
| `pop-laboratory` | Blueprint grid, lab precision — engineer-friendly |
| `ikea-manual` | Minimal line art — SOPs, specs |
| `chalkboard` | Classroom feel — internal tech sharing |

## Aspect Options

| Value | Ratio | Notes |
|-------|-------|-------|
| `landscape` | 16:9 | Slides, blog headers, web banners |
| `portrait` | 9:16 | Mobile, social, dense modules (default for `dense-modules`) |
| `square` | 1:1 | Social posts, thumbnails |
| Custom W:H | e.g. `3:4`, `4:3`, `2.35:1` | Pass through verbatim to the prompt |

## Custom Style Fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Unique style identifier (kebab-case) |
| `description` | Yes | One-line description shown in Step 3 recommendations |
| `prompt_fragment` | Yes | Style traits appended into the Step 5 prompt body |

## Example: Minimal Preferences

```yaml
---
version: 1
preferred_layout: bento-grid
preferred_style: corporate-memphis
language: zh
---
```

`preferred_image_backend` is omitted above; absence is treated as `auto`. `image_model` is also omitted; absence is treated as `gpt-image-2`.

## Example: Full Preferences

```yaml
---
version: 1

preferred_layout: dense-modules
preferred_style: pop-laboratory
preferred_aspect: portrait

language: zh

preferred_image_backend: codex-imagegen
image_model: gpt-image-2

custom_styles:
  - name: my-brand
    description: "Brand-aligned tech-management infographic"
    prompt_fragment: "Use brand palette (#0F172A, #2563EB, #F97316); rounded rectangles; clean sans-serif typography; ample whitespace; engineer-friendly tone."
---
```
