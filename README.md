# Sync Settings — Connect & Integrations Concept

A high-fidelity, self-contained prototype exploring the product and UX of a
**two-way sync between an inventory product and Shopify**: source-of-truth,
directional control, conflict resolution, change preview, and recovery policy.

> Built as a product-thinking exercise. The product (**"StockBase"**) and all
> data are fictional. No proprietary code or branding.

**[▶ Open the live prototype](https://me-shahsavan.github.io/sync-settings-concept/)**
&nbsp;·&nbsp; or just open `index.html` in a browser (no build step).

---

## Why this exists

Integrations are usually treated as backend plumbing. They aren't — for an SMB
operator, a sync surface *is* the product's trust layer. The hard problems are
rarely "call the API"; they're:

- Which side owns a field when both can edit it?
- What happens to the records that *don't* fit cleanly?
- How does a non-technical user know what just synced, and undo a mistake?

This prototype is an attempt to design for those questions, not around them.

## Two versions — the iteration

The repo intentionally keeps two versions to show how the thinking evolved.

| | **v1 — Foundation** | **v3 — Trust & Recovery** |
|---|---|---|
| Connect & guided setup | ✓ | ✓ |
| Location matching (with suggestions) | ✓ | ✓ |
| Push / Pull buttons (directional sync) | ✓ (but blind) | ✓ (safe to press) |
| Pricing scheme mapping | ✓ | ✓ |
| Order-conflict list | basic | grouped + bulk-fix |
| **Change preview before sync** | — | ✓ ("would update 14 products") |
| **"Send one as a test"** | — | ✓ |
| **Conflict policy** ("do this without asking me") | — | ✓ |
| **Activity log / last-sync visibility** | — | ✓ |

The jump from v1 to v3 is the point: v1 makes sync *possible*; v3 makes it
*trustworthy*.

### The insight that drove v3

In v1 the **Push / Pull buttons worked** — but users hesitated to click them. They
didn't know *what* a sync would change, or whether it could be undone, so a
working feature went unused. That isn't a reliability problem; it's a **trust**
problem. v3 answers the question the user was actually asking — *"what happens if
I press this?"* — with a change preview, a send-one-as-a-test option, and visible
sync history. Same buttons; now safe to press.

## Integration concepts demonstrated

- **Source of truth is per-object, not binary.** For a matched location, one
  side owns stock; the UI makes that an explicit choice rather than an
  assumption.
- **Directional sync is two decisions, not one toggle.** "Send to Shopify" and
  "Bring in from Shopify" are configured separately, each with its own rules.
- **Freshness is stated, not hidden.** Real-time on change vs an hourly sweep is
  surfaced to the user — because "real-time" is a promise you have to keep.
- **Change preview.** Before a sync runs, show the blast radius ("12 updates to
  Shopify") so the operator commits with eyes open.
- **Conflict resolution as a first-class surface.** Missing customer, missing
  pricing scheme, missing taxing scheme — each conflict has a clear, named next
  action instead of a raw error.
- **Recovery policy.** "For future conflicts, do this without asking me" lets the
  operator pre-decide: create the missing entity, add an adjustment line, or
  stop and surface an error.
- **Safe-to-retry vs human-review.** Some mismatches can auto-resolve; some need
  a person. The interface encodes that distinction instead of treating every
  failure the same.
- **Visibility builds trust.** Last sync time, recent imports, and per-item
  status let an operator trust the system without reading logs.

## Tech

Plain, self-contained HTML + CSS. No framework, no build, no dependencies —
open any file directly. Vanilla JS drives the interactive bits (toggles,
conflict expansion, activity log).

```
index.html        # landing page linking both versions
index-v1.html     # v1 — foundation
index-v3.html     # v3 — trust & recovery
```
