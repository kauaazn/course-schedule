# Course Schedule — Technical Study Guide

This guide explains the path from a user's action to the updated schedule. Follow the data through the app instead of memorizing individual lines.

## 1. Project overview

Course Schedule is a frontend-only weekly class planner. A user creates courses and meetings; React renders them in a calendar, calculates summaries, and flags overlaps. The browser's `localStorage` keeps the plans on the current device. There is no backend or database.

## 2. File map

| File | Responsibility |
| --- | --- |
| `index.html` | HTML shell, metadata, and React root element. |
| `src/main.tsx` | Loads styles and mounts `<App />`. |
| `src/App.tsx` | Data types, state, schedule rules, actions, and UI components. |
| `src/style.css` | Visual design, calendar layout, and responsive behavior. |
| `package.json` | Development/build scripts and dependencies. |
| `vite.config.ts` | Vite and React plugin configuration. |

Most of the UI currently lives in one file. That keeps this demo's flow in one place; a future refactor could split the modal, calendar, and course list into smaller components.

## 3. Data model

At the top of `src/App.tsx`, inspect `Meeting`, `Course`, and `Plan`.

- A `Meeting` contains a weekday, start time, end time, and room.
- A `Course` contains its name, code, instructor, color, and a list of meetings.
- A `Plan` represents one schedule option for a term and contains courses.

A course has multiple meetings because it can meet on more than one weekday. The `starter` value provides sample content on a first visit.

## 4. React state and hooks

The `App` component owns the data that drives the page. Its main state values are:

| State | Meaning |
| --- | --- |
| `plans` | All schedule options and their courses. |
| `active` | ID of the selected plan. |
| `query` | Current course-search text. |
| `modal` | Whether the course form is open. |
| `editing` | Course being edited, or `null` for a new course. |
| `toast` | Short save confirmation. |

When a setter such as `setPlans` changes state, React renders the component again. The calendar, list, and summary are derived from the same course data, so they stay in sync.

- `useState` stores values that change and affect the UI.
- `useEffect` saves plans to local storage when they change.
- `useMemo` recalculates conflicts when the course list changes.

## 5. Persistence in the browser

On startup, the initializer for `plans` reads `course-schedule-plans`. It also checks the previous `grade-de-disciplinas-plans` and `grade-studio-plans` keys as migration fallbacks. If no valid saved data exists, it uses `starter`. An effect serializes plans as JSON and writes the new key.

Local storage only stores strings, so JSON converts objects into text and `JSON.parse` recreates them when the page loads. Clearing site data in the browser removes these plans. There is no remote backup or device synchronization.

## 6. Create and edit flow

1. The **Add course** button opens `CourseModal` with no selected course.
2. Editing passes the existing course into that component.
3. The modal keeps its form fields and meeting list in local state.
4. Its submit handler checks that the course has a name and each end time follows its start time.
5. The modal calls `onSave` with a `Course` object.
6. `saveCourse` either replaces the course with the matching ID or appends a new course.
7. React renders the new calendar and list, then the effect stores the change.

IDs let the app identify an item even when its name changes. `makeId` creates simple IDs for new plans and courses.

## 7. Time conflict rule

The `mins` helper converts an `HH:MM` string to minutes after midnight. For example, 08:30 becomes 510. The conflict calculation compares meetings on the same day and checks:

```text
start A < end B  AND  start B < end A
```

Both conditions must be true for two time ranges to overlap. The comparisons are strict, so a class ending at 10:00 and another beginning at 10:00 are back-to-back, not conflicting. The result powers both the warning and the highlighted calendar blocks. The current warning highlights the full class block rather than the exact overlap segment.

## 8. Calendar rendering

The `days` array defines the weekday columns, and `times` creates hour labels. For each meeting, the component calculates a vertical offset from its start time, a height from its duration, and a column from its weekday. The course supplies the block color. CSS creates the grid; React positions the meeting blocks. Media queries make the calendar scroll on narrow screens.

The previous/next week controls are visual only in this version; they do not change the displayed dates yet.

## 9. Search, duplicate, and CSV export

- `visible` filters by course name, code, or instructor without mutating the original array.
- `duplicate` creates a new plan and new IDs for its courses and meetings, so edits to a copy do not affect the original.
- `exportCsv` turns each meeting into a row, builds a browser `Blob`, and downloads it.

## 10. Study exercises

1. Add two meetings on the same day that overlap by 20 minutes. Find the condition that detects the conflict.
2. Move the second meeting so it begins exactly when the first ends. Explain why the warning disappears.
3. Reload the page and trace how local storage restores the plans.
4. Duplicate a plan, edit the copy, and explain why the original is unchanged.
5. Add a field to `Course` and find each UI and export location that must also use it.

## 11. Explain the project

> “Course Schedule is a React and TypeScript planner. Each course can have several weekly meetings. React state holds schedule options, and an effect persists them in the browser. A time-range calculation detects overlaps, and the UI presents the results in a weekly calendar and course list.”
