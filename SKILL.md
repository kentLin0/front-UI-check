---
name: figma-ui-reconstruction
description: Reconstruct, implement, or fidelity-correct Figma application screens in frontend code, then verify the real running page with screenshots. Use when a user asks to build, reproduce, align, or pixel-match a Figma design. Skip frontend work without a Figma design reference.
metadata:
  short-description: Reconstruct and verify Figma application screens
---

# Figma UI Reconstruction

Implement the design in the repository's existing frontend stack and verify the rendered application rather than relying only on source code or secondary screenshots.

## Establish the comparison target

- Resolve the exact Figma node containing the complete screen. Parent groups may include visible layers or overflow content that are absent from a nested frame export.
- Capture the real running application at an explicit viewport and device scale. Treat a supplied screenshot as supporting evidence unless the user identifies it as the final runtime target.
- Produce one authoritative Figma reference at the runtime viewport's exact pixel dimensions. If the complete screen spans nested frames or sibling layers, compose or crop them on a 1:1 canvas and verify all required content is visible before review.
- Compare the Figma reference and local-service capture at the same viewport and device scale. Exclude review annotations, cursor images, canvas padding, and other non-product layers.
- Before Figma `get_design_context`, load the available Figma design-to-code skill. Before `use_figma`, load the available Figma-use skill.

## Inspect before implementing

Read repository instructions, the application shell, layout styles, existing components, design tokens, and installed UI packages. Reuse the project's established components and conventions where they match the design.

When modular delivery is requested, map the screen into independently owned regions and give each region a description file using [references/module-contract.md](references/module-contract.md). When using subagents, establish contracts and exclusive file ownership before delegation, then designate one integrator to own shared layout and assembled-page verification.

## Implement the layout

Build the outer geometry first: viewport, fixed and fluid regions, layout constraints, overflow, and stacking. Then implement content, controls, states, and interactions.

Inspect repeated and data-driven structures explicitly. Confirm their headers, items, states, dimensions, and any content extending beyond nested frames. Follow explicit product behavior over an isolated static export, including responsive and fluid-layout requirements.

Use actual icon components or assets for interface icons. Inspect installed libraries and existing icon abstractions before adding assets. Prefer the project's icon library, then an exact design asset, then a local SVG or icon component. Record a limitation when none is available. Do not simulate icons with font-dependent text characters.

## Verify the running page

Run the project's relevant checks and production build. Open the actual running application in a browser and set the target viewport. Before every authoritative screenshot, confirm the final URL and intended route, reject login, error, loading, and unintended redirect states, wait for a stable page landmark and required asynchronous content, and wait for `document.fonts.ready`. Capture only after these checks pass. Inspect important layout invariants with DOM rectangles and computed styles when possible.

Use direct visual review instead of treating a pixel-diff ratio as truth. When adversarial review is requested, have independent reviewers inspect layout, component detail, and content completeness. Give reviewers only the two authoritative screenshots and explicitly identify excluded annotations.

If reviewers disagree or report a suspicious repeated offset, do not change the UI immediately. Crop both images with identical coordinates at original resolution, inspect the local regions, and measure DOM bounds. A finding is actionable only when the images or layout measurements support it. Read [references/visual-review.md](references/visual-review.md) for the full protocol.

## Deliverables

Keep the final Figma reference, fresh runtime screenshot, labeled side-by-side review image, requested module descriptions, and a short review record. Record the Figma node, final runtime URL, viewport, device scale, and capture time beside the files. Confirm that paired screenshots have identical dimensions before presenting them together.

Report what changed, the runtime URL and viewport, build results, confirmed remaining limitations such as unavailable fonts, and clickable paths to final artifacts.
