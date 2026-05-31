# Loom Stack — shared brand assets

Canonical images for **all** Loom repos. Hosted on GitHub Pages (`main` branch).

## URLs (use in README / og:image)

| Asset | PNG (recommended) | SVG (source) |
|-------|-------------------|--------------|
| **Banner** 1280x320 | `https://kroq86.github.io/loom-stack/assets/loom-stack-banner.png` | `.../loom-stack-banner.svg` |
| **Social / OG** 1200x630 | `https://kroq86.github.io/loom-stack/assets/loom-stack-og.png` | `.../loom-stack-og.svg` |
| **Logo** 256x256 | `https://kroq86.github.io/loom-stack/assets/loom-stack-logo.png` | `.../loom-stack-logo.svg` |

**Use PNG in GitHub README** — GitHub blocks external SVG images for security.

## README snippet (copy into every Loom repo)

```markdown
[![Loom Stack](https://kroq86.github.io/loom-stack/assets/loom-stack-banner.png)](https://kroq86.github.io/loom-stack/)
```

Regenerate PNG from SVG after edits:

```bash
cd assets
rsvg-convert -w 1280 -h 320 loom-stack-banner.svg -o loom-stack-banner.png
rsvg-convert -w 1200 -h 630 loom-stack-og.svg -o loom-stack-og.png
rsvg-convert -w 256 -h 256 loom-stack-logo.svg -o loom-stack-logo.png
```

## Colors (match `styles.css`)

| Token | Hex |
|-------|-----|
| Background | `#0f1110` |
| Surface | `#171a18` |
| Text | `#e8ece9` |
| Muted | `#9aa89f` |
| Accent gold | `#d4a853` |
| Accent dim | `#a68437` / `#8B7355` |
| Link / mint | `#7ec8a8` |

## Repos that should use the banner

- [loom-stack](https://github.com/kroq86/loom-stack) (canonical host)
- [loom-tailcalls](https://github.com/kroq86/loom-tailcalls)
- [loom-runner](https://github.com/kroq86/loom-runner)
- [flow-xray](https://github.com/kroq86/flow-xray)
- [loom-run](https://github.com/kroq86/loom-run) (showcase)
