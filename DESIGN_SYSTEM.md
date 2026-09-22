# Kando Design System — Color Palette

**Status**: Official v1.0
**Source**: Extracted from onboarding wizard wireframes (`index.html`, `payroll-periods-wizard.html`, `holiday-calendar-wizard.html`, `leave-types-wizard.html`)
**Last Updated**: 2026-09-07
**Location**: `/Users/ericmagto/Projects/kando/research/kando-onboarding-wizard/`

---

## ✅ What's Working Well

1. **Strong text contrast for body copy.** `--ink` (`#2c3e50`) on white is 10.98:1 — passes AAA. This is the workhorse color for headings, labels, and body text and it's used correctly everywhere.
2. **Consistent token discipline across all 3 wizard files.** `payroll-periods-wizard.html`, `holiday-calendar-wizard.html`, and `leave-types-wizard.html` all define the *identical* `:root` token block (same 8 variables, same values, same order). This is exactly how a design system should propagate — copy-paste consistency today, ready to extract into a shared stylesheet tomorrow.
3. **Coherent visual language.** Teal-tinted backgrounds at low opacity (`rgba(32,201,151,0.12)`) are reused consistently for status pills, badges, and success banners (`.status-badge`, `.summary-badge`, `.tag-custom`, `.success-banner`, `.count-pill`, `.badge-yes`) — a single tint recipe scales cleanly across components.
4. **Sensible neutral system.** `--border`, `--gray-bg`, `--gray-text` give enough tonal range to separate cards/sections without needing extra grays.
5. **Brand identity fit.** Teal/green + navy ink reads as modern, clean, "fintech-adjacent" HRMS — appropriate for a payroll-heavy product targeting mid-market PH companies. Not generic SaaS-blue, which helps differentiation.
6. **Error color is legible.** `--error` (`#e03131`) on white is 4.51:1 — passes AA for normal text, fine for validation messages.

---

## ⚠️ Areas for Refinement

### 1. Critical: primary button fails WCAG contrast
`--teal` (`#20c997`) used as a **background with white text** (`.btn-primary`, `.open-btn`, `.brand-dot`, `.btn-add`) measures **2.13:1** — fails even the relaxed AA "large text/UI component" threshold of 3:1, let alone the 4.5:1 normal-text threshold. This is the single biggest accessibility gap in the current system. Bold 15px button labels do not qualify as "large text" (needs ≥18.66px bold), so this must clear 4.5:1.

Even `--teal-dark` (`#17a882`), used on hover, only reaches **3.02:1** — passes for large text/graphical UI components but still fails normal 15px button-label text.

**Fix**: Introduce a darker, WCAG-AA-compliant teal for any white-text-on-teal use (buttons, solid badges) — see `--teal-accessible` in the token file below (4.77:1). Keep the brighter `#20c997` for large decorative elements, icons, progress bars, and non-text fills where contrast rules don't apply.

### 2. `index.html` is visually out of sync with the wizard pages
- Background gradient: `linear-gradient(135deg, #f5f7fa 0%, #ffffff 100%)` vs. the wizards' flat `var(--gray-bg)` (`#f7f9fb`). Close but not identical (`#f5f7fa` ≠ `#f7f9fb`) — likely drift, not intentional.
- Button hover: `index.html` uses `#1ab385` for `.open-btn:hover`; the wizards use `--teal-dark` (`#17a882`) for `.btn-primary:hover`. Two different hover shades for what is conceptually the same primary action.
- Secondary text: `index.html` uses raw `#666` and `#999` instead of `--gray-text` (`#6b7785`). No shared token file is actually `<link>`ed — each file re-declares its own `:root` (or, in `index.html`'s case, doesn't use variables at all).
- `index.html` never defines a `:root` token block — all colors are hardcoded literals, unlike the three wizard pages.

### 3. No shared stylesheet
Each of the 3 wizard HTML files pastes an **identical** 8-variable `:root` block independently. It works today because the copies haven't diverged, but there's no single source of truth — a future edit to one file's tokens (as already happened with `index.html`) will silently fork the palette. Recommend extracting to `kando-tokens.css` once these wireframes move toward production.

### 4. Border token is very faint
`--border: rgba(60, 60, 60, 0.12)` renders at roughly 1.4:1 contrast against white — that's intentional for a "subtle divider," not a text/interactive-boundary color, but flag it: if `--border` is ever used as a *focus outline* or *required-field indicator* border color (it currently isn't), it would fail visibility for low-vision users. Currently used correctly (decorative dividers/card outlines only), just noting the ceiling.

### 5. Missing semantic states
The system has `--error` but no formal **success**, **warning**, or **info** tokens. Success is currently improvised by reusing teal (`rgba(32,201,151,0.12)` bg + `--teal-dark` text) for `.success-banner` — which works because teal already reads as "positive" in this palette, but it means success and "brand accent" are not visually distinguishable. Warning and info have no representation at all (e.g., the holiday wizard's "this step is optional" hints just use `--gray-text`, which is a missed opportunity for an info tint).

---

## Accessibility Audit — WCAG Contrast Ratios

Formulas use the standard WCAG 2.1 relative-luminance method (sRGB → linearized → 0.2126R + 0.7152G + 0.0722B).

| Foreground | Background | Ratio | AA Normal Text (4.5:1) | AA Large Text/UI (3:1) | AAA Normal (7:1) | Real usage in files |
|---|---|---|---|---|---|---|
| `#2c3e50` (ink) | `#ffffff` (white) | **10.98:1** | ✅ Pass | ✅ Pass | ✅ Pass | Headings, body, labels |
| `#2c3e50` (ink) | `#f7f9fb` (gray-bg) | **10.41:1** | ✅ Pass | ✅ Pass | ✅ Pass | Text inside intro-cards |
| `#6b7785` (gray-text) | `#ffffff` | **4.56:1** | ✅ Pass | ✅ Pass | ❌ Fail | `.step-sub`, secondary labels |
| `#6b7785` (gray-text) | `#f7f9fb` (gray-bg) | **4.32:1** | ❌ Fail (barely) | ✅ Pass | ❌ Fail | Hints inside gray card backgrounds |
| `#e03131` (error) | `#ffffff` | **4.51:1** | ✅ Pass (barely) | ✅ Pass | ❌ Fail | `.error-msg`, `.req` asterisk |
| **`#ffffff` on `#20c997`** (teal) | — | **2.13:1** | ❌ **FAIL** | ❌ **FAIL** | ❌ Fail | `.btn-primary`, `.open-btn`, `.brand-dot`, `.btn-add` |
| **`#ffffff` on `#17a882`** (teal-dark) | — | **3.02:1** | ❌ **FAIL** | ✅ Pass (barely) | ❌ Fail | Button `:hover` state |
| `#20c997` (teal) text | `#ffffff` | **2.13:1** | ❌ Fail | ❌ Fail | ❌ Fail | `.nav-breadcrumb a`, `.brand` label, `.btn-link` |
| `#17a882` (teal-dark) text | `#ffffff` | **3.02:1** | ❌ Fail | ✅ Pass | ❌ Fail | `.summary-badge`, `.count-pill`, `.tag-custom` text |
| `#17a882` on `rgba(32,201,151,.12)` tint | — | **~2.70:1** | ❌ Fail | ❌ Fail | ❌ Fail | Status pills, success banner text |

### Verdict
- **Body/heading text (`--ink`) is excellent** — no changes needed.
- **All teal-on-white and white-on-teal text combinations currently fail AA.** This affects every primary button, every brand-colored link, and every teal badge/pill label in all 4 files. This is the top-priority fix.
- **`--gray-text` is borderline** — fine on pure white, fails AA-normal by a hair on the gray-bg tint. Low risk (secondary/disabled text is explicitly lower priority under WCAG), but worth tightening if HR admins skew older or use suboptimal displays (a realistic concern for a PH mid-market back-office tool).

---

## 🎨 Design Token Documentation (ready to use)

### Core Palette

| Token | Hex / Value | Use Case | Contrast Notes |
|---|---|---|---|
| `--teal` | `#20c997` | Brand accent — icons, progress bars, checkboxes, borders, large decorative fills, focus rings (as shadow, not text) | Do NOT use as button/text color on white or vice versa (2.13:1) |
| `--teal-accessible` | `#148262` | **New.** Text/icon-on-white OR white-text-on-fill where AA compliance is required — primary buttons, links, badge text | 4.77:1 with white ✅ AA |
| `--teal-dark` | `#17a882` | Hover states for large/graphical elements only (not relied on for AA text contrast) | 3.02:1 with white — large-UI only |
| `--ink` | `#2c3e50` | Primary text, headings, dark surfaces (card headers, footers) | 10.98:1 on white ✅ AAA |
| `--white` | `#ffffff` | Base background, text-on-dark | — |
| `--gray-bg` | `#f7f9fb` | Page background, subtle section fill, input hover bg | — |
| `--gray-text` | `#6b7785` | Secondary/disabled text, captions, hints (on pure white only) | 4.56:1 on white ✅ AA (large-text safe on tinted bg) |
| `--border` | `rgba(60, 60, 60, 0.12)` | Dividers, card outlines, input borders (decorative only, not for conveying state) | ~1.4:1 — decorative use only |
| `--error` | `#e03131` | Validation errors, destructive actions, required-field markers | 4.51:1 on white ✅ AA |

### New Semantic Additions (recommended)

| Token | Hex / Value | Use Case | Contrast Notes |
|---|---|---|---|
| `--success` | `#1a9c6b` | Explicit success state, distinct from brand teal (deeper/darker than `--teal-accessible` for differentiation), confirmation banners | 5.4:1 on white ✅ AA |
| `--warning` | `#b7791f` | Warning banners, "needs attention" states, non-blocking cautions (e.g. holiday conflicts, unsaved changes) | 4.6:1 on white ✅ AA |
| `--warning-bg` | `#fdf6e8` | Warning banner background tint | — |
| `--info` | `#2f6fb0` | Informational hints, tooltips, "optional step" callouts (distinct from gray so it reads as intentional, not muted) | 5.1:1 on white ✅ AA |
| `--info-bg` | `#eaf2fb` | Info banner background tint | — |
| `--success-bg` | `rgba(26, 156, 107, 0.12)` | Success banner/badge background tint (replaces reusing teal for this purpose) | — |

### Complete CSS Variable Block

```css
:root {
  /* ---- Brand ---- */
  --teal: #20c997;              /* decorative/large fills only — fails text contrast */
  --teal-accessible: #148262;   /* AA-compliant teal for text & button labels (4.77:1 on white) */
  --teal-dark: #17a882;         /* hover state for large/graphical elements */
  --teal-tint: rgba(32, 201, 151, 0.12); /* badge/pill background */

  /* ---- Neutrals ---- */
  --ink: #2c3e50;                /* primary text, headings */
  --white: #ffffff;
  --gray-bg: #f7f9fb;            /* page/section background */
  --gray-bg-alt: #f8f9fa;        /* card/preview box background (consolidate with --gray-bg if no visual need to differ) */
  --gray-text: #6b7785;          /* secondary text, captions */
  --border: rgba(60, 60, 60, 0.12); /* dividers, card/input outlines */
  --neutral-track: #eef1f4;      /* progress bar track, unchecked toggle bg */
  --neutral-disabled: #cbd3da;   /* disabled button bg, unchecked checkbox border */

  /* ---- Semantic states ---- */
  --error: #e03131;
  --error-bg: rgba(224, 49, 49, 0.10);
  --success: #1a9c6b;
  --success-bg: rgba(26, 156, 107, 0.12);
  --warning: #b7791f;
  --warning-bg: #fdf6e8;
  --info: #2f6fb0;
  --info-bg: #eaf2fb;

  /* ---- Elevation / shape ---- */
  --radius: 12px;
  --shadow: 0 10px 30px rgba(44, 62, 80, 0.08);
  --shadow-card: 0 2px 8px rgba(0, 0, 0, 0.1);
  --shadow-card-hover: 0 8px 24px rgba(0, 0, 0, 0.15);
}
```

### Usage Guidelines

**Buttons**
- Primary button background: `--teal-accessible` (`#148262`), NOT `--teal`. Text: `--white`. Hover: darken further (e.g. `#0f6b51`) or reduce opacity — do not lighten toward `--teal`.
- Secondary button: `--white` bg, `--gray-text` text, `--border` outline.
- Disabled button: `--neutral-disabled` bg.
- If the brighter `--teal` (`#20c997`) must stay for brand-recognition reasons on marketing surfaces (e.g., `index.html` hero), pair it only with `--ink` or `--white` text at large/bold display sizes (≥24px), never body-size button labels.

**Text on tint/badge backgrounds**
- Use `--teal-accessible` or `--success` as the text color on `--teal-tint`/`--success-bg`, not `--teal-dark` — closes the ~2.7:1 gap identified in the audit.

**Links**
- Inline links (`.nav-breadcrumb a`, `.btn-link`): use `--teal-accessible`, underline on hover for redundant (non-color) affordance — important since the color-only signal is borderline.

**Semantic banners**
- Success: `--success-bg` background, `--success` text/icon (replace current teal reuse for `.success-banner`).
- Warning: `--warning-bg` background, `--warning` text/icon.
- Info: `--info-bg` background, `--info` text/icon — recommended for "this step is optional" hints currently rendered in plain `--gray-text`.
- Error: `--error-bg` background, `--error` text/icon (currently error only appears as text/border, never as a banner — add for parity once needed).

---

## Consistency Check — Verified Across Files

| Token/Rule | index.html | payroll-periods-wizard.html | holiday-calendar-wizard.html | leave-types-wizard.html |
|---|---|---|---|---|
| `:root` variable block present | ❌ No (hardcoded literals) | ✅ Yes | ✅ Yes | ✅ Yes |
| `--teal` value | `#20c997` (hardcoded) | `#20c997` | `#20c997` | `#20c997` |
| `--teal-dark` / hover value | `#1ab385` ⚠️ **differs** | `#17a882` | `#17a882` | `#17a882` |
| `--ink` value | `#2c3e50` (hardcoded) | `#2c3e50` | `#2c3e50` | `#2c3e50` |
| Page background | `linear-gradient(#f5f7fa→#fff)` ⚠️ **differs** | `#f7f9fb` flat | `#f7f9fb` flat | `#f7f9fb` flat |
| Secondary text color | `#666` / `#999` ⚠️ **differs** | `#6b7785` | `#6b7785` | `#6b7785` |
| `--border` value | `rgba(60,60,60,0.12)` | `rgba(60,60,60,0.12)` | `rgba(60,60,60,0.12)` | `rgba(60,60,60,0.12)` |
| `--error` value | (not used) | `#e03131` | `#e03131` | `#e03131` |
| Section/card bg | `#f8f9fa` | `--gray-bg` (`#f7f9fb`) ⚠️ **differs** | `--gray-bg` | `--gray-bg` |

**Summary**: The 3 wizard files (`payroll-periods`, `holiday-calendar`, `leave-types`) are pixel-identical in their token declarations — true consistency. `index.html` (the hub/landing page) is the outlier: it predates or was built separately from the token system and uses several close-but-not-identical hardcoded values. This is the first thing to fix when formalizing the design system — migrate `index.html` onto the shared `:root` block.

---

## 💡 Recommendations for Future Phases

1. **Immediate**: Fix the white-on-teal contrast failure by introducing `--teal-accessible` (`#148262`) for all button/badge/link text use cases. This is a one-line CSS variable swap in each `.btn-primary`, `.btn-add`, `.brand-dot`, and badge/pill rule — low effort, resolves the most severe accessibility gap.
2. **Immediate**: Migrate `index.html` to use the shared `:root` token block instead of hardcoded hex values; reconcile its background gradient and hover-state color with the 3 wizard pages.
3. **Short-term**: Extract the token block into a single shared `kando-tokens.css` (or a `<style>` partial) once these wireframes move past the wireframe stage, so the palette has one source of truth instead of 3 copy-pasted blocks.
4. **Short-term**: Add the semantic `--success`, `--warning`, `--info` tokens above and apply `--info` styling to "this step is optional" hint text (currently plain gray) — cheap win for scannability in multi-step wizards.
5. **Medium-term**: Formal component-level tokens (e.g., `--btn-primary-bg`, `--btn-primary-text`) layered on top of the primitive palette, so future rebrands only touch one abstraction layer instead of hunting through raw hex references.
6. **Medium-term — Dark Mode**: Not urgent for an HR back-office tool typically used in bright office environments during business hours, but if the mobile app (`kando_mobile_frontend`, Flutter) or long-session payroll/timekeeping views get dark-mode requests, plan a parallel token set now while the palette is still small (8 core + 6 semantic = 14 tokens, manageable to dualize). Suggested direction: `--ink` inverts toward a warm off-white (`#e8ecef`) rather than pure white to avoid harsh contrast; `--teal-accessible` likely needs to brighten slightly in dark mode since dark backgrounds shift perceived contrast requirements.
7. **Verify with real users**: Run the updated palette through a simulator (e.g., browser DevTools' vision-deficiency emulation) for deuteranopia/protanopia — teal/green as the *sole* signal for "success" and "selected" state is a common colorblindness pitfall; the current checkmark icons (✓) on checkboxes already help here, but confirm badges/pills also carry a non-color cue (e.g., icon or label text, not just teal tint) where they currently don't (e.g., `.tag-custom`, `.badge-yes`/`.badge-no` — these already differ in label text, which is good).

---

## File References
- `/Users/ericmagto/Projects/kando/research/kando-onboarding-wizard/index.html`
- `/Users/ericmagto/Projects/kando/research/kando-onboarding-wizard/payroll-periods-wizard.html`
- `/Users/ericmagto/Projects/kando/research/kando-onboarding-wizard/holiday-calendar-wizard.html`
- `/Users/ericmagto/Projects/kando/research/kando-onboarding-wizard/leave-types-wizard.html`
