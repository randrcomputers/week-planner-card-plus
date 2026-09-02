# Week Planner Card Plus

**Current release: v2.0.13**

**Week Planner Card Plus** is a fork of the excellent **Week Planner Card** by FamousWolf, with extra features aimed at a **Skylight-style family calendar dashboard**.  
This “Plus” version adds UI behavior needed for our Skylight dashboard setup (for example: a working **Add button** + **hash-based popup routing** fixes), and it can be used with both **cloud calendars** (Google / CalDAV / etc.) and **Local Calendar (.ics)**.

For Local Calendar `.ics` add/edit/delete support, it’s designed to pair nicely with the companion integration:

- **ICS Calendar Tools** (add/edit/delete + repeat support for Local Calendar `.ics` calendars)  
  https://github.com/randrcomputers/ics-calendar-tools

> If you only want the original Week Planner Card, see:  
> https://github.com/FamousWolf/week-planner-card

---

## What’s different in “Plus”

- ✅ **Skylight-style** UI flows (popup routing / interaction behavior)
- ✅ Fixes for the **Add button** behavior (restored + reliable)
- ✅ Fixes for **hash / popup routing** used in our Skylight-style dashboard
- ✅ **Empty-day + empty-space click-to-add** (optional, built-in dialog)
- ✅ **Edit remembers calendar** (Edit dialog preselects the clicked event’s calendar)
- ✅ **Schedule view (Skylight timeline)** + recommended CSS for all-day wrapping
- ✅ **Built-in Add/Edit dialog** with Title, Description, Location, and repeat (daily / weekly / **fortnightly** / monthly / yearly)
- ✅ **Local Calendar (.ics)** title fallback when only Description is set in the file
- ✅ Per-calendar **Display in day header** (e.g. holidays as pills under the day number)
- ✅ **Series-safe recurring edit/delete** (This event / This and following / Whole series)
- ✅ Local Calendar edits via Home Assistant’s native `calendar/event/update` WebSocket API
- ✅ Optional **`soonTime`** window → CSS class `soon` for upcoming events
- ✅ Schedule/timeline **`startHour` / `endHour`** (fixed or `auto`) to hide empty hours
- ✅ Timeline **`autoScroll`**, **`dimPastEvents`**, **`showCurrentTimeBoundary`** (all opt-in, default off)
- ✅ Optional pairing with **ICS Calendar Tools** as a fallback for Local Calendar `.ics`  
  https://github.com/randrcomputers/ics-calendar-tools

---

## Recent updates (September 2026)

### v2.0.13 — Timeline auto-scroll and “now” line ([#9](https://github.com/randrcomputers/week-planner-card-plus/issues/9))

Thanks to [@sebeard](https://github.com/sebeard) for the suggestion. All three options default to **`false`** (no change unless you enable them):

- **`autoScroll`** — on load, scroll the timeline so the current time is visible (~1 hour from the top)
- **`dimPastEvents`** — dim timed events on **today** that already ended (timeline + list schedule view)
- **`showCurrentTimeBoundary`** — red “now” line across **today’s column** in the timeline grid

```yaml
viewMode: schedule   # or timelineWeek / timelineDay
autoScroll: true
dimPastEvents: true
showCurrentTimeBoundary: true
startHour: auto      # pairs well with autoScroll (#5)
endHour: auto
```

---

## Recent updates (August 2026)

### v2.0.12 — soon class, timeline hours, docs for combineSimilarEvents

- **`soonTime`** ([#7](https://github.com/randrcomputers/week-planner-card-plus/issues/7) — thanks [@pete-malibu](https://github.com/pete-malibu)): optional `HH:MM` window so upcoming events get CSS class `soon` (default `00:00` = no change). Style with card_mod, e.g. `.event.soon { background: ... }`
- **`startHour` / `endHour`** ([#5](https://github.com/randrcomputers/week-planner-card-plus/issues/5) — thanks [@treiners](https://github.com/treiners)): hide empty early/late hours in schedule/timeline view. Use a number (`7`, `22`) or `"auto"`
- **`combineSimilarEvents`** documented ([#8](https://github.com/randrcomputers/week-planner-card-plus/issues/8)): default is **`false`** (events from different calendars stay separate even with the same title/time)

After updating via HACS (or copying `week-planner-card-plus.js`), **hard-refresh** the dashboard (Ctrl+F5).

---

## Recent updates (July 2026)

### v2.0.11 — Local Calendar edit + series-safe recurrence

Thanks to [@enieuwy](https://github.com/enieuwy) for [PR #6](https://github.com/randrcomputers/week-planner-card-plus/pull/6):

- **Local Calendar editing works** via Home Assistant’s native WebSocket `calendar/event/update` (no longer depends on non-existent `calendar.update_event` / missing ICS service names)
- **Recurring events are series-safe** — edit/delete scopes: *This event only* / *This and following* / *Whole series* (uses `recurrence_id` + `recurrence_range`; avoids shifting the series DTSTART)
- **Themed in-card delete dialog** (replaces stacked browser `confirm()` prompts)
- Edit-dialog polish: Start/End on one row, compact description, clearer **Apply changes to** control

After updating via HACS (or copying `week-planner-card-plus.js`), **hard-refresh** the dashboard (Ctrl+F5).

---

## Earlier updates (May 2026)

Summary of fixes and features added in **v2.0.5 → v2.0.10**.

### Add / Edit dialog fixes

| Version | Change |
|---------|--------|
| **v2.0.5** | Title, Location, and Description fields no longer go blank while typing (fixed `ha-textfield` binding). HACS listing icon (`icon.png`). |
| **v2.0.7** | Description no longer cleared when opening Edit (removed spurious `value-changed` on load). |
| **v2.0.8** | **Description** is always visible: native `<textarea>` under **Title** (Home Assistant’s `ha-textarea` often does not render inside custom-card dialogs). Form scrolls when Repeat options are expanded. |
| **v2.0.10** | **Title** and **Location** also use native inputs (same blank-while-typing issue as Description). Edit dialog fills Title from description fallback when Local Calendar `summary` is empty. |

**Recommended field usage**

- **Title** — main label on the calendar grid (like Google Calendar event names).
- **Description** — extra notes; optional. Saved to `.ics` via **ICS Calendar Tools** for local calendars.
- **Location** — optional.

### Local Calendar (.ics) — event text on the grid

| Version | Change |
|---------|--------|
| **v2.0.6** | If a local event has **Description** but no **Title/SUMMARY** in the API, the card shows the first line of the description on the grid (blue/local events were showing time only). On save, if Title is empty but Description is filled, the first line is copied into **summary** for the `.ics` file. |

### Repeat: Fortnightly

| Version | Change |
|---------|--------|
| **v2.0.9** | New **Repeat → Fortnightly** option. Stored as standard iCal: `FREQ=WEEKLY;INTERVAL=2`. Week-day buttons work the same as **Weekly**. Existing fortnightly events open correctly in Edit. |

### Display holidays (or any calendar) in the day header

| Version | Change |
|---------|--------|
| **v2.0.9** | Per-calendar option **`displayInHeader: true`** (also **`showInDayHeader`**). **All-day** events from that calendar appear as small **pills under the day number**, not duplicated in the main event list. Ideal for **holidays** (`calendar.united_states_mn`, etc.). |

Example (inside your `config-template-card` → `week-planner-card-plus` → `calendars`):

```yaml
- entity: calendar.united_states_mn
  name: Holidays
  color: var(--holidays-default-primary-color)
  filter: ${ HOLCAL }
  displayInHeader: true
```

Or in the card editor: open **Calendars** → your holiday calendar → enable **Display in day header**.

### Month view — column alignment (“gap”)

| Version | Change |
|---------|--------|
| **v2.0.9** | When **Hide days without events** is on, empty days now keep an invisible placeholder so the **7-column grid stays aligned** (fixes reported “gap” / misaligned columns). Outside-month padding days also keep their slot. |

If a gap is still reported, check whether **Hide days without events** is enabled and share a screenshot of the month view.

### Version history (short)

| Version | Notes |
|---------|--------|
| 2.0.13 | Timeline `autoScroll`, `dimPastEvents`, `showCurrentTimeBoundary` ([#9](https://github.com/randrcomputers/week-planner-card-plus/issues/9)) |
| 2.0.12 | `soonTime` class; timeline `startHour`/`endHour`; document `combineSimilarEvents` |
| 2.0.11 | Local Calendar native WS edit; series-safe recurring edit/delete ([PR #6](https://github.com/randrcomputers/week-planner-card-plus/pull/6) by [@enieuwy](https://github.com/enieuwy)) |
| 2.0.10 | Title + Location native inputs; edit-dialog title fallback |
| 2.0.9 | Fortnightly repeat; `displayInHeader`; month grid alignment |
| 2.0.8 | Visible Description field (native textarea) |
| 2.0.7 | Edit dialog Description not wiped on open |
| 2.0.6 | Local calendar grid title from Description fallback |
| 2.0.5 | Edit field typing fix; `icon.png` |
| 2.0.4 | `defaultAllDay` option |
| 2.0.3 | Dutch language |

---

## Credits

- **[@FamousWolf](https://github.com/FamousWolf)** — original [Week Planner Card](https://github.com/FamousWolf/week-planner-card)
- **[@enieuwy](https://github.com/enieuwy)** — [PR #6](https://github.com/randrcomputers/week-planner-card-plus/pull/6): Local Calendar native WS update + series-safe recurring edits/deletes (v2.0.11)
- **[@pete-malibu](https://github.com/pete-malibu)** — [#7](https://github.com/randrcomputers/week-planner-card-plus/issues/7): `soonTime` / `soon` event class idea (v2.0.12)
- **[@treiners](https://github.com/treiners)** — [#5](https://github.com/randrcomputers/week-planner-card-plus/issues/5): timeline `startHour` / `endHour` (v2.0.12)
- **[@steelincable](https://github.com/steelincable)** — [#8](https://github.com/randrcomputers/week-planner-card-plus/issues/8): clarify `combineSimilarEvents` for multi-calendar lunch menus (v2.0.12)
- **[@sebeard](https://github.com/sebeard)** — [#9](https://github.com/randrcomputers/week-planner-card-plus/issues/9): timeline auto-scroll, dim past events, now line (v2.0.13)

---

## Calendar support (Google / cloud + Local Calendar)

Week Planner Card Plus works with **Home Assistant calendar entities** (`calendar.*`) from many sources.  
**What you can do (Add/Edit/Delete/Repeat) depends on what your calendar integration supports.**

Note certain calendars have limitations:

- **CalDAV** — can usually **add** events; edit/delete often not available (no UID)
- **Google** — can usually **add** and **delete**; **edit** support varies
- **Local Calendar** — **add / edit / delete** (including recurring scopes) via the card’s native Home Assistant WebSocket path; **ICS Calendar Tools** remains a useful companion/fallback for file-level `.ics` work

### Cloud calendars (Google / CalDAV / etc.)
- ✅ **View events** in the planner
- ✅ **Add events** (built-in dialog)
- ✅ **Delete events**
- ✅ **Repeat** supported (built-in dialog)
- ⚠️ **Edit support varies** by provider/integration and configuration

### Local Calendar (.ics)
- ✅ **View events**
- ✅ **Add events**
- ✅ **Edit / Delete** via native Home Assistant calendar WebSocket APIs (including This event / This and following / Whole series)
- ✅ **ICS Calendar Tools** still useful as a fallback / for direct `.ics` tooling  
  https://github.com/randrcomputers/ics-calendar-tools

---

<img width="1903" height="961" alt="image" src="https://github.com/user-attachments/assets/beef1e76-9105-4a71-8ef1-2d6ef66e6c6a" />
<img width="1914" height="963" alt="image" src="https://github.com/user-attachments/assets/cf921957-3e9f-4736-ad09-dd473233b4a7" />
<img width="1907" height="1071" alt="image" src="https://github.com/user-attachments/assets/758644fa-1e02-468d-a77a-455502439e10" />

---

## Installation

### HACS (Recommended)

1. Make sure HACS is installed and working.
2. HACS → ⋮ → Custom repositories
3. Open the menu (top right) → **Custom repositories**.
4. Add this repo URL:
   - `https://github.com/randrcomputers/week-planner-card-plus`
5. Category: Dashboard
6. Install **Week Planner Card Plus**.
7. Restart Home Assistant (or reload resources if you prefer).

#### Add the Lovelace Resource

HACS usually offers to add the resource automatically, but you can do it manually if needed:

**Settings → Dashboards → Resources → Add Resource**
- URL (typical HACS path):
  - `/hacsfiles/week-planner-card-plus/week-planner-card-plus.js`
- Type:
  - `JavaScript Module`

> If your built file name/path differs, use the file that exists in your installed `/config/www/community/` folder.

---

### Manual Install (advanced)

1. Copy the built `.js` file into:
   - `/config/www/`
   (or `/config/www/community/week-planner-card-plus/`)
2. Add a Resource:

Example:
- URL:
  - `/local/week-planner-card-plus.js`
- Type:
  - `JavaScript Module`

---

## Basic Usage

### New built-in Add dialog (recommended)

This uses the Plus card’s built-in Add dialog:
- Clicking a totally empty day opens **Add**
- Clicking empty space within a day that already has events opens **Add**
- Clicking an event opens **Edit** (and the calendar is preselected based on the clicked event)

```yaml
type: custom:week-planner-card-plus
tapEmptyDayToAdd: false           # legacy scripted popup (leave off)
clickEmptyDayToAddPlus: true      # NEW built-in dialog
calendars:
  - entity: calendar.family_calendar
```

### Legacy scripted Add dialog (older dashboards)

This uses the old “scripted” Add flow (kept for backwards compatibility).

```yaml
type: custom:week-planner-card-plus
tapEmptyDayToAdd: true            # legacy scripted popup
clickEmptyDayToAddPlus: false     # disable built-in dialog
calendars:
  - entity: calendar.family_calendar
```

> Tip: Use **only one** add mode. Set **either** `tapEmptyDayToAdd` **or** `clickEmptyDayToAddPlus` to `true` — not both.

---

## Schedule View (Skylight)

Week Planner Card Plus also includes a **Schedule** view intended to feel like a Skylight-style weekly timeline.

### Notes / Known quirks
- All-day events are rendered as **pills** at the top of each day column.
- Timed events render inside the hour grid.
- Use **`startHour` / `endHour`** (or `"auto"`) to hide empty early/late hours — see Options below.
- Enable **`autoScroll`**, **`dimPastEvents`**, and **`showCurrentTimeBoundary`** for a Google Calendar–style timeline (all default off).
- If your dashboard uses narrow columns (or long descriptions), all-day pills may visually **bleed into the next day** unless wrapping is forced via CSS.

### Required card_mod for Schedule View

```yaml
            card:
              type: custom:week-planner-card-plus
              viewMode: ${ VIEWMODE } # Needed for new scheduled view
              tapEmptyDayToAdd: false
              clickEmptyDayToAddPlus: true
              calendars:
                - entity: calendar.family_calendar
```

### Recommended card_mod CSS for Schedule View

✅ **ADD THIS: All-day pills wrap inside the day column (no bleed to next day)**

```yaml
card_mod:
  style: |
    /* ✅ ADD THIS: All-day pills wrap inside the day column (no bleed to next day) */
    .timelineAllDayPill,
    .timelineAllDayPill * {
      white-space: normal !important;
      overflow-wrap: anywhere !important;
      word-break: break-word !important;
    }
```

### Optional: force schedule text to black (Skylight readability)

If your schedule pills show white text and you want consistent dark text:

```yaml
card_mod:
  style: |
    /* Schedule view: force black text on all-day + timed pills */
    .timelineAllDayPill,
    .timelineAllDayPill * {
      color: #111 !important;
    }

    .timelineEvent,
    .timelineEvent * {
      color: #111 !important;
    }

    /* Slightly dimmer time text */
    .timelineEvent .time,
    .timelineAllDayPill .time {
      color: rgba(0,0,0,0.70) !important;
    }
```

---

## Notes on Repeat (Recurring Events)

- **Cloud calendars (Google/CalDAV/etc.)**: recurring events are created using Home Assistant’s calendar APIs (built-in dialog).
- **Local Calendar (.ics)**: from **v2.0.11**, add/edit/delete of recurring events uses Home Assistant’s native calendar WebSocket APIs with series-safe scopes (*This event* / *This and following* / *Whole series*). **ICS Calendar Tools** remains available as a fallback for file-level `.ics` work.

**Repeat options in the built-in dialog:** No repeat, Daily, Weekly, **Fortnightly** (every 2 weeks), Monthly, Yearly — plus week-day selection for Weekly/Fortnightly, interval, and end (never / until date / count).

For an **existing** recurring series, the pattern editor is disabled (delete + re-create to change the RRULE). Use **Apply changes to** for occurrence/future field edits.

---

## Options

### Per-calendar: `displayInHeader` (boolean)

When `true`, **all-day** events from this calendar are shown as **pills in the day header** (under the date / weather), not in the main event list.

- Use for **holidays**, birthdays (all-day), or any calendar you want as a compact day label.
- **Timed** events from that calendar still appear in the normal list.
- Also accepts legacy name: `showInDayHeader`

```yaml
calendars:
  - entity: calendar.united_states_mn
    name: Holidays
    color: var(--holidays-default-primary-color)
    displayInHeader: true
```

### `defaultAllDay` (boolean)
Default state of the **All day** toggle in the built-in Add/Edit dialog.

- `true` = All day starts ON
- `false` = All day starts OFF

**Default:** `false`

### `combineSimilarEvents` (boolean)

When `true`, events with the same title + start + end from **different calendars** are merged into one chip (multi-color).

When `false` (default), each calendar keeps its own event — e.g. two school lunch ICS feeds both titled “Lunch Menu” stay separate.

```yaml
combineSimilarEvents: false   # default — keep calendars separate
calendars:
  - entity: calendar.prairieland_lunch
    name: Prairieland Lunch
  - entity: calendar.parkside_lunch
    name: Parkside Lunch
```

### `soonTime` (string, `HH:MM`)

Events that **start within this duration from now** get CSS class `soon` (in addition to not being `past` / `ongoing`).

- Default: `"00:00"` → `soon` is never applied (no change for existing dashboards)
- Example: `soonTime: "01:00"` → events starting in the next hour get class `soon`

Style with card_mod (no default color is applied by the card):

```yaml
card_mod:
  style: |
    .event.soon {
      background: #c62828 !important;
      color: #fff !important;
    }
```

### `startHour` / `endHour` (number or `"auto"`)

Schedule/timeline view only. Controls which hour rows are shown.

- `startHour`: `0`–`23` or `"auto"` (default `0`)
- `endHour`: `1`–`24` or `"auto"` (default `24`)
- `"auto"` uses the earliest/latest **timed** event in the visible days (floored/ceiled to the hour)

```yaml
viewMode: schedule
startHour: 7
endHour: 22
# or:
# startHour: auto
# endHour: auto
```

Aliases: `timelineStartHour` / `timelineEndHour`.

### `autoScroll` (boolean, timeline)

When `true`, scrolls the timeline body on first load so the current time is visible (about one hour from the top). Default: **`false`**.

### `dimPastEvents` (boolean, timeline + list schedule)

When `true`, dims timed events on **today** that have already ended. Default: **`false`**.

### `showCurrentTimeBoundary` (boolean, timeline)

When `true`, shows a red horizontal line at the current time across **today’s column**. Default: **`false`**.

### `clickEmptyDayToAddPlus` (boolean)
When `true`, empty-day / empty-space clicks open the **built-in Add dialog** (recommended).

### `tapEmptyDayToAdd` (boolean)
Legacy mode. When `true`, empty-day clicks use the **older scripted Add flow**.

---

## Companion integration (Local Calendar)

From **v2.0.11**, Local Calendar **edit/delete/repeat** primarily uses Home Assistant’s native calendar WebSocket APIs.

**ICS Calendar Tools** is still useful as a companion/fallback (and for scripting with `list_events`):

- **ICS Calendar Tools**  
  https://github.com/randrcomputers/ics-calendar-tools
