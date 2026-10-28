# Cadence — Student OS

A single-file student productivity app. Everything — markup, styles and scripts — lives in one self-contained `index.html`. There is no backend, no build step, no account and no network calls. Open the file in a browser and it works.

> **Data lives in your browser.** Every record you create is written to `localStorage` under the key `cadence.v1` the moment you save it, and read back when the page loads. That is why your data survives a reload — and why it does **not** follow you to another browser or device unless you export a backup.

---

## How to run it

1. Download or clone this repository.
2. Open `index.html` in a modern browser (Chrome, Edge, Firefox or Safari).

That's it. No install, no build step, no server, no account, no API keys. The app is fully functional offline.

**Optional browser features** (the app detects and reports what is available, and degrades gracefully):

| Feature | Needs | If unavailable |
| --- | --- | --- |
| Audio recording | Microphone + `MediaRecorder` | Type or paste notes instead |
| Live transcription | Web Speech API (Chrome / Edge / Safari 14.1+) + secure context | Manual text entry; flagged in-app |
| Summaries & questions | Nothing — runs locally | Falls back to first-sentence extract if text is very short |
| Deadline notifications | Browser Notification API (optional) | In-app alerts only |
| Gmail filters | Nothing — local XML export | Copy the search query instead |

---

## Features

### 1. Daily plan
Tasks get a priority (high / medium / low), a course, an estimate and a date. The **Today** screen shows the current day's list sorted by priority with a completion bar. The plan view can move between days, carry unfinished work to tomorrow, and shows the week at a glance. Priority beats due date in sorting, because a high-priority task due Friday deserves today's attention more than a low-priority one due tomorrow.

### 2. Study reminders
Two jobs in one place:
- **Deadline alerts** — add an exam or deadline and it shows a countdown chip that turns amber at 7 days and red at 2 days, and appears in the Today "due soon" panel.
- **Spaced repetition scheduler** — give a topic and a first study date, pick an interval ladder (1·3·7·14·30 days by default, with dense / weekly / light alternatives), and Cadence creates one reminder per interval. Tick each off as you do it.

Alerts appear in-app on every visit (checked on load and every 60 seconds while open); system notifications are optional.

### 3. Lecture notes — recording, transcription, summary, questions
- **Recording** via `MediaRecorder` with a timer, a stop button and a downloadable audio file for the session.
- **Live transcription** via the Web Speech API (`SpeechRecognition`).
- **Summarising and question generation** run locally with a classical extractive algorithm — sentences are scored by term frequency, position and length, and the top ones are kept; questions are built from definitional and causal sentences plus keyword cloze cards. No model API, no key, no upload.

If a microphone or speech support is missing, type or paste the lecture into the same box and press **Summarise** / **Generate questions** — the exact same pipeline runs.

### 4. Assignments
Title, course, due date, weight and status (not started → in progress → submitted → graded). Overdue items get a red edge. The stats row totals open work, work due this week, and the percentage of your grade already submitted. Filter between open, submitted and all.

### 5. Focus & distraction control
A Pomodoro timer with 25 / 5 / 15-minute modes, a countdown ring, and automatic hand-off from focus to break. Before starting you write down whatever is pulling at you — the point is to park it, not fight it. Completed focus sessions are logged with the linked task, and daily minutes are totalled on the screen and on Today. **Distraction-free** opens a full-screen timer that covers the app, so the next tab switch is a conscious decision. The timer keeps accurate time from timestamps even if the tab is throttled in the background.

### 6. Email & file organization
- **File naming** — fill in course, type, title, date, version and initials, choose a pattern, and get a normalised filename: lowercase, spaces to hyphens, accents stripped, unsafe characters removed, ISO dates, zero-padded versions. Saved names build a small log so you can see your own consistency.
- **Gmail filters** — the app builds a Gmail search query and a matching filter XML in Gmail's import format. Download the `.xml`, then in Gmail go to **Settings → Filters and Blocked Addresses → Import filters → Choose file → Create filters**. You review each one before it is applied. No API key, no OAuth, and it never asks for your password.

### 7. Weekly review
One card per ISO week. It shows the week's real numbers (tasks completed, focus minutes, notes captured, cards reviewed, reminders cleared), lets you tick off what carries into next week, and stores three free-text fields: wins, friction, and three commitments. Past weeks stay readable, so you can see whether the same friction keeps recurring.

### 8. Grades & GPA
Add courses with credit hours, then add assessments with a score and a weight. Cadence computes the weighted course percentage, a letter grade, and a credit-weighted GPA. Assessments without a score are excluded, so the GPA reflects graded work only. **What-if** projects a course percentage if you score a given mark on the outstanding weight, so you can see what you actually need on the final.

> This is your own arithmetic, not an official transcript.

### Added feature 1 — Flashcards (spaced repetition)
Rereading notes feels productive but barely moves retention; retrieval practice does. Cards use a simplified **SM-2** algorithm — each rating (Again / Hard / Good / Easy) adjusts the interval and ease factor, so weak cards return in minutes and strong ones in weeks. Cards can be created by hand or generated in bulk from a lecture note's generated questions.

### Added feature 2 — Habit streaks
Grades are downstream of boring daily behaviour — sleep, reading, showing up. A 14-day grid with a current streak and a best streak makes a missed day visible without a lecture. Consistency percentage is shown per habit, so a broken streak does not erase the evidence of the good weeks.

### Added feature 3 — Backup & data export
A `localStorage`-only app is exactly as durable as one browser profile. JSON export makes the data portable between laptop, phone and browser, and makes "I cleared my cookies" a ten-second fix instead of a lost semester. **Merge** keeps both sets; **replace** wipes first. Both paths validate the file and refuse anything that is not a Cadence backup.

---

## A typical day

- **Morning — plan (5 min).** Open **Today**, then **Daily plan**. Add the day's real work with priorities. Three high-priority items is a full day; everything else is optional. Check **Reminders** for countdowns and clear any due review.
- **Lecture — capture.** Open **Lecture notes**, hit record, and let the transcript fill in. If speech recognition is unavailable the banner tells you, and you type instead. Press **Summarise** and **Generate questions**, then save.
- **Between classes — recall (10 min).** Go to **Flashcards** and clear the due queue. "Generate questions" on a note can push straight into new cards.
- **Deep work — focus.** Pick the task in **Focus timer**, park your distractions in the box, and run a 25-minute block. Take the break; the timer hands off automatically.
- **Afternoon — submissions.** Check **Assignments**, move anything you finished to submitted, and look at what is due in the next three days.
- **Evening — file and log.** Name today's files in **Files & email** so tomorrow's search works. Tick your **Habits**.
- **Sunday — review.** Open **Weekly review**, read the numbers, write wins and friction, set three commitments. Then **Backup → download JSON**.

---

## Data & privacy

- **Storage:** all data is kept locally in `localStorage` under the key `cadence.v1`. Nothing is sent anywhere — there is no `fetch` call to a third party in the file.
- **One-device data:** private/incognito windows, "clear site data", and iOS Safari's 7-day eviction for unused sites can all wipe `localStorage`. **Export a JSON backup weekly if the work matters.**
- **Permissions:** microphone (only if you record), notifications (only if you enable them). Everything else needs nothing.

---

## Repository contents

| File | Description |
| --- | --- |
| `index.html` | The entire app — markup, styles and scripts inline. |
| `README.md` | This document. |
