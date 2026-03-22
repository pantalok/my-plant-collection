# Project: My Plant Collection — Texas Care Guide

## Overview
Single-page static site (`index.html`) hosted on GitHub Pages at:
- **Repo**: https://github.com/pantalok/my-plant-collection
- **Live site**: https://pantalok.github.io/my-plant-collection/

## Architecture
- Everything is in one file: `index.html` (inline CSS, inline JS, base64-embedded images)
- No frameworks, no build tools — pure vanilla HTML/CSS/JS
- Plant photos are converted from HEIC to JPEG (600px wide, quality 60) and base64-encoded inline
- GitHub Pages deploys from `main` branch, root `/`

## Current Plant Count: 36
- **9 Indoor plants** (data-category="indoor"): Bird of Paradise (#1), Citrus Tree (#2), Chinese Money Plant (#3), Golden Pothos (#4), Lucky Bamboo (#5), Fiddle Leaf Fig (#6), Monstera (#7), Moth Orchid (#35), Majesty Palm (#36)
- **27 Outdoor plants** (data-category="outdoor"): Dragon Tree (#8), Corn Plant (#9), Hydrangea (#10), Chinese Evergreen (#11), Peace Lily (#12), Impatiens (#13), Kalanchoe pink (#14), Aloe Vera (#15), Florist Kalanchoe (#16), Boston Fern (#17), Cactus (#18), Herb Garden (#19), Rose (#20), Curry Leaf (#21), Mandevilla (#22), Hibiscus (#23), Dipladenia (#24), Jasmine (#25), Begonia (#26), Bougainvillea (#27), Liriope (#28), Azalea (#29), Salvia (#30), Croton (#31), Canna Lily (#32), Asiatic Lily (#33), Variegated Schefflera (#34)

## Page Structure

### Sticky Nav Bar
- Tab toggle row: All / Indoor / Outdoor buttons (`.tab-btn` with `data-tab`)
- Nav pills: one per plant (`.nav-pill` with `data-category` and `href="#plant-N"`)
- JS filters both cards and pills when tabs are clicked

### Plant Cards
Each plant card follows this structure:
```html
<article class="plant-card" id="plant-N" data-category="indoor|outdoor">
  <div class="card-header">
    <div class="card-image-wrap">
      <img src="data:image/jpeg;base64,..." alt="Plant Name" loading="lazy">
      <div class="card-number">N</div>
      <div class="difficulty-badge difficulty-easy|moderate|hard">Easy|Moderate|Demanding</div>
      <div class="category-badge category-indoor|outdoor">Indoor|Outdoor</div>
    </div>
    <div class="card-intro">
      <div class="plant-latin">Latin name</div>
      <h2 class="plant-name">Common Name</h2>
      <p class="plant-desc">Description...</p>
      <div class="plant-info-bar">
        <span class="info-tag toxicity-safe|mild|pets|danger">Toxicity label</span>
        <span class="info-tag info-size">↕ Height range</span>
        <span class="info-tag info-prop">✂ Propagation method</span>
      </div>
      <button class="share-btn" onclick="sharePlant(this)">...</button>
    </div>
  </div>
  <div class="care-grid">
    <!-- 4 care items: Watering, Light, Temperature, Extra Care -->
  </div>
  <div class="meter-guide">
    <div class="meter-header">🌱 Ideal Meter Readings</div>
    <div class="meter-grid">
      <!-- 4 items: pH, Moisture (with meter level), Temp (°F), Light (lux + level) -->
    </div>
  </div>
  <div class="texas-tip">...</div>
</article>
```

### Features
- **Tab filtering**: All/Indoor/Outdoor tabs show/hide cards and nav pills
- **Share button**: "Learn more..." button on each card uses Web Share API on mobile (native share sheet to Claude app, etc.) with clipboard copy fallback on desktop
- **Plant info bar**: Toxicity badge (color-coded: green=safe, yellow=mild, orange=pets, red=danger), mature size, propagation method
- **Meter guide**: Ideal pH, moisture (mapped to meter levels: DRY-, DRY+, NOR, WET-, WET+), temperature (°F), light (mapped to levels: LOW- through HGH+, in lux)
- **Readings tracker**: 4th tab ("Readings") shows thumbnail grid of all plants; tap to record pH/moisture/temp/light readings; history view with color-coded values vs ideal ranges; data persisted in localStorage (`plantReadings` key)
- **Empty state**: Friendly message when a category has no plants
- **Mobile responsive**: `@media (max-width: 768px)` for card stacking, `@media (max-width: 480px)` for phone-specific spacing/fonts (meter grid → 2 columns)
- **Texas-specific care tips** on every card

## Readings Tracker
- **Tab**: "Readings" in the tab-row shows a thumbnail grid instead of plant cards
- **Quick record**: Tap a thumbnail → modal with date (auto today) + 4 number inputs (pH, moisture%, temp°F, light lux) → Save
- **History**: "View History" button in modal opens full-screen overlay with date-sorted table; values color-coded green (in-range), yellow (borderline), red (out-of-range) against each card's meter-guide ideal values
- **Storage**: `localStorage` key `plantReadings`, structure: `{ "plant-1": [{ "date": "2026-03-22", "ph": 6.5, "moisture": 45, "temp": 72, "light": 5000 }, ...] }`
- **Delete**: Each history row has a × button to remove individual readings
- **Keyboard**: Escape closes modal and history overlay

## How to Add a New Plant

1. **Convert the photo**: `sips -s format jpeg -s formatOptions 60 -Z 600 INPUT.HEIC --out /tmp/plant-web/NAME.jpg`
2. **Base64 encode**: `base64 -i /tmp/plant-web/NAME.jpg | tr -d '\n' > /tmp/plant-b64/NAME.b64`
3. **Add a nav pill** in the `<nav class="nav-bar">` section with the correct `data-category` and `href="#plant-N"`
4. **Add the card HTML** following the structure above, inserting after the last card of the same category (indoor cards go after #35, outdoor cards go after #34)
5. **Update the hero count** in the `<p>` tag: "A care guide for N plants..."
6. **Include the share button** in the card-intro section

## Key CSS Variables (defined in :root)
- Green theme: `--green-50` through `--green-900`
- Badge colors: `.difficulty-easy` (green), `.difficulty-moderate` (yellow/sun), `.difficulty-hard` (warm/red)
- Category badges: `.category-indoor` (green tint), `.category-outdoor` (orange tint)

## Photo Sources
- Indoor plants (original 7): `~/Downloads/Photos-3-001 (1)/IMG_8361-8367.HEIC`
- Outdoor plants (27): `~/Downloads/Photos-3-001/IMG_8368-8404.HEIC`
- Moth Orchid: `~/Downloads/Photos-3-001 (1)/IMG_8405.HEIC`

## Git Preferences
- Never mention Claude, Anthropic, or AI in commit messages
- No Co-Authored-By lines referencing Claude
