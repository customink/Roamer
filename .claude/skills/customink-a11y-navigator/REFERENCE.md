# Reference: Templates, A11y Criteria & Cost

Supporting reference for the CustomInk Usability Navigator skill. Loaded on demand — not on every invocation.

## Think-Aloud Narration Template

Use this structure for each navigation step:

```
## [Page Name] — Step [N]

**First impression:** [What strikes me about this page? Clear or cluttered?]

**What I'm trying to do:** [My current goal as a user]

**How I'm finding my way:** [What a11y elements am I using to orient and navigate?]

**What I'll click and why:** [My decision — what drew me to this element?]

**What happened:** [Did it do what I expected? Was I surprised?]

**Side note (a11y):** [Optional — only if I noticed a gap while navigating]
```

### Example

> **First impression:** Standard e-commerce landing page. The nav has a `<nav>` with `aria-label="Main Navigation"` — clear category links. Search bar is easy to find. Good.
>
> **What I'm trying to do:** Browse t-shirts — probably what most people come here for.
>
> **How I'm finding my way:** Main nav landmark has everything labeled. I can jump right to "Custom T-Shirts" without hunting.
>
> **What I'll click and why:** "Custom T-Shirts" in the main nav — most obvious entry point.
>
> **What happened:** Landed on a category page with listings. Natural transition, no surprise popups.
>
> **Side note (a11y):** Search input uses `aria-label` only — no visible `<label>`.

## Final Report Template

```markdown
# CustomInk Usability Study Report
**Date:** [date]
**Model:** [model used]
**Pages visited:** [count]

## Overall Experience
[3-5 sentences: What was it like as a first-time user? Intuitive? Could you accomplish tasks?]

## Task Walkthroughs

### [Task: e.g., "Find and customize a t-shirt"]
- **Outcome:** Completed / Partially completed / Abandoned
- **Steps taken:** [brief]
- **Friction points:** [what slowed you down]
- **What worked well:** [what felt smooth]

## Usability Highlights
### What Worked Well
[Intuitive, fast, or well-designed elements]

### Pain Points
[Friction, confusion, dead ends, unexpected behavior]

## Accessibility Gaps Found

### Blocked Navigation (couldn't use a11y to interact)
[Elements that forced CSS selector fallback]

### Slowed Down (harder than it should have been)
[Confusing hierarchy, non-descriptive links, missing landmarks]

### Noticed in Passing
[Minor issues that didn't affect the task]

## Cost Summary
- Tool calls: [count]
- Estimated cost: $[amount] (see pricing below)
```

## Common A11y Gaps to Classify

When you bump into a gap, reference the relevant WCAG criterion:

- **Images:** alt text missing (1.1.1)
- **Headings:** skipped levels or missing h1 (1.3.1)
- **Forms:** inputs without labels (3.3.2, 4.1.2)
- **Links:** vague text like "click here" (2.4.4)
- **Keyboard:** unreachable elements (2.1.1), no visible focus (2.4.7)
- **Contrast:** text below 4.5:1 ratio (1.4.3)
- **Landmarks:** missing nav/main/banner roles (1.3.1)
- **Language:** missing lang attribute (3.1.1)
- **Custom controls:** missing ARIA roles/states (4.1.2)

## Severity Levels

Classify gaps by how they affected your navigation:

| Level | Feels Like | Action |
|-------|-----------|--------|
| **Blocked** | "I can't interact with this via a11y — had to use CSS selectors" | Flag in narration |
| **Slowed Down** | "Got there eventually, but harder than it should be" | Note briefly |
| **Noticed** | "Not great, but didn't affect me right now" | Mention if relevant |
| **Nitpick** | "Technically could be better" | Only if you're already discussing the area |

## JavaScript A11y Checks

Run these only when you encounter a specific gap you want to quantify — not as a routine check. Note: these inspect the light DOM only. Elements inside Shadow DOM (web components) will not be detected.

### Heading hierarchy
```javascript
const h = document.querySelectorAll('h1,h2,h3,h4,h5,h6');
JSON.stringify([...h].map(e => ({ level: e.tagName, text: e.textContent.trim().substring(0, 80) })), null, 2);
```

### Images without alt text
```javascript
const imgs = document.querySelectorAll('img');
const missing = [...imgs].filter(i => !i.hasAttribute('alt')).map(i => ({ src: i.src.substring(0, 100) }));
JSON.stringify({ total: imgs.length, missingAlt: missing.length, details: missing.slice(0, 10) }, null, 2);
```

### Unlabeled form inputs
```javascript
const inputs = [...document.querySelectorAll('input,select,textarea')].filter(i => i.type !== 'hidden');
const unlabeled = inputs.filter(i => !i.labels?.length && !i.hasAttribute('aria-label') && !i.hasAttribute('aria-labelledby'));
JSON.stringify({ total: inputs.length, unlabeled: unlabeled.length, details: unlabeled.map(i => ({ type: i.type, name: i.name })) }, null, 2);
```

### ARIA landmarks
```javascript
const lm = document.querySelectorAll('[role="banner"],[role="navigation"],[role="main"],[role="contentinfo"],[role="search"],header,nav,main,footer,aside');
JSON.stringify([...lm].map(e => ({ tag: e.tagName.toLowerCase(), role: e.getAttribute('role') || 'implicit', label: e.getAttribute('aria-label') || 'none' })), null, 2);
```

## Cost Estimation

Pricing varies by model. Use the row matching your session model.

| Model | Input (per MTok) | Output (per MTok) | ~Avg per tool call |
|-------|-----------------|-------------------|-------------------|
| Haiku | $0.80 | $4.00 | ~$0.004 |
| Sonnet | $3.00 | $15.00 | ~$0.015 |
| Opus | $15.00 | $75.00 | ~$0.075 |

At session end, multiply your total tool calls by the average for a rough estimate. For a typical 40-call Haiku session, expect ~$0.16.

Formula: `cost = (input_tokens / 1_000_000 * 0.80) + (output_tokens / 1_000_000 * 4.00)`
