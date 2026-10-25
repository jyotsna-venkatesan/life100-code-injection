<script>
document.addEventListener('DOMContentLoaded', async function() {
  // 1. Social icons injection
  var footerSocials = document.querySelector('[data-footer] .flex.flex-wrap.gap-2\\.5');
  if (footerSocials) {
    footerSocials.innerHTML = `
      <a href="#" aria-label="Facebook" style="display:flex; padding:8px; border-radius:50%; border:1px solid rgba(255,239,229,0.3); color:#FFEFE5; transition: all 0.2s ease;"
        onmouseover="this.style.backgroundColor='#fc9504';this.style.borderColor='#fc9504';this.querySelector('svg').style.stroke='#1b1000';"
        onmouseout="this.style.backgroundColor='transparent';this.style.borderColor='rgba(255,239,229,0.3)';this.querySelector('svg').style.stroke='#FFEFE5';">
        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" stroke-width="2" stroke="#FFEFE5" fill="none" stroke-linecap="round" stroke-linejoin="round"><path stroke="none" d="M0 0h24v24H0z" fill="none"/><path d="M7 10v4h3v7h4v-7h3l1 -4h-4v-2a1 1 0 0 1 1 -1h3v-4h-3a5 5 0 0 0 -5 5v2h-3"/></svg>
      </a>
      <a href="https://www.instagram.com/6090club" aria-label="Instagram" style="display:flex; padding:8px; border-radius:50%; border:1px solid rgba(255,239,229,0.3); color:#FFEFE5; transition: all 0.2s ease;"
        onmouseover="this.style.backgroundColor='#fc9504';this.style.borderColor='#fc9504';this.querySelector('svg').style.stroke='#1b1000';"
        onmouseout="this.style.backgroundColor='transparent';this.style.borderColor='rgba(255,239,229,0.3)';this.querySelector('svg').style.stroke='#FFEFE5';">
        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" stroke-width="2" stroke="#FFEFE5" fill="none" stroke-linecap="round" stroke-linejoin="round"><path stroke="none" d="M0 0h24v24H0z" fill="none"/><rect x="4" y="4" width="16" height="16" rx="4"/><circle cx="12" cy="12" r="3"/><line x1="16.5" y1="7.5" x2="16.5" y2="7.501"/></svg>
      </a>
      <a href="https://www.youtube.com/@6090club" aria-label="Youtube" style="display:flex; padding:8px; border-radius:50%; border:1px solid rgba(255,239,229,0.3); color:#FFEFE5; transition: all 0.2s ease;"
        onmouseover="this.style.backgroundColor='#fc9504';this.style.borderColor='#fc9504';this.querySelector('svg').style.stroke='#1b1000';"
        onmouseout="this.style.backgroundColor='transparent';this.style.borderColor='rgba(255,239,229,0.3)';this.querySelector('svg').style.stroke='#FFEFE5';">
        <svg xmlns="http://www.w3.org/2000/svg" width="18" height="18" viewBox="0 0 24 24" stroke-width="2" stroke="#FFEFE5" fill="none" stroke-linecap="round" stroke-linejoin="round"><path stroke="none" d="M0 0h24v24H0z" fill="none"/><rect x="3" y="5" width="18" height="14" rx="4"/><path d="M10 9l5 3l-5 3z"/></svg>
      </a>
    `;
  }

  // 2. Build Hero Banner (Homepage only)
  var isHome = window.location.pathname === '/' || window.location.pathname === '';
  if (isHome && !document.querySelector('.gh-hero-custom')) {
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
  }

  // 3. Helper: extract uncompressed full-resolution image URL
  function getBestImgUrl(imgEl) {
    if (!imgEl) return null;
    var src = null;
    var srcset = imgEl.getAttribute('srcset');

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
      src = imgEl.getAttribute('src') || imgEl.src;
    }

    if (src) {
      return src.replace(/\/size\/w\d+\//, '/');
    }
    return null;
  }

  // 4. Fetch /site-assets/ and dynamically bind Hero & Cards
  try {
    var res = await fetch('/site-assets/');
    if (!res.ok) {
      console.warn('[SiteAssets] HTTP fetch failed:', res.status);
      return;
    }

    var html = await res.text();
    var doc = new DOMParser().parseFromString(html, 'text/html');

    // Collect all candidate images from the page
    var pageImgs = Array.from(doc.querySelectorAll('.gh-content img, article img, figure img, main img'));
    console.log('[SiteAssets] Total images discovered on /site-assets/:', pageImgs.length);

    if (pageImgs.length === 0) {
      console.warn('[SiteAssets] No images found inside /site-assets/. Make sure the page has images and is Published.');
      return;
    }

    // Identify Hero image (marked with "hero" in alt or caption, or defaults to 1st image)
    var heroImg = pageImgs.find(function(img) {
      var alt = (img.getAttribute('alt') || '').toLowerCase();
      var fig = img.closest('figure');
      var cap = (fig && fig.querySelector('figcaption') ? fig.querySelector('figcaption').textContent : '').toLowerCase();
      return alt.includes('hero') || cap.includes('hero');
    }) || pageImgs[0];

    // Apply Hero Background
    if (isHome && heroImg) {
      var heroEl = document.querySelector('.gh-hero-custom');
      var heroUrl = getBestImgUrl(heroImg);
      if (heroEl && heroUrl) {
        heroEl.style.backgroundImage = 'url("' + heroUrl + '")';
        console.log('[SiteAssets] Hero background loaded:', heroUrl);
      }
    }

    // Pool of remaining images for the 6 cards (excluding the hero image)
    var cardPool = pageImgs.filter(function(img) { return img !== heroImg; });

    // Update each card by data-asset-key
    var targetCardImgs = document.querySelectorAll('img[data-asset-key]');
    targetCardImgs.forEach(function(targetEl, idx) {
      var key = (targetEl.getAttribute('data-asset-key') || '').toLowerCase();
      var shortKey = key.replace(/^card-/, '');

      // Method A: Check alt text, caption, or src filename for the key
      var matchedImg = cardPool.find(function(img) {
        var alt = (img.getAttribute('alt') || '').toLowerCase();
        var fig = img.closest('figure');
        var cap = (fig && fig.querySelector('figcaption') ? fig.querySelector('figcaption').textContent : '').toLowerCase();
        var src = (img.getAttribute('src') || '').toLowerCase();

        return alt.includes(key) || alt.includes(shortKey) ||
               cap.includes(key) || cap.includes(shortKey) ||
               src.includes(shortKey);
      });

      // Method B: Fallback by order of appearance if tags weren't assigned
      if (!matchedImg && cardPool[idx]) {
        matchedImg = cardPool[idx];
        console.log('[SiteAssets] Fallback order match for:', key, '-> item #' + (idx + 1));
      }

      if (matchedImg) {
        var resolvedUrl = getBestImgUrl(matchedImg);
        if (resolvedUrl) {
          targetEl.src = resolvedUrl;
          if (targetEl.hasAttribute('srcset')) {
            targetEl.removeAttribute('srcset');
          }
          console.log('[SiteAssets] Injected card:', key, '->', resolvedUrl);
        }
      } else {
        console.warn('[SiteAssets] No image available for:', key);
      }
    });

  } catch (err) {
    console.error('[SiteAssets] Error processing assets:', err);
  }
});
</script>

<script>
  window.socials = window.socials || {};
</script>