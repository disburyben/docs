# Webflow Custom Code — Dirtrack Partners & Co
### Paste these into Webflow Project Settings → Custom Code

---

## HEAD Embed (Global — all pages)

```html
<!-- Google Fonts: Bebas Neue + Anton -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Anton&family=Bebas+Neue&display=swap" rel="stylesheet">

<!-- Film Grain Overlay -->
<style>
  #film-grain {
    position: fixed;
    top: 0; left: 0;
    width: 100vw; height: 100vh;
    pointer-events: none;
    z-index: 9999;
    opacity: 0.045;
    background-image: url('YOUR-GRAIN-PNG-URL');
    background-repeat: repeat;
    background-size: 256px 256px;
  }

  /* Animate grain slightly for realism */
  @keyframes grain-shift {
    0%   { background-position: 0 0; }
    25%  { background-position: -30px 15px; }
    50%  { background-position: 15px -20px; }
    75%  { background-position: -20px 10px; }
    100% { background-position: 0 0; }
  }
  #film-grain { animation: grain-shift 0.4s steps(1) infinite; }

  /* Logo marquee */
  .marquee-track {
    display: flex;
    gap: 80px;
    animation: marquee-scroll 30s linear infinite;
    white-space: nowrap;
  }
  .marquee-track.reverse {
    animation-direction: reverse;
  }
  @keyframes marquee-scroll {
    from { transform: translateX(0); }
    to   { transform: translateX(-50%); }
  }
  .marquee-wrapper:hover .marquee-track {
    animation-play-state: paused;
  }

  /* Horizontal gallery scroll */
  .h-gallery {
    display: flex;
    overflow-x: scroll;
    scroll-snap-type: x mandatory;
    -webkit-overflow-scrolling: touch;
    scrollbar-width: none;
  }
  .h-gallery::-webkit-scrollbar { display: none; }
  .h-gallery-item {
    flex: 0 0 85vw;
    scroll-snap-align: start;
    aspect-ratio: 16/9;
    overflow: hidden;
  }
  .h-gallery-item img {
    width: 100%; height: 100%;
    object-fit: cover;
    filter: grayscale(15%);
  }

  /* Respect reduced motion */
  @media (prefers-reduced-motion: reduce) {
    .marquee-track { animation: none; }
    * { transition-duration: 0.01ms !important; animation-duration: 0.01ms !important; }
  }
</style>
```

---

## BODY Embed (Global — before `</body>`)

```html
<!-- Film grain div -->
<div id="film-grain"></div>

<!-- Page transition overlay -->
<div id="page-overlay" style="
  position: fixed; inset: 0;
  background: #080808;
  z-index: 99998;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s ease;
"></div>

<script>
  // Page transition fade
  document.addEventListener('DOMContentLoaded', () => {
    // Fade out overlay on load
    const overlay = document.getElementById('page-overlay');
    requestAnimationFrame(() => { overlay.style.opacity = '0'; });

    // Fade in on internal link clicks
    document.querySelectorAll('a[href]').forEach(link => {
      const href = link.getAttribute('href');
      if (!href || href.startsWith('#') || href.startsWith('http') || href.startsWith('mailto')) return;
      link.addEventListener('click', e => {
        e.preventDefault();
        overlay.style.pointerEvents = 'all';
        overlay.style.opacity = '1';
        setTimeout(() => { window.location.href = href; }, 320);
      });
    });
  });
</script>

<script>
  // Stat counter animation
  // Usage: add data-counter="4.2" data-prefix="$" data-suffix="M+" to the number element
  document.querySelectorAll('[data-counter]').forEach(el => {
    const target = parseFloat(el.dataset.counter);
    const prefix = el.dataset.prefix || '';
    const suffix = el.dataset.suffix || '';
    const isFloat = !Number.isInteger(target);

    const observer = new IntersectionObserver(entries => {
      if (!entries[0].isIntersecting) return;
      let current = 0;
      const increment = target / 60;
      const timer = setInterval(() => {
        current += increment;
        if (current >= target) { current = target; clearInterval(timer); }
        el.textContent = prefix + (isFloat ? current.toFixed(1) : Math.floor(current)) + suffix;
      }, 16);
      observer.disconnect();
    }, { threshold: 0.5 });

    observer.observe(el);
  });
</script>

<script>
  // Dust particle effect for hero section
  // Add id="hero-particles" to the hero section in Webflow
  const heroSection = document.getElementById('hero-particles');
  if (heroSection) {
    heroSection.style.position = 'relative';
    heroSection.style.overflow = 'hidden';

    const style = document.createElement('style');
    style.textContent = `
      .dust-p {
        position: absolute;
        border-radius: 50%;
        background: rgba(240,237,232,0.3);
        pointer-events: none;
        animation: dust-float linear infinite;
      }
      @keyframes dust-float {
        0%   { transform: translateY(0) translateX(0); opacity: 0; }
        15%  { opacity: 1; }
        85%  { opacity: 0.6; }
        100% { transform: translateY(-280px) translateX(-30px); opacity: 0; }
      }
    `;
    document.head.appendChild(style);

    for (let i = 0; i < 16; i++) {
      const p = document.createElement('div');
      p.className = 'dust-p';
      const size = Math.random() * 2 + 1;
      p.style.cssText = `
        width: ${size}px; height: ${size}px;
        left: ${Math.random() * 100}%;
        bottom: ${Math.random() * 30}%;
        animation-duration: ${10 + Math.random() * 12}s;
        animation-delay: ${Math.random() * 8}s;
        opacity: 0;
      `;
      heroSection.appendChild(p);
    }
  }
</script>

<script>
  // Custom cursor
  // Only on non-touch devices
  if (window.matchMedia('(hover: hover)').matches) {
    const cursor = document.createElement('div');
    cursor.id = 'custom-cursor';
    cursor.style.cssText = `
      position: fixed;
      width: 12px; height: 12px;
      border: 1px solid rgba(232,65,10,0.6);
      border-radius: 50%;
      pointer-events: none;
      z-index: 999999;
      transform: translate(-50%, -50%);
      transition: width 0.25s ease, height 0.25s ease, background 0.25s ease, border-color 0.25s ease;
    `;
    document.body.appendChild(cursor);

    let mouseX = 0, mouseY = 0;
    document.addEventListener('mousemove', e => {
      mouseX = e.clientX; mouseY = e.clientY;
      cursor.style.left = mouseX + 'px';
      cursor.style.top = mouseY + 'px';
    });

    // Expand on interactive elements
    document.querySelectorAll('a, button, [data-cursor-expand]').forEach(el => {
      el.addEventListener('mouseenter', () => {
        cursor.style.width = '36px';
        cursor.style.height = '36px';
        cursor.style.background = 'rgba(232,65,10,0.12)';
        cursor.style.borderColor = 'rgba(232,65,10,0.9)';
      });
      el.addEventListener('mouseleave', () => {
        cursor.style.width = '12px';
        cursor.style.height = '12px';
        cursor.style.background = 'transparent';
        cursor.style.borderColor = 'rgba(232,65,10,0.6)';
      });
    });

    // Hide default cursor
    document.body.style.cursor = 'none';
    document.querySelectorAll('a, button').forEach(el => { el.style.cursor = 'none'; });
  }
</script>
```

---

## Page-Specific: Section 2 Text Reveal

In Webflow, wrap the manifesto copy text in a `<div class="manifesto-block">`. Each paragraph should be a separate `<p class="manifesto-line">`. Then in **Webflow IX2**, apply this interaction to each `.manifesto-line`:

- **Trigger:** Element enters viewport (threshold: 20%)
- **Initial state:** `opacity: 0; transform: translateY(10px)`
- **Final state:** `opacity: 1; transform: translateY(0)`
- **Duration:** 600ms, ease-out
- **Stagger:** Apply to each element individually with increasing delay (0ms, 0ms, 0ms — let scroll position create natural stagger since section is very tall)

Alternatively, use this custom code on the page:

```html
<script>
  // Enhanced manifesto line reveal
  // Add class="manifesto-line" to each <p> in the manifesto section
  const lines = document.querySelectorAll('.manifesto-line');
  lines.forEach(line => {
    line.style.cssText += 'opacity: 0; transform: translateY(10px); transition: opacity 0.6s ease, transform 0.6s ease;';
  });

  const revealObserver = new IntersectionObserver(entries => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.style.opacity = '1';
        entry.target.style.transform = 'translateY(0)';
        revealObserver.unobserve(entry.target);
      }
    });
  }, { threshold: 0.3 });

  lines.forEach(line => revealObserver.observe(line));
</script>
```

---

## Apply Page — Form Submission Handler

Replace the Webflow native form success state with a custom message by embedding this in the `/apply` page:

```html
<script>
  const form = document.querySelector('form');
  if (form) {
    form.addEventListener('submit', () => {
      // Webflow handles actual submission
      // This just tracks the event if analytics is wired up
      if (typeof gtag !== 'undefined') {
        gtag('event', 'application_submitted', { event_category: 'engagement' });
      }
    });
  }
</script>
```

---

## Notes for Developer

- **Grain PNG:** Generate a seamless noise texture at 256×256px (use `noise.texture.net` or Photoshop Add Noise filter, export as PNG). Upload to Webflow Assets and replace `YOUR-GRAIN-PNG-URL` in the HEAD embed.
- **Video hosting:** Do NOT embed the hero video via YouTube. Host on Cloudflare Stream or Mux for performance. Use the `<stream>` or `<video>` element directly.
- **Font licensing:** Bebas Neue and Anton are open license (Google Fonts). Neue Montreal and Satoshi require checking license for self-hosted web use — both have free tiers.
- **Mobile:** Hide the custom cursor on touch devices — the `(hover: hover)` media query in the script handles this.
- **Webflow plan:** Business plan required for removal of Webflow branding on the custom domain. CMS plan is not needed since this site has no CMS — it's all static content.
