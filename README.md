# Optimal Windows

A small static website for mapping the current time against flexible daily timing windows instead of rigid work blocks. Enter a wake time, optionally set a target next wake time for sleep planning, and the page shows which windows are open now, what fits those windows, and what is coming next.

## What It Does

- Calculates the day from a configurable wake time.
- Supports an optional target next wake time to calculate the optimal sleep window without changing the visible 24-hour calendar.
- Shows the strongest active window based on the local machine clock.
- Tracks optimal windows for sleep, naps, coffee, task types, optional Zyn, alcohol, workout, and wind-down.
- Lets optional windows be shown or hidden.
- Displays every window as a foreground card in a calendar-style day view.
- Places overlapping windows into side-by-side lanes without making them consume schedule time.

## Running Locally

No build step is required. Open `index.html` directly in a browser:

```powershell
start .\index.html
```

The app is fully client-side and uses only HTML, CSS, and JavaScript.

## Calendar Behavior

The calendar always renders a 24-hour wake cycle, from `Wake` to the same clock time the next day. The optional `Target next wake` field does not clip or resize the visible day. It only recalculates the optimal sleep window:

- With no target next wake, sleep defaults to `21:00-22:00`.
- With a target next wake, sleep becomes the 8-9 hour window before that target.
- Target next wake times close after the wake time are treated as tomorrow's wake. For example, `Wake 05:15` and `Target next wake 05:16` means tomorrow at `05:16`, so sleep is `20:16-21:16`.

All windows render in the foreground. If windows overlap, the app assigns them side-by-side lanes so each window remains directly visible.

## Editing Windows

The windows live in `windowTemplates` inside `index.html`.

Each window has:

- `label`: display name.
- `group`: category shown in the map.
- `color`: tag and card color.
- `priority`: which active window wins the current panel.
- `startOffset` / `endOffset`: minutes after wake.
- `fixedStartClock` / `fixedEndClock`: fixed clock-time windows, such as the default sleep window from 9-10pm.
- `relativeToSleep`, `startBeforeSleep`, and `endBeforeSleep`: calculate windows backward from the sleep window.
- `posture`: short current-mode label.
- `note`: guidance shown in the current panel.
- `fits`: recommended activities for that window.

Example:

```js
{
  id: "deep-work",
  label: "Deep creation",
  group: "Task type",
  color: "#214f3c",
  priority: 90,
  startOffset: 90,
  endOffset: 270,
  posture: "Create",
  note: "The cleanest window for writing, architecture, analysis, hard thinking, and original output.",
  fits: ["Writing and synthesis", "Hard analytical work", "Planning that turns into concrete output"]
}
```

## Project Structure

```text
.
|-- index.html
`-- README.md
```

## Notes

The app always uses the current local time from `new Date()` for the current-window panel. The time inputs are `Wake` and optional `Target next wake`.
