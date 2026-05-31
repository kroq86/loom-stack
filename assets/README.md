# Loom Stack — shared brand assets

Canonical images for **all** Loom repos. Host on GitHub Pages after push to `main`.

## URLs (use in README / og:image)

| Asset | Path | Use |
|-------|------|-----|
| **Banner** (1280×320) | `https://kroq86.github.io/loom-stack/assets/loom-stack-banner.svg` | README header, all repos |
| **Social / OG** (1200×630) | `https://kroq86.github.io/loom-stack/assets/loom-stack-og.svg` | GitHub social preview, Twitter card |
| **Logo** (256×256) | `https://kroq86.github.io/loom-stack/assets/loom-stack-logo.svg` | favicon, small embeds |

## README snippet (copy into every Loom repo)

```markdown
[![Loom Stack banner](https://kroq86.github.io/loom-stack/assets/loom-stack-banner.svg)](https://kroq86.github.io/loom-stack/)
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
- [loom-tailcalls](https://github.com/kroq86/loom-tailcalls) (`loom`)
- [loom-runner](https://github.com/kroq86/loom-runner) (`loom-agent`)
- [flow-xray](https://github.com/kroq86/flow-xray)
- [loom-run](https://github.com/kroq86/loom-run) (showcase)

After editing SVGs, push `loom-stack` first so Pages URLs stay valid.
