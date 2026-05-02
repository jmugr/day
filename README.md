# State Lines

A small static website for mapping the current time against cognitive and body-state components based on how long you have been awake. Enter a wake time, optionally set a target next wake time for sleep planning, and the main page shows line graphs for alertness, focus, executive control, processing speed, and physical readiness.

## What It Does

- Calculates the day from a configurable wake time.
- Supports an optional target next wake time to calculate the optimal sleep window.
- Shows current component strength based on the local machine clock.
- Shows the sleep window as a subtle band and stops the graph at the end of that sleep window.
- Tracks states like wake transition, prime focus, executive control, recovery dip, execution bandwidth, social/reactive bandwidth, downshift, and sleep.
- Shows dependencies inside each window card, such as wake time, sleep, caffeine timing, and next-day baseline.
- Lets optional windows be shown or hidden.
- Displays every cognitive state as a sorted list of cards.
- Marks currently open and optional states directly on the list.
- Keeps the cognitive state card map available at `cognitive-map.html`.

## Running Locally

No build step is required. You can open `index.html` directly in a browser:

```powershell
start .\index.html
```

Or host it locally from the project directory:

```powershell
python -m http.server 8000 --bind 127.0.0.1
```

Then open `http://127.0.0.1:8000/`.

The app is fully client-side and uses only HTML, CSS, and JavaScript.

Open `cognitive-map.html` for the card-based cognitive state map.

## State Line Scale

The State Lines graph uses an absolute 0-100 component-strength scale. A value of `100` means the component is at its modeled ideal maximum, not merely at its own highest point for the day. Because of that, some lines intentionally never max out:

- `Alertness` can plateau below 100 because normal wakefulness is not the same as maximum arousal.
- `Executive control` and `Processing speed` may peak lower because they represent more fragile capacities.
- `Focus` and `Physical readiness` can reach 100 when their modeled conditions are strongest.

The lines are not normalized independently. This keeps the components comparable to each other, so a line that peaks at 80 is being shown as weaker than a line that peaks at 100. `Physical readiness` is inverted from recovery need: high means body readiness is high and recovery need is low.

## Cognitive State Behavior

The window list covers the wake cycle from `Wake` to the same clock time the next day. Most states are calculated from time awake, while sleep-relative states are calculated backward from the sleep window. The optional `Target next wake` field only recalculates the optimal sleep window:

- With no target next wake, sleep defaults to `21:00-22:00`.
- With a target next wake, sleep becomes the 8-9 hour window before that target.
- Target next wake times close after the wake time are treated as tomorrow's wake. For example, `Wake 05:15` and `Target next wake 05:16` means tomorrow at `05:16`, so sleep is `20:16-21:16`.

All states render as full cards in chronological order. Overlapping states stay visible as separate cards.

## Current Panel

The left panel shows the highest-priority active cognitive state, or the next state when no window is currently open. It includes:

- The active or upcoming window time.
- Time since wake.
- The next opening window.
- Time until bed, calculated from the sleep window.
- Good-fit activities for the selected window.
- Any other active window, or the next upcoming window.

## Dependencies

Dependency labels are shown as pills inside each window card.

Explicit dependency examples:

- `Sleep`: `Wake time`, `Target next wake`
- `Prime focus`: `Wake time`, `Optional first coffee`
- `Recovery dip`: `Wake time`, `Caffeine timing`
- `Execution bandwidth`: `Wake time`, `Recovery dip`
- `Downshift`: `Sleep`

Windows without explicit `dependencies` infer them from their timing rule: sleep-relative windows depend on `Sleep`, and wake-relative or fixed clock windows depend on `Wake time`.

## Editing Windows

The windows live in `windowTemplates` inside `cognitive-map.html`.

Each window has:

- `label`: display name.
- `group`: category shown in the map.
- `color`: tag and card color.
- `priority`: which active window wins the current panel.
- `startOffset` / `endOffset`: minutes after wake.
- `fixedStartClock` / `fixedEndClock`: fixed clock-time windows, such as the default sleep window from 9-10pm.
- `relativeToSleep`, `startBeforeSleep`, and `endBeforeSleep`: calculate windows backward from the sleep window.
- `dependencies`: optional labels shown on each card; omitted windows infer dependencies from wake-time or sleep-window timing.
- `note`: guidance shown in the current panel.
- `fits`: recommended activities for that window.

Example:

```js
{
  id: "prime-focus",
  label: "Prime focus",
  group: "1h 15m-4h awake",
  color: "#214f3c",
  priority: 90,
  startOffset: 75,
  endOffset: 240,
  dependencies: ["Wake time", "Optional first coffee"],
  note: "The cleanest high-agency state for original thought, architecture, writing, and hard synthesis.",
  fits: ["Writing and synthesis", "Hard analytical work", "Planning that turns into concrete output"]
}
```

## Project Structure

```text
.
|-- index.html
|-- cognitive-map.html
|-- state-lines.html
|-- SESSION_SUMMARY.md
`-- README.md
```

## Notes

The app always uses the current local time from `new Date()` for the current-state panel and line graph. The time inputs are `Wake` and optional `Target next wake`.
