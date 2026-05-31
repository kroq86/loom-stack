[![Loom Stack](https://kroq86.github.io/loom-stack/assets/loom-stack-banner.png)](https://kroq86.github.io/loom-stack/)

# Loom Stack — GitHub Pages

Canonical hub for the Loom Python stack. This repo is not a sixth product; it
is the shared map, landing page, and cross-repo documentation for:

- [loom-tailcalls](https://github.com/kroq86/loom-tailcalls)
- [loom-runner](https://github.com/kroq86/loom-runner)
- [flow-xray](https://github.com/kroq86/flow-xray)
- [loom-run](https://github.com/kroq86/loom-run) (dev showcase)
- [loom-ops](https://github.com/kroq86/loom-ops) (ops / runbook product)

**Live site:** https://kroq86.github.io/loom-stack/

**Full ecosystem map (repos + PyPI):** [docs/ECOSYSTEM.md](docs/ECOSYSTEM.md)

## Stack shape

```text
loom-tailcalls  ->  loom-runner  ->  flow-xray
   primitive          runtime        microscope
        \                 \              \
         \                 +---- loom-run / loom-ops proof apps
          \
           +---- loom-stack = canonical docs, not a runtime dependency
```

## Brand assets

Shared banner, logo, and OG image: [assets/](assets/) · [assets/README.md](assets/README.md)

Static HTML in the repo root; published via GitHub Pages from `main`.
