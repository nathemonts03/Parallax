# Parallax Wedding Website

Developed by EM Capital.

A static, single-file website for a wedding organizer offering planning, photography, video, and same-day edit (SDE) films. No build step, no dependencies, no backend.

The hero is a scroll-driven parallax scene with a subject: a bride and groom stay centered while the world moves past them in layers. The sky moves from dawn to golden hour to dusk to night as you scroll, and each stage introduces one service.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The whole site: HTML, CSS, and JavaScript in one file |
| `README.md` | This guide |

## Run it

Open `index.html` in any modern browser. To publish it, upload the file to any static host (Netlify, Vercel, GitHub Pages, Cloudflare Pages, or plain web hosting). If you add photos or videos, upload them next to the HTML file and keep the relative paths.

## Page structure

| Section | Anchor | What it is |
| --- | --- | --- |
| Journey scene | `#top` | Pinned parallax scene with four captions: planning, photography, film, same-day edit |
| Photography | `#photo` | Six photo tiles with a reveal and a gentle inner parallax |
| Wedding films | `#film` | Three video cards that open a player |
| Same-day edit | `#sde` | Timeline from ceremony to reception screening |
| Contact | `#contact` | Inquiry form |

## Customize

### Brand and contact
- The browser tab title is `Parallax Wedding Website` (the `<title>` tag). Replace `Linden Lane` in the nav logo and the footer with the client's brand.
- The footer credit "Developed by EM Capital" is in the `<footer>` element.
- Replace `hello@lindenlane.example` in the footer and in the form handler near the bottom of the script (the `mailto:` line).
- The form has no server. It opens the visitor's email app. To collect submissions without email, change the form to post to a form service such as Formspree or Netlify Forms.

### Photos
Each photo is a `.tile` whose placeholder is a gradient set in its `style` attribute. Swap it for your image:

```html
<div class="tile rv" style="background-image:url('photos/garden-vows.jpg')"><span>Garden vows</span></div>
```

Keep the tile's `background-size:100% 140%` (set in CSS). The extra height is what lets the image drift as you scroll. Use portrait images, around 1200 x 1600 px, compressed for the web.

### Videos
Each film card is a `<button class="vid">` with a `data-video` attribute. Add your file path and the card plays it in a modal:

```html
<button class="vid" data-video="films/highlight.mp4" data-title="Isabel and Marco: highlight film" ...>
```

Use H.264 `.mp4` files for the widest support. The same attribute works on any other button on the page. The `.vid` cards also take a gradient background that you can replace with a poster image the same way as photos.

### Copy
- Captions in the journey scene are the four `.ch` blocks. Each has a heading, a sentence, and a button linking to a section.
- The same-day edit timeline is the `.steps` block in `#sde`. Change the times and wording to match your real workflow.

### Colors and time of day
The sky, hills, trees, ground, couple, and sun colors come from the `K` array in the script. Each entry is:

```js
[progress, [sky top, sky middle, sky bottom, far hills, trees, ground, couple, sun], string-light level, star level]
```

`progress` runs from 0 to 1 over the scroll. The four entries are dawn, golden hour, dusk, and night. Colors blend smoothly between them. Edit the hex values to change the mood, for example a daytime-only wedding palette.

Colors for the content sections below the scene are the CSS variables at the top of the stylesheet (`--bg`, `--ink`, `--rose`, `--gold`). A dark-mode set is included and follows the visitor's system setting.

### Scene length and speed
- **Scroll length:** change `.journey{height:520vh}`. A larger value makes the walk slower.
- **Layer speed:** each layer is a `.ly` element with a `data-s` value (`.12` far hills, `.35` trees, `.7` arches, `1.25` foreground). Higher means faster. The total horizontal travel is three screen widths multiplied by that value.
- **Arch positions:** in the `fill` function, the `[40,145,250]` list sets where the three flower arches sit, in `vw` units. With the arch layer speed of `.7`, an arch at `x` is centered over the couple when progress equals `(x - 40) / 210`. The first arch frames the couple at the start and the last frames them at the end.

### The couple
The couple is an inline SVG inside `.couple`. Edit its shapes, or replace the `<svg>` with your own silhouette. It uses the `--u` color, which changes with the time of day.

## How the parallax works
1. The `.journey` section is very tall, and `.stage` is pinned to the screen inside it.
2. Scroll position inside that section becomes a progress value from 0 to 1.
3. The progress is smoothed, then used to move each layer sideways at its own speed, move the sun and moon, blend the colors, and pick the active caption.
4. Layers are generated by JavaScript, so they always fill the width of the screen and are rebuilt on resize.

## Accessibility
- Visitors who prefer reduced motion get no smoothing, no walking animation, and no scroll animation.
- Keyboard focus is visible on all links, buttons, and form fields.
- The four dots on the right of the scene are real buttons that jump to each stage.
- The couple illustration has a text label for screen readers.

## Browser support
Current Chrome, Edge, Safari, and Firefox. The page uses CSS `color-mix`, `clip-path`, `backdrop-filter`, and the `<dialog>` element. Fonts (Fraunces and Figtree) load from Google Fonts, with system serif and sans-serif fallbacks if they are blocked.

## Known limits
- Photos and videos are placeholders until you add your own.
- The inquiry form depends on the visitor having an email app set up, unless you connect a form service.
- On very short or very narrow screens, the caption and the couple can sit close together. Test on your target phones after changing the copy.
