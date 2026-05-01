# Optimal Weekday

A small static website for mapping the current time against an ideal weekday schedule. Enter a wake time, optionally mark that the first deep work block starts at home, and the page shows the current cognitive state, best work for the moment, active timing windows, and the full day map.

## What It Does

- Calculates the day from a configurable wake time.
- Shows the current cognitive state based on the local machine clock.
- Supports a `Deep work at home first` mode that moves the first creation block before the commute.
- Displays timing windows for coffee, optional Zyn, water cutoff, and sleep without making them consume schedule time.
- Marks timing windows on the Day Map wherever they overlap a block.

## Running Locally

No build step is required. Open `index.html` directly in a browser:

```powershell
start .\index.html
```

The app is fully client-side and uses only HTML, CSS, and JavaScript.

## Deploying To GitHub Pages

1. Push this repository to GitHub.
2. Open the repository settings in GitHub.
3. Go to `Pages`.
4. Set the source to `Deploy from a branch`.
5. Choose the `main` branch and `/root` folder.
6. Save and wait for GitHub Pages to publish the site.

The published URL will usually look like:

```text
https://YOUR_USERNAME.github.io/REPOSITORY_NAME/
```

## Editing The Schedule

The main schedule lives in `sectionTemplates` inside `index.html`.

Each section has:

- `heading`: the schedule section name.
- `color`: the color used in the timeline.
- `state`: the cognitive state shown in the current panel.
- `focus`: the mode or working posture for that section.
- `work`: the recommended work list.
- `items`: the ordered tasks and durations.

Example item:

```js
{ label: "First coffee and deep work", duration: 100, note: "Writing, thinking, ideas, analysis" }
```

Durations are in minutes. The `Sleep` item uses `derivedSleepWindow: true`, so it expands to fill the remaining time until the next wake time.

## Editing Timing Windows

Timing windows live in `windowTemplates` inside `index.html`.

Windows are separate from schedule blocks. They do not take time away from the day; they are overlays calculated from the wake time or from specific schedule blocks.

Current windows include:

- First coffee
- Second coffee
- Optional Zyn
- Last big water intake
- Optimal sleep window

Each window has:

- `label`: display name.
- `color`: tag and card color.
- `note`: guidance shown in the current panel.
- `findStart`: logic for finding the start time.
- `duration`: window length in minutes.

Use `wakeMinutes + minutesPerDay - X` for windows that are calculated backward from the next wake time, such as sleep or water cutoff.

## Project Structure

```text
.
├── index.html
└── README.md
```

## Notes

The app always uses the current local time from `new Date()` for the current-state panel. The only time input is the wake time.
