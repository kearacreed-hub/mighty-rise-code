# mighty-rise-code

# Vertical Process Interaction (Rise / Mighty)

## Purpose
A custom vertical “process” interaction for Rise courses, presenting a set of reserved matters as expandable steps connected by animated SVG lines.

Learners can select a step to reveal additional details. Only one step can be open at a time.

---

## Intended Platform
- **Authoring tool:** Articulate Rise (via Mighty)
- **Implementation:** Custom HTML / CSS / JavaScript block
- **Runtime context:** Rise iframe

This code is not a standalone web app.

---

## Files / Structure
All code is currently embedded inline within the Rise block:
- **HTML:** Container structure for the interaction
- **CSS:** Layout, animation, colours, and visual states
- **JavaScript:** Data-driven rendering, interaction logic, SVG connectors, and Mighty completion signal

---

## How It Works
1. On page load, a predefined list of steps is defined in JavaScript.
2. Each step is rendered dynamically into a vertical column.
3. Steps animate into view sequentially.
4. Selecting a step:
   - Closes all other steps
   - Opens the selected step to reveal details
   - Toggles instructional labels
5. Dashed SVG connector lines are dynamically drawn between steps based on their runtime positions.
6. On completion, a message is sent to the parent frame to signal Mighty that the interaction is complete.

---

## Interaction Behaviour
- **Single-open model:** Only one step can be open at any time
- **Click-based toggle:** Clicking a step opens or closes it
- **Animated reveal:** Steps fade and translate into view
- **Responsive redraw:** Connectors redraw on resize and state change

---

## Accessibility Notes
- Uses semantic `div` elements (no native button roles)
- Click interaction only (no keyboard handling currently implemented)
- Colour contrast should be validated against WCAG requirements
- Expand/collapse state is visually indicated but not announced to assistive technology

---

## Styling Notes
- Font: Montserrat (fallbacks included)
- Colour gradients applied per step using `:nth-child`
- Open state overrides background and text colour
- SVG connectors are non-interactive (`pointer-events: none`)

---

## Key JavaScript Considerations
- All DOM manipulation is scoped to `#rise-process-vertical`
- No global variables are exposed
- No external libraries or network calls
- Safe DOM APIs only (`createElement`, `classList`, `querySelector`)
- Layout-dependent drawing uses `getBoundingClientRect`

---

## Rise / Mighty Specific Behaviour
This interaction relies on:
- Rise iframe rendering
- Runtime DOM measurement for SVG connectors
- Parent frame messaging for completion

Because of this, behaviour in CodePen or other preview tools may be *indicative* rather than identical.

---

## Completion Signal
The following message is sent to the parent frame once the interaction has finished loading:

```js
window.parent.postMessage(
  { type: 'MIGHTY_INTERACTIVE_COMPLETE' },
  '*'
);
``

