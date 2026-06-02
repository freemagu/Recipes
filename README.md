# Recipes

A collection of recipe notes in a single, consistent Markdown format that is
readable by humans (in Obsidian or any Markdown app) and easy for an app to
parse — especially the nutrition **macros**, which live in machine-readable
YAML frontmatter.

Every recipe is one `.md` file in the repo root.

## File naming

- One recipe per file, named with underscores instead of spaces, e.g.
  `Akis_Moroccan_Chicken.md`, `High-Protein_Pancakes.md`.
- The human-facing name lives in the `title` field; the machine key lives in
  `id` (see below). The filename is not parsed for data.

## File structure

```markdown
---
<frontmatter — see schema below>
---

# <title>

> <one-line description>

## Nutrition

| Macro    | Whole recipe | Per serving (÷N) |
| -------- | ------------ | ---------------- |
| Calories | ...          | ...              |
| Protein  | ... g        | ... g            |
| Fat      | ... g        | ... g            |
| Carbs    | ... g        | ... g            |
| Sugar    | ... g        | ... g            |

## Ingredients

- [ ] <item>

## Method

- [ ] 1 — <step>

## Notes

- <free-form notes>
```

## Frontmatter schema

### Required fields (present on every recipe)

| Field            | Type           | Notes |
| ---------------- | -------------- | ----- |
| `id`             | string (slug)  | Stable, lowercase, hyphenated identifier derived from the title (e.g. `akis-moroccan-chicken`). Intended as the app's primary key. |
| `title`          | string (quoted)| Display name. No leading emoji. |
| `diet_type`      | string         | One of: `animal-based`, `carnivore`, `dessert`, `drink`, `high-calorie`, `keto`, `low-carb`, `other`, `paleo`. |
| `primary_protein`| string         | Main protein source, e.g. `chicken`, `ground beef + pork`, `none`. |
| `servings`       | integer        | Number of servings the macros are divided into for the per-serving column. |
| `calories`       | integer        | **Whole-recipe** total calories (kcal). |
| `protein_g`      | integer        | **Whole-recipe** total protein in grams. |
| `fat_g`          | integer        | **Whole-recipe** total fat in grams. |
| `carbs_g`        | integer        | **Whole-recipe** total carbohydrate in grams. |
| `sugar_g`        | integer        | **Whole-recipe** total sugar in grams. |
| `macros_basis`   | string         | Always `total` — declares that the macro fields above are for the whole recipe, not per serving. |
| `tags`           | list of strings| Flow-style list, e.g. `[dinner, batch-cook, high-protein]`. |

### Optional fields (present only when known)

| Field            | Type    | Notes |
| ---------------- | ------- | ----- |
| `cooking_method` | string  | e.g. `oven`, `air fryer`, `BBQ`, `pan / oven / air fryer`. |
| `prep_time_min`  | integer | Prep time in minutes. |
| `cook_time_min`  | integer | Cook time in minutes. |
| `freeze_time_min`| integer | Freeze/set time in minutes (frozen desserts). |
| `source`         | string  | Provenance, e.g. `bear`. Omitted when unknown. |

### Example

```yaml
---
id: akis-moroccan-chicken
title: "Aki's Moroccan Chicken"
diet_type: paleo
primary_protein: chicken
servings: 6
calories: 4140
protein_g: 300
fat_g: 270
carbs_g: 174
sugar_g: 34
macros_basis: total
tags: [dinner, batch-cook]
source: bear
---
```

## Macros: how to consume them

- The five macro fields (`calories`, `protein_g`, `fat_g`, `carbs_g`,
  `sugar_g`) are **flat top-level keys** so an app can read them directly,
  e.g. `data.protein_g`.
- They are **whole-recipe totals** (`macros_basis: total`).
- **Per-serving = total ÷ `servings`** (rounded to the nearest whole number).
  The per-serving values shown in the `## Nutrition` table are derived this
  way and are for human display only — apps should compute from the
  frontmatter totals and `servings` to avoid rounding drift.

## Body conventions

- **Description** — a single blockquote (`>`) line under the title summarising
  the dish.
- **`## Nutrition`** — a table showing whole-recipe totals and the derived
  per-serving column. Human-facing; the source of truth is the frontmatter.
- **`## Ingredients`** — Markdown task-list checkboxes (`- [ ] item`). May be
  grouped under `###` sub-headings (e.g. `### Spice Mix`, `### Custard Base`).
- **`## Method`** — task-list checkboxes with the step number kept as text and
  an em dash: `- [ ] 1 — Do the thing.` (A plain `N. ` after the checkbox is
  avoided because some Markdown parsers misread it as a nested ordered list.)
  May be grouped under numbered `###` sub-headings.
- **`## Notes`** — free-form bullets (not checkboxes); optional tips, yields,
  storage, variations.

## Checkboxes

Ingredients and Method use Markdown task lists so they can be ticked off while
cooking in Obsidian or any Markdown app. Use `- [ ]` for unchecked and `- [x]`
for checked. Checking items does not affect the macros (those come from
frontmatter).
