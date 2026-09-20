# Visual review protocol

Use this when comparing a running page with Figma or resolving conflicting review findings.

## Authoritative inputs

Record the Figma file and node ID, whether it is a page frame or parent group with overflow siblings, the browser URL, viewport, device scale, browser engine, capture time, and excluded annotation layers.

The reference and runtime image must have identical pixel dimensions and show the same product viewport. Before capture, verify the final runtime URL, a stable page selector, required async content, and `document.fonts.ready`. Do not substitute a pasted webpage screenshot for a fresh local-service capture when the running page is the requested result.

## Review passes

Review original-size images directly. Independent passes should cover:

1. Layout: fixed regions, coordinates, widths, heights, padding, overflow, and clipping.
2. Detail: icons, borders, colors, type weight, row height, menus, controls, and states.
3. Completeness: missing tables, columns, rows, controls, labels, dialogs, and overflow content.

Ask reviewers to name the element and observable boundary. Do not use similarity scores or diff ratios as decision evidence; browser rendering, font fallback, annotations, and antialiasing make them misleading.

## Resolve disputed findings

When a reviewer claims an offset:

1. Crop both original images with the same `x`, `y`, `width`, and `height`.
2. View both crops at original resolution.
3. Check component borders and stable landmarks before judging text glyphs.
4. Query `getBoundingClientRect()` and relevant computed styles on the runtime page.
5. Change code only when the crop or DOM measurement confirms the claim.

Adapt this browser evaluation to the page's stable selectors:

```js
const rect = (selector) => {
  const value = document.querySelector(selector).getBoundingClientRect()
  return { x: value.x, y: value.y, width: value.width, height: value.height, right: value.right }
}

return ['header', 'main', '[data-testid="primary-content"]']
  .filter((selector) => document.querySelector(selector))
  .map((selector) => ({ selector, ...rect(selector) }))
```

Repeated agreement is not proof when reviewers inspect the same scaled composite. Confirm reported offsets against original-resolution crops or runtime geometry before changing code.

## Font limits

Inspect the computed font and loaded font resources. Platform fallback fonts can change glyph width, apparent weight, and optical spacing even when component coordinates match. Record this as a rendering limitation unless the required font is legally and technically available.
