<style>
/* ==========================================================================
   1. GLOBAL
   ========================================================================== */

/* Page background */
body {
  background-color: #FFEFE5 !important;
}

/* Sitewide: headings (h1-h4) and card titles are capitalized; paragraphs and excerpts are not */
.force-title-case,
h1, h2, h3, h4, .gh-card-title, [data-post-card-title] {
  text-transform: capitalize !important;
}
p, .gh-card-excerpt, .gh-card-text, [data-post-card-excerpt] {
  text-transform: none !important;
}

/* Rounded corners on images, portal links, buttons and inputs */
img { border-radius: 8px; }
.gh-card-image img { border-radius: 8px; }
a[data-portal] { border-radius: 8px !important; }
button { border-radius: 8px !important; }
input { border-radius: 8px !important; }

/* Thicker, branded scrollbar */
html {
  scrollbar-gutter: stable;
  scrollbar-width: auto;               /* Firefox: only auto/thin/none are supported */
  scrollbar-color: #3d251d #ffe5e5;    /* thumb, track */
}
html::-webkit-scrollbar {
  width: 18px;
  height: 18px;
}
html::-webkit-scrollbar-track {
  background: #ffe5e5;
}
html::-webkit-scrollbar-thumb {
  background: #3d251d;
  border-radius: 999px;
  border: 2px solid #ffe5e5;
}
html::-webkit-scrollbar-thumb:hover {
  background: #5a362c;
}
html::-webkit-scrollbar-corner {
  background: #ffe5e5;
}
@media (max-width: 768px) {
  html::-webkit-scrollbar-thumb {
    border-width: 4px;
  }
}

/* Sticky footer: keeps the footer at the bottom on short pages */
html, body {
  height: 100%;
}
body {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
main,
[data-main],
#site-main {
  flex: 1 0 auto;
}
[data-footer] {
  flex-shrink: 0;
}


/* ==========================================================================
   2. HEADER / NAVIGATION
   ========================================================================== */

/* Sticky dark header */
[data-header] {
  position: sticky;
  top: 0;
  z-index: 100;
  background-color: #3D251D !important;
  color: #FFEDFC !important;
}

/* Current page underline */
[data-header] li.nav-current::after {
  background-color: #fc9504 !important;
  bottom: -4px !important;
}

/* Hover underline on non-current nav items */
[data-header] nav li:not(.nav-current) a {
  position: relative;
}
[data-header] nav li:not(.nav-current) a::after {
  content: '';
  position: absolute;
  bottom: -4px;
  left: 0;
  width: 100%;
  height: 2px;
  background-color: #FFEDFC;
  opacity: 0;
  transition: opacity 0.25s ease;
}
[data-header] nav li:not(.nav-current) a:hover::after {
  opacity: 1;
}

/* Mobile menu toggle hover */
[data-menu-toggle]:hover span {
  background-color: #fc9504 !important;
  transition: background-color 0.2s ease;
}

/* Subscribe / sign-in hover */
a[data-portal="subscribe"]:hover {
  background-color: #ffa404 !important;
  transition: background-color 0.2s ease;
}
a[data-portal="signin"]:hover {
  color: #ffa404 !important;
  transition: color 0.2s ease;
}

/* Nav link hover: orange text for normal links, orange background for the Home link */
[data-header] nav a:not([href="/"]):hover {
  color: #fc9504 !important;
  transition: color 0.2s ease !important;
}
nav a[href="/"]:hover {
  background-color: #ffa404 !important;
  color: #1b1000 !important;
  transition: background-color 0.2s ease !important;
}
.home-hover-btn {
  transition: background-color 0.2s ease !important;
}
.home-hover-btn:hover {
  background-color: #ffa404 !important;
}

/* Nav spacing + vertical divider between About and Games */
@media (min-width: 1024px) {
  [data-header] {
    --nav-gap: 2.25rem; 
  }

  /* Space the top-level items evenly */
  [data-header] ul:has(> li[data-slug]):not(li ul, [data-menu] ul) {
    column-gap: var(--nav-gap) !important;
  }
  [data-header] ul:not(li ul, [data-menu] ul) > li[data-slug] {
    margin-left: 0 !important;
    margin-right: 0 !important;
  }

  /* The divider, centred in the gap to the left of "Games" */
  [data-header] ul:not(li ul, [data-menu] ul) > li[data-slug="games"] {
    position: relative;
  }
  [data-header] ul:not(li ul, [data-menu] ul) > li[data-slug="games"]::before {
    content: "";
    position: absolute;
    left: calc(var(--nav-gap) / -2);
    top: 50%;
    transform: translateY(-50%);
    width: 2px;
    height: 1.5em;                                 
    background-color: rgba(255, 237, 252, 0.4);     
  }
}

/* No divider in the slide-out mobile menu */
[data-menu] li[data-slug="games"]::before {
  display: none !important;
}
/* ==========================================================================
   3. SECTION HEADINGS (title with a line on each side + "See all" button)
   ========================================================================== */

[data-section-head] {
  justify-content: center !important;
  align-items: center !important;
  gap: 16px !important;
  position: relative !important;
}
[data-section-title] {
  text-align: center !important;
  flex-shrink: 0 !important;
}

/* The two decorative lines: a real <span> on one side, ::before on the other */
[data-section-head] > span {
  display: block !important;
  flex: 1;
  height: 2px;
  background-color: #0d0d0d !important;
  opacity: 0.15;
  border: none !important;
  max-width: calc(100% - 120px) !important;
}
[data-section-head]::before {
  content: '';
  display: block;
  flex: 1;
  height: 2px;
  background-color: #0d0d0d !important;
  opacity: 0.15;
}

/* "See all" button, pinned to the right of the heading */
[data-section-head] > a {
  position: absolute !important;
  right: 0 !important;
  top: 50% !important;
  transform: translateY(-50%) !important;
  display: inline-flex !important;
  align-items: center !important;
  justify-content: center !important;
  gap: 8px !important;
  background-color: #3D251D !important;
  color: #FFEFE5 !important;
  font-size: 0.95rem !important;
  font-weight: 500 !important;
  letter-spacing: 0.04em !important;
  padding: 10px 18px !important;
  min-height: 44px !important;
  border-radius: 6px !important;
  text-decoration: none !important;
  border: none !important;
  white-space: nowrap !important;
  z-index: 10 !important;
}
[data-section-head] > a span {
  color: #FFEFE5 !important;
}
[data-section-head] > a svg {
  stroke: #FFEFE5 !important;
}

/* Mobile: title and "See all" share a row, the line drops underneath */
@media (max-width: 768px) {
  [data-section-head] {
    display: flex !important;
    flex-wrap: wrap !important;
    flex-direction: row !important;
    align-items: center !important;
    justify-content: flex-start !important;
    gap: 8px 12px !important;
  }
  [data-section-head] > h2,
  [data-section-title] {
    order: 1 !important;
    flex: 0 1 auto !important;
    margin: 0 !important;
    text-align: left !important;
  }
  [data-section-head] > a {
    order: 2 !important;
    margin-left: auto !important;
    position: static !important;
    transform: none !important;
    display: inline-flex !important;
    font-size: 0.85rem !important;
    padding: 8px 12px !important;
    min-height: 44px !important;
  }
  [data-section-head] > span {
    order: 3 !important;
    flex: 0 0 100% !important;
    width: 100% !important;
    max-width: none !important;
    margin: 4px 0 0 !important;
  }
  [data-section-head]::before {
    display: none !important;
  }
}


/* ==========================================================================
   4. POST CARDS
   ========================================================================== */

/* Tag and archive pages: white background and white cards */
body.tag-template,
body.archive-template {
  --color-bgr: #ffffff !important;
}
.tag-template article,
.archive-template article,
.tag-template .gh-card,
.archive-template .gh-card,
.tag-template [data-post-card],
.archive-template [data-post-card] {
  background-color: #ffffff !important;
  --color-bgr: #ffffff !important;
}

/* Tag and archive pages: card shape, spacing and hover lift */
.tag-template .gh-article,
.archive-template .gh-article,
.tag-template article.post,
.archive-template article.post {
  background-color: #ffffff !important;
  border-radius: 12px !important;
  padding: 12px 24px !important;
  margin-bottom: 24px !important;
  transition: transform 0.3s ease, box-shadow 0.3s ease !important;
  box-sizing: border-box !important;
}
.tag-template .gh-article:hover,
.archive-template .gh-article:hover,
.tag-template article.post:hover,
.archive-template article.post:hover {
  transform: translateY(-2px) !important;
  box-shadow: 0 4px 12px rgba(0,0,0,0.05) !important;
}

/* Homepage "latest posts" block uses the cream page color, not white */
[data-latest-posts] {
  --color-bgr: #FFEFE5 !important;
  background-color: #FFEFE5 !important;
}
[data-latest-posts] * {
  --color-bgr: #FFEFE5 !important;
}
[data-latest-posts] input {
  background-color: #ffffff !important;
  --color-bgr: #ffffff !important;
}

/* All post cards: rounded corners, clipped overflow */
[data-post-card],
[data-post-card] > a figure,
[data-podcasts] article,
[data-podcasts] [data-post-card],
[data-videos] article,
[data-videos] [data-post-card] {
  border-radius: 8px !important;
  overflow: hidden !important;
}

/* Hero post cards: rounded text area with extra bottom padding */
[data-hero-posts] [data-post-card-content],
[data-hero-posts] article > div,
[data-hero-posts] .gh-card-content {
  border-radius: 8px !important;
  padding-bottom: 28px !important;
}

/* Hero post cards: hide the small tag label (leaves custom /tag/ link cards visible) */
[data-hero-posts] [data-post-card-tags] a {
  display: none !important;
}

/* Video tag page: cream card content */
.tag-video [data-post-card-content],
.tag-video article > div {
  background-color: #FFEFE5 !important;
}

/* "Latest" feed on tag/archive/author/latest pages: force white cards */
body.tag-template [data-post-feed="latest"],
body.archive-template [data-post-feed="latest"],
body.author-template [data-post-feed="latest"],
body.page-latest [data-post-feed="latest"] {
  --color-bgr: #ffffff !important;
  --color-bgr-tone: #ffffff !important;
}
body.tag-template [data-post-feed="latest"] [data-post-card],
body.archive-template [data-post-feed="latest"] [data-post-card],
body.author-template [data-post-feed="latest"] [data-post-card],
body.page-latest [data-post-feed="latest"] [data-post-card],
body.tag-template [data-post-feed="latest"] [data-post-card-content],
body.archive-template [data-post-feed="latest"] [data-post-card-content],
body.author-template [data-post-feed="latest"] [data-post-card-content],
body.page-latest [data-post-feed="latest"] [data-post-card-content],
body.tag-template [data-post-feed="latest"] [data-post-card] > a,
body.archive-template [data-post-feed="latest"] [data-post-card] > a,
body.author-template [data-post-feed="latest"] [data-post-card] > a,
body.page-latest [data-post-feed="latest"] [data-post-card] > a {
  background-color: #ffffff !important;
}


/* ==========================================================================
   5. HOMEPAGE HERO BANNER (injected by the script below)
   ========================================================================== */

.gh-hero-custom {
  width: 100%;
  min-height: 100vh;
  background-color: #3D251D; /* Fallback while loading */
  background-size: cover;
  background-position: center;
  display: flex;
  align-items: center;
  padding: 40px 5%;
  box-sizing: border-box;
  margin-bottom: 4rem !important;
  transition: background-image 0.3s ease-in-out;
}
.gh-hero-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  gap: 40px;
}

/* Left side: title, subheading, paragraph */
.gh-hero-left {
  flex: 1;
  color: white;
  text-shadow: 0 4px 20px rgba(0,0,0,0.7), 0 2px 6px rgba(0,0,0,0.9), 0 1px 2px rgba(0,0,0,1);
}
.gh-hero-left h1 {
  font-size: clamp(4rem, 10vw, 6.5rem);
  font-weight: 700;
  margin: 0 0 12px 0;
  line-height: 1.1;
  text-align: left;
  font-family: 'DejaVu Serif', serif !important;
}
.gh-hero-left .gh-hero-subheading {
  font-size: clamp(1.35rem, 2.6vw, 1.85rem);
  margin: 0;
  opacity: 0.97;
  text-align: left;
}
.gh-hero-left .gh-hero-paragraph {
  font-size: clamp(1.1rem, 1.9vw, 1.4rem);
  margin: 16px 0 0 0;
  opacity: 0.95;
  text-align: left;
  line-height: 1.55;
  max-width: 650px;
}

/* Signup card (bottom right) */
.gh-hero-bottom {
  display: flex;
  justify-content: flex-end;
  align-items: flex-start;
  margin-top: 60px;
}
.gh-hero-card-wrap {
  flex: 0 0 380px;
  max-width: 100%;
  max-height: 280px;
}
.gh-hero-card {
  background: rgba(255, 255, 255, 0.85);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.5);
  border-radius: 20px;
  padding: 22px 28px;
  color: #1b1000;
  text-shadow: none;
  height: 100%;
  box-sizing: border-box;
}
.gh-hero-card h3 {
  font-size: 1.5rem;
  font-weight: 800;
  margin: 0 0 8px 0;
  line-height: 1.15;
}
.gh-hero-card p {
  font-size: 0.925rem;
  line-height: 1.45;
  opacity: 0.85;
  margin: 0 0 14px 0;
  color: #1b1000;
}
.gh-hero-card-btn {
  display: inline-block;
  background-color: #fc9504;
  color: #1b1000 !important;
  padding: 10px 24px;
  border-radius: 10px;
  font-weight: 700;
  text-decoration: none;
  font-size: 0.95rem;
  transition: background-color 0.2s ease;
}
.gh-hero-card-btn:hover {
  background-color: #ffa404;
}

/* Hero: mobile */
@media (max-width: 768px) {
  .gh-hero-custom {
    padding: 40px 6%;
    align-items: flex-start;
    padding-top: 60px;
  }
  .gh-hero-inner {
    flex-direction: column;
    gap: 0;
  }
  .gh-hero-left h1 {
    font-size: clamp(2.8rem, 12vw, 4rem);
  }
  .gh-hero-left .gh-hero-subheading {
    font-size: 1.3rem;
  }
  .gh-hero-left .gh-hero-paragraph {
    font-size: 1.15rem;
  }
  .gh-hero-bottom {
    flex-direction: column;
    margin-top: 32px;
    gap: 32px;
  }
  .gh-hero-card-wrap {
    flex: none;
    width: 100%;
  }
  .gh-hero-card {
    padding: 24px;
  }
}


/* ==========================================================================
   6. FOOTER
   ========================================================================== */

[data-footer] {
  background-color: #3D251D !important;
  color: #FFEFE5 !important;
}
[data-footer] .border-t-4,
[data-footer] .border-b,
[data-footer] .border-t {
  border-color: rgba(255,239,229,0.2) !important;
}
[data-footer] [data-brand] {
  color: #FFEFE5 !important;
}

/* Footer column links */
[data-footer] ul a {
  color: rgba(255,239,229,0.7) !important;
  text-decoration: none !important;
  border-bottom: 2px solid transparent !important;
  transition: color 0.2s ease, border-color 0.25s ease !important;
  display: inline-block !important;
  opacity: 1 !important;
}
[data-footer] ul a:hover {
  color: #FFEFE5 !important;
  border-bottom-color: #fc9504 !important;
}

/* Copyright line links */
[data-footer] [data-footer-date] a,
[data-footer] [data-footer-ghost] a,
[data-footer] [data-footer-theme] a {
  color: rgba(255,239,229,0.7) !important;
  border-bottom: 2px solid transparent !important;
  transition: color 0.25s ease, border-color 0.25s ease !important;
  display: inline-block !important;
}
[data-footer] [data-footer-date] a:hover,
[data-footer] [data-footer-ghost] a:hover,
[data-footer] [data-footer-theme] a:hover {
  color: #FFEFE5 !important;
  border-bottom-color: #fc9504 !important;
}

/* Footer link grid: 2 columns on mobile, 5 on desktop */
[data-footer] .footer-grid {
  display: grid !important;
  grid-template-columns: repeat(2, 1fr) !important;
  gap: 24px !important;
  padding: 32px 0 !important;
  border-bottom: 1px solid rgba(255,239,229,0.2) !important;
}
@media (min-width: 768px) {
  [data-footer] .footer-grid {
    grid-template-columns: repeat(5, 1fr) !important;
    gap: 32px !important;
  }
}

/* Footer social icons */
[data-footer] nav .footer-social {
  border: 1px solid rgba(255,239,229,0.3) !important;
  color: #FFEFE5 !important;
  background-color: transparent !important;
  transition: color 0.2s ease, border-color 0.2s ease, background-color 0.2s ease !important;
}
[data-footer] nav .footer-social:hover {
  background-color: #fc9504 !important;
  border-color: #fc9504 !important;
  color: #1b1000 !important;
}
[data-footer] nav .footer-social:hover svg {
  stroke: #1b1000 !important;
}
</style>

<script>
/* Homepage only: build hero banner with full-resolution background from /site-assets/ */
document.addEventListener('DOMContentLoaded', async function() {
  if (window.location.pathname === '/' || window.location.pathname === '') {
    var hero = document.createElement('div');
    hero.className = 'gh-hero-custom';
    hero.innerHTML = `
      <div class="gh-hero-inner">
        <div class="gh-hero-left">
          <h1>Life100.club</h1>
          <p class="gh-hero-subheading">Your trusted guide to health, longevity, and living well in your 60s, 70s, and beyond!</p>
          <p class="gh-hero-paragraph">We've gathered the latest health and brain research, simple fitness tips, and great travel and exploration deals. All these to help you extend your health span, get connected with like-minded individuals, and enjoy this new chapter of your life.</p>
          <div class="gh-hero-bottom">
            <div class="gh-hero-card-wrap">
              <div class="gh-hero-card">
                <h3>Start your journey</h3>
                <p>Get weekly insights on health, longevity, and lifestyle delivered straight to your inbox.</p>
                <a href="#/portal/signup" class="gh-hero-card-btn">Join our FREE Newsletter</a>
              </div>
            </div>
          </div>
        </div>
      </div>
    `;

    var header = document.querySelector('[data-header]');
    if (header) {
      header.insertAdjacentElement('afterend', hero);
    }

    try {
      var res = await fetch('/site-assets/');
      if (res.ok) {
        var html = await res.text();
        var doc = new DOMParser().parseFromString(html, 'text/html');
        var img = doc.querySelector('img[alt*="hero-bg"]') || doc.querySelector('.gh-content img, article img, figure img');

        if (img) {
          var src = null;
          var srcset = img.getAttribute('srcset');

          if (srcset) {
            var parts = srcset.split(',').map(function(item) {
              var s = item.trim().split(/\s+/);
              return { url: s[0], w: parseInt(s[1], 10) || 0 };
            });
            parts.sort(function(a, b) { return b.w - a.w; });
            if (parts.length > 0 && parts[0].url) {
              src = parts[0].url;
            }
          }

          if (!src) {
            src = img.getAttribute('src') || img.src;
          }

          if (src) {
            // Strip Ghost's thumbnail subdirectory (e.g. /content/images/size/w1000/... -> /content/images/...)
            src = src.replace(/\/size\/w\d+\//, '/');
            hero.style.backgroundImage = 'url("' + src + '")';
          }
        }
      }
    } catch (err) {
      console.warn('Could not load hero background from /site-assets/:', err);
    }
  }
});
</script>