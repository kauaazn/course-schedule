# Course Schedule

Course Schedule is a responsive React and TypeScript app for organizing university classes, comparing schedule options, and spotting time conflicts. It runs entirely in the browser and stores schedules on the current device.

## Features

- Create, rename, duplicate, and compare schedule plans.
- Add courses with instructors, locations, colors, and multiple weekly meetings.
- Search courses by name, code, or instructor.
- View a weekly calendar and identify overlapping meetings. Back-to-back classes are allowed.
- Export meetings to CSV.
- Keep plans between visits with browser local storage.

## Run locally

Requires Node.js 20.19 or later.

```bash
npm install
npm run dev
```

Open the URL printed by Vite, usually `http://localhost:5173`. Create a production build with `npm run build`.

## How it works

- `src/main.tsx` mounts the React application.
- `src/App.tsx` defines the data model, application state, schedule rules, and interface.
- `src/style.css` contains the visual design and responsive layout.
- `localStorage` stores schedule plans as JSON on the current browser.

See [Study Guide](STUDY-GUIDE.md) for a technical walkthrough and exercises.

## Stack

React · TypeScript · Vite · CSS · Lucide · Web Storage API

## Current scope

This is a client-side portfolio project. It has no account, remote backup, or cross-device synchronization. A natural next step would be unit tests for the conflict rule, JSON import/export, and an API for synchronization.
