# Module description contract

Create one Markdown file per independently implemented UI module. Use names derived from the design's actual regions or components.

Each description should contain:

```markdown
# ComponentName

## Responsibility
Describe the visible region and owned behavior.

## Figma source
- File: design file name
- Node: exact node ID
- Reference dimensions: width × height

## Public interface
List props, emitted events, slots, and important defaults.

## States and interactions
List selected, expanded, menu, dialog, search, sorting, pagination, empty, and disabled states.

## Assets and icons
List design-specific assets and installed UI-library icons.

## Layout invariants
Record fixed dimensions, fluid behavior, anchoring, column rules, overflow, z-index, and responsive behavior.

## Verification
Record the screenshot crop and runtime measurements.
```

Do not freeze responsive behavior into one screenshot measurement. Record Figma dimensions as the reference state and describe the intended resizing, anchoring, wrapping, clipping, and overflow behavior separately.
