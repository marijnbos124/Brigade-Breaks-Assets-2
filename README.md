# Brigade Breaks – image assets

## Included
- `cards/`: 80 standalone PNG illustrations, one for every card in the game.
- `CARD_IMAGE_MAP.js`: exact card ID → relative image path mapping.
- `manifest.json`: card names, categories and paths in JSON.
- `source_sheets/`: the original gallery sheets retained as source/reference.

## Folder placement
Put the entire `assets` folder next to the HTML file:

```
Brigade-Breaks.html
assets/
  cards/
  CARD_IMAGE_MAP.js
  manifest.json
```

## Important
Do NOT use sprite cropping. Every game card already has its own image file.

## Recommended rendering change
Add this CSS after the existing `.card-illus,.mini-illus,.locked-silhouette` CSS block:

```css
.card-illus.has-card-art,
.mini-illus.has-card-art {
  background-image:
    linear-gradient(180deg, rgba(1,5,12,.04), rgba(1,5,12,.48)),
    var(--card-art);
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}
.card-illus.has-card-art::before,
.card-illus.has-card-art::after,
.mini-illus.has-card-art::before,
.mini-illus.has-card-art::after { display:none; }
```

Create this helper near the other render helpers:

```js
function cardArtMarkup(card, className='card-illus') {
  const path = CARD_IMAGE_MAP[card.id];
  const holo = card.rarity === 'mythic' ? ' holo-shine' : '';
  if (!path) return `<div class="${className} illus-${card.category}${holo}"></div>`;
  return `<div class="${className} has-card-art${holo}" style="--card-art:url('${path}')"></div>`;
}
```

Then replace every existing illustration div in the game with one of these:

```js
${cardArtMarkup(card, 'card-illus')}
${cardArtMarkup(card, 'mini-illus')}
```

Keep `.locked-silhouette` unchanged for uncollected cards.
