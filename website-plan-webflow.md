# Webflow Website Plan — Dirtrack Partners & Co
### Direction: Teaser / Exclusive / Gate Everything

---

## Core Principle

The website is NOT an information product. It is a brand statement that makes the right people apply and filters everyone else out. No pricing. No detailed service descriptions. No portal walkthrough. Nothing that can be lifted. Everything drives toward one action: **Apply to Work With Us.**

The feel: you land on this and feel like you're looking at something you weren't supposed to find.

---

## What The Site Does NOT Show (Intentional)

- No pricing
- No detailed service breakdowns (named only)
- No portal details (just that it exists)
- No process explanations
- No team bios or headshots
- No contact form — application form only (harder barrier)

---

## Site Structure

Single-page scroll experience. Header has wordmark + one "Apply" button only. No nav links. Two sub-pages at launch:

- `/apply` — Application form
- `/login` — Client portal redirect (just a login button, no content)

---

## Section-by-Section Plan

---

### Section 1 — Hero (Full Screen Video)

**100vh, edge-to-edge looping reel. Nothing else.**

Video edit — seamless loop of:
- Winged sprint car snapping sideways into turn one
- Dirt exploding into catch fence under floodlights
- Helmet visor closeup reflecting track lights
- Suspension compressing hard over the cushion
- Fuel can / pit road chaos
- Long lens shot in traffic — motion blur, compression
- Wing panel detail passing at speed, shallow DOF

**Text on screen:**
```
DIRTRACK PARTNERS & CO.                    [Apply →]
```
Wordmark top-left. Apply button top-right. Nothing else. The video IS the headline.

**Webflow:** `autoplay muted loop playsinline`, `object-fit: cover`. Overlay: `rgba(0,0,0,0.25)`.

---

### Section 2 — Scroll Build-Up (Black + Small White Text)

**Pure black. Small centred text. Line-by-line scroll reveal. Slow. Deliberate.**

Section is 300–400vh tall so the reader is paced.

Copy (each line fades in on scroll):

```
There is no commercial agency built for dirt track racing.

Not at this level.

We fix that.

Advisory. Design. Content.

For the programmes that are serious about what comes
off the track, not just what happens on it.

We don't work with everyone.

Most programmes aren't ready.

If yours is —
```

Line drops. No resolution. Scroll continues.

**Typography:** Neue Montreal, 16px, off-white `#F0EDE8`, letter-spacing 0.05em, line-height 2.4.

**Webflow IX2:** Each `<p>` → scroll trigger → `opacity: 0 + translateY(10px)` → `opacity: 1 + translateY(0)`. 80ms stagger. Ease-out.

---

### Section 3 — Logo Roller

**Infinite horizontal marquee of partner/client logos. All monochrome white.**

- Two rows, scrolling opposite directions
- Speed: ~20px/second
- Logo max-height: 32px, `opacity: 0.6`, `opacity: 1` on hover
- Gap between logos: 80px

**Label above:**
```
PROGRAMMES WE'VE BUILT FOR
```
Satoshi, 11px, uppercase, `rgba(255,255,255,0.4)`, letter-spacing 0.2em.

**Webflow:** CSS `@keyframes` marquee on duplicated strip. Second row: `animation-direction: reverse`.

---

### Section 4 — The Three Pillars (Names Only)

**Three words. No explanations.**

```
ADVISORY

DESIGN

CONTENT
```

Bebas Neue or Anton, 120–140px desktop, off-white. No descriptions. No bullet points. A 1px Sprint Car Red line (48px wide) appears under each word on scroll.

**Webflow IX2:** Clip-path reveal bottom-up per word. `clip-path: inset(100% 0 0 0) → inset(0 0 0 0)`. 600ms ease-out. 200ms stagger.

---

### Section 5 — Work Teaser (No Client Names, No Details)

**Raw visual gallery. The work speaks. No identification.**

- Horizontal scroll desktop (snap per image), vertical stack mobile
- Images: liveries, content stills, design pieces
- `filter: grayscale(15%)` on each
- No captions, no labels, no hover text

**Label above:**
```
SELECTED BUILDS
```

**Webflow:** `overflow-x: scroll` flex container + `scroll-snap-type: x mandatory` + `scroll-snap-align: start` per item.

---

### Section 6 — The Portal (Tease Only)

**One statement. No screenshots. No feature list.**

```
Our clients see everything.

Every outreach. Every deal stage.
Every dollar their brand is generating.

In real time.

No other agency offers this.
```

Same small text treatment as Section 2. Last line has a 1-second delay after the others. No CTA. Let it sit.

---

### Section 7 — Scarcity / Qualification

**Set the barrier. Make it feel earned.**

```
We take on a limited number of
programmes each season.

Applications are reviewed.
Not everyone is accepted.
```

Below in large Bebas Neue, Sprint Car Red `#E8410A`:

```
CURRENTLY ACCEPTING
2025 / 2026 APPLICATIONS
```

64px. No other decoration.

---

### Section 8 — Final CTA (Full Screen)

**100vh. One question. One button.**

```
Is your programme ready?
```

Bebas Neue or Canela Italic, 80px, off-white. Centred.

```
[Apply to Work With Us]
```

Sprint Car Red `#E8410A` background. White text. Anton, 14px, uppercase. Square corners (no border-radius). Links to `/apply`.

Fine print below button:
```
Applications are reviewed within 5 business days.
No retainer is required to apply.
```
Satoshi, 12px, `rgba(255,255,255,0.4)`.

---

### Section 9 — Footer

```
DIRTRACK PARTNERS & CO.
Sponsorship Advisory · Graphic Design · Content Creation

[Client Login]    [Instagram]    [LinkedIn]

© 2025 Dirtrack Partners & Co.
```

Pure black. Off-white type only. No nav links. No phone number.

---

## The Apply Page (`/apply`)

Full black page. Simple form.

Fields:
- Name
- Programme / Team name
- Series you compete in
- What do you need? (Advisory / Design / Content / All Three)
- Tell us about your programme (open textarea)
- How did you hear about us?
- Email

Submit: "Submit Application"

On submission: "We'll be in touch within 5 business days."

No pricing. No service detail. No confirmation of what they're receiving.

---

## Design System

### Colors
| Role | Hex |
|---|---|
| Background | `#080808` |
| Text Primary | `#F0EDE8` |
| Text Muted | `rgba(240,237,232,0.4)` |
| Accent (one only) | `#E8410A` — Sprint Car Red |
| Accent Hover | `#C23208` |

### Typography
| Use | Font | Size |
|---|---|---|
| Impact / pillars | Bebas Neue | 96–140px |
| Section headers | Anton | 52–72px |
| Body / manifesto | Neue Montreal | 15–20px |
| Labels / tags | Satoshi | 11–13px uppercase |

Bebas Neue + Anton: Google Fonts (free). Neue Montreal + Satoshi: self-hosted WOFF2 or Adobe Fonts.

### Texture
- Film grain PNG overlay: `position: fixed; width: 100vw; height: 100vh; pointer-events: none; z-index: 9999; opacity: 0.05`
- Nothing else. Let the footage and type do the work.

---

## Webflow Interactions Summary

| Interaction | Mechanism |
|---|---|
| Navbar → solid on scroll | IX2 scroll trigger on `.nav` → `background: rgba(8,8,8,0.95)` at 80px |
| Section 2 text reveals | IX2 scroll trigger per `<p>`, opacity + translateY, 80ms stagger |
| Pillar word reveals | IX2 scroll trigger, `clip-path: inset(100% 0 0 0) → inset(0)`, 200ms stagger |
| Logo roller | CSS `@keyframes marquee`, duplicated strip, row 2 reversed |
| Horizontal gallery | CSS `overflow-x: scroll` + `scroll-snap-type: x mandatory` |
| CTA hover | IX2 hover → `background: #C23208` + `translateY(-2px)`, 200ms |
| Page transition | JS custom code: black overlay fade in/out on link click |
| Film grain | Fixed PNG overlay, 5% opacity, no interaction |

---

## Build Order

1. Source / license hero video footage (search: "sprint car racing night dirt track" on Shutterstock or Getty)
2. Lock the Section 2 manifesto copy exactly as written
3. Webflow project setup: color variables, typography classes, film grain overlay
4. Build one section at a time, top to bottom
5. `/apply` page last

See `webflow-custom-code.md` for all JS/CSS embed snippets.
