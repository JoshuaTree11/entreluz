# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Entreluz is a static, single-file website for a handmade lamp ("velador") business based in Montevideo, Uruguay. There is no build system, no framework, no package manager, and no dependencies — everything lives in `index.html`.

## Development

Open `index.html` directly in a browser. No server, compilation, or install step is needed.

## Architecture

`index.html` is a self-contained file with three inline sections:

1. **`<style>`** — All CSS, using CSS custom properties defined in `:root` (`--k`, `--f`, `--p`, `--h`, etc. for dark/amber palette).
2. **`<body>`** — HTML markup for the full single-page site: nav, hero, product configurator, FAQ, and footer.
3. **`<script>`** — Vanilla JS product configurator logic at the bottom of the file.

### JS Product Configurator

The script uses a single global state object `S` that tracks the user's current configuration:

```js
var S = { talle, tallePrice, cable, cableExtra, foco, focoExtra, color, colorLabel, regalo, entrega, envioExtra, zona }
```

Key functions:
- `selTalle / selCable / selFoco / selEntrega / selZona` — mutate `S` and call update functions
- `updViz()` — updates the lamp 3D preview based on current selections
- `updResumen()` — rebuilds the order summary text
- `updTotal()` — recalculates and renders the price breakdown
- `buildMsg()` — constructs the WhatsApp order message
- `validate()` — validates name/phone/address fields before sending
- `calc()` — computes total price from `S`

Orders are submitted by opening a `wa.me` WhatsApp link with the message from `buildMsg()`. If WhatsApp doesn't open, a modal shows the message text for manual copy.

### Pricing data (in the script)

```js
var TALLES = { S: {n:'Chico', p:800}, M: {n:'Mediano', p:950}, G: {n:'Grande', p:1100} };
var FOCOS  = { calido:'Cálida', frio:'Fría', rgb:'Inteligente' };
```

Cable surcharges, shipping costs per zone, and gift-wrap surcharge (+$50) are hardcoded inline.

## Language

All user-facing content and code comments are in Spanish (es-UY locale, UYU currency).
