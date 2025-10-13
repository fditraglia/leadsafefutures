# ReciteMe Installation Guide

Quick guide for adding ReciteMe accessibility toolbar to this Jekyll/GitHub Pages site.

## Prerequisites

1. Get a ReciteMe account and client key from https://reciteme.com/
2. Note: ReciteMe is a paid service

## Installation (3 steps)

### Step 1: Create `_includes/head.html`

This overrides the Minima theme's head to add ReciteMe. Based on Minima 2.5.1 (current GitHub Pages version).

```html
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  {%- seo -%}
  <link rel="stylesheet" href="{{ "/assets/main.css" | relative_url }}">
  {%- feed_meta -%}
  {%- if jekyll.environment == 'production' and site.google_analytics -%}
    {%- include google-analytics.html -%}
  {%- endif -%}

  <!-- ReciteMe - replace YOUR_CLIENT_KEY_HERE with actual key -->
  <script type="text/javascript">
  var serviceUrl = "//api.reciteme.com/asset/js?key=";
  var serviceKey = "YOUR_CLIENT_KEY_HERE";
  var options = {};
  var autoLoad = false;
  var enableFragment = "#reciteEnable";
  var loaded = [], frag = !1;

  window.location.hash === enableFragment && (frag = !0);

  function loadScript(c, b) {
      var a = document.createElement("script");
      a.type = "text/javascript";
      a.readyState ? a.onreadystatechange = function () {
          if ("loaded" == a.readyState || "complete" == a.readyState) {
              a.onreadystatechange = null;
              void 0 != b && b();
          }
      } : void 0 != b && (a.onload = function () { b() });
      a.src = c;
      document.getElementsByTagName("head")[0].appendChild(a);
  }

  function _rc(c) {
      c += "=";
      for (var b = document.cookie.split(";"), a = 0; a < b.length; a++) {
          for (var d = b[a]; " " == d.charAt(0);) d = d.substring(1, d.length);
          if (0 == d.indexOf(c)) return d.substring(c.length, d.length);
      }
      return null;
  }

  function loadService(c) {
      for (var b = serviceUrl + serviceKey, a = 0; a < loaded.length; a++)
          if (loaded[a] == b) return;
      loaded.push(b);
      loadScript(serviceUrl + serviceKey, function () {
          "function" === typeof _reciteLoaded && _reciteLoaded();
          "function" == typeof c && c();
          Recite.load(options);
          Recite.Event.subscribe("Recite:load", function () { Recite.enable() });
      });
  }

  "true" == _rc("Recite.Persist") && (autoLoad = !0);
  if (frag || autoLoad) loadService();
  </script>
</head>
```

### Step 2: Create `_includes/reciteme-button.html`

```html
<button id="enableRecite" class="reciteme-button" aria-label="Accessibility Tools">
  Accessibility
</button>

<script>
document.addEventListener("DOMContentLoaded", function() {
  document.getElementById('enableRecite').addEventListener("click", function() {
    loadService();
  });
});
</script>

<style>
.reciteme-button {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background: #0066cc;
  color: white;
  border: none;
  padding: 12px 20px;
  border-radius: 8px;
  cursor: pointer;
  box-shadow: 0 2px 8px rgba(0,0,0,0.3);
  z-index: 9999;
}
.reciteme-button:hover { background: #0052a3; }
.reciteme-button:focus { outline: 3px solid #ffc107; }
</style>
```

### Step 3: Add button to pages

**Option A - All pages:** Create `_layouts/default.html`:

```html
<!DOCTYPE html>
<html lang="{{ page.lang | default: site.lang | default: "en" }}">
  {%- include head.html -%}
  <body>
    {%- include header.html -%}
    {% include reciteme-button.html %}
    <main class="page-content" aria-label="Content">
      <div class="wrapper">{{ content }}</div>
    </main>
    {%- include footer.html -%}
  </body>
</html>
```

**Option B - Homepage only:** Add to `index.md` after front matter:
```markdown
{% include reciteme-button.html %}
```

### Deploy

```bash
git add _includes/ _layouts/
git commit -m "Add ReciteMe accessibility toolbar"
git push origin main
```

Wait ~10 minutes for GitHub Pages to rebuild.

## Maintenance

**If GitHub Pages updates Minima** (rare, check https://pages.github.com/versions/ quarterly):
- Currently on 2.5.1 (since 2019)
- If they update to 3.0, change line 51 in head.html from `/assets/main.css` to `/assets/css/style.css`

## Troubleshooting

- **Button doesn't appear:** Check GitHub Actions for build errors
- **Toolbar doesn't load:** Verify client key is correct in head.html
- **Site broken:** `git revert <commit>` and push

## Why This Approach?

GitHub Pages uses Minima 2.5.1 which doesn't support `custom-head.html` (that's only in unreleased v3.0). So we override `_includes/head.html` directly - this is standard Jekyll practice and very stable.
