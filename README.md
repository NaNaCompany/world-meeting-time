# World Meeting Time

Independent buildless private prototype for W017. Serve `dist/` through static HTTP. No installation or remote data calls; no advertising, tracking or input persistence.

## Scope and behavior

- Reference date, local time and one of 12 IANA city zones; 15–240 minute meeting duration.
- Up to six participant cities, each with editable local working hours. Optional Saturday/Sunday exclusion follows each participant's local date. The reference city participates only if it is in the participant list.
- Actual browser `Intl.DateTimeFormat` rules determine UTC offsets at each instant. No fixed-offset city table.
- Nonexistent source times are rejected. Repeated source times expose an explicit first/second occurrence selector and warning. Exact conversion candidates are validated back against the requested wall time. Offset candidates are sampled around the date to cover standard and daylight time, including half-hour transitions.
- Common starts are tested at 30-minute intervals from 00:00 to 23:30 on the reference city's date. Every minute of the full meeting must fit every participant's work window. Selecting a result updates the schedule.
- Schedule text copy and UTF-8 ICS export. ICS uses UTC instants, so calendar apps can display their own time zones.
- Six fully translated catalogs and static routes: Korean `/`, English `/en/`, Spanish `/es/`, Simplified Chinese `/zh/`, Japanese `/ja/`, German `/de/`. Manual language selector on every route; switching resets inputs.

## Limits

Years 2000–2100. No official holiday calendars or personal leave. Working windows must start and end on the same local day. The browser's time-zone data may lag legal changes. Supported named city zones are intentionally bounded to 12. Browser visual/clipboard/download QA is assigned to the owner and has not been claimed as performed.

## Verification

Run from this app's root with the workspace RTK executable:

```powershell
& '../../.tools/rtk/rtk.exe' proxy node --check dist/app.js
& '../../.tools/rtk/rtk.exe' proxy node --check dist/core.js
& '../../.tools/rtk/rtk.exe' proxy node --check dist/locales.js
& '../../.tools/rtk/rtk.exe' proxy node test.mjs
```

Executed with the environment's bundled primary Node runtime. All commands exited 0. Actual fixture output:

`PASS: DST gap/fold including 30-minute transition, offsets, date rollover, complete-duration work windows, shared start fixtures, validation, ICS escaping and all 6 locale routes`

Independent expected values cover Seoul 09:00 = 00:00 UTC; New York 2026-03-08 02:30 nonexistent; New York 2026-11-01 01:30 at both 05:30/06:30 UTC; winter/summer -05/-04 offsets; Kathmandu +05:45; Tokyo date rollover; Lord Howe half-hour gap and fold; meeting end boundary inclusion; local weekends; 17 half-hour common options for Seoul/Tokyo 09:00–18:00 with a 60-minute duration; no Seoul/New York office-hours overlap; malformed and extreme input; ICS punctuation/newline escaping; every route's language and localization keys.

## Sources and rights

Original UI, text, samples and code. No third-party libraries, images or prose included. Standards references:

- [ECMAScript Internationalization API](https://tc39.es/ecma402/#datetimeformat-objects)
- [IANA Time Zone Database](https://www.iana.org/time-zones)

IANA describes local-time history and periodically updated offset and daylight-saving rules; the app relies on the data shipped by the browser rather than fetching a database.

## Public GitHub Pages release

Launch: https://nanalab.kr/world-meeting-time/

Korean is at the root. English, Spanish, Chinese, Japanese and German use `/en/`, `/es/`, `/zh/`, `/ja/`, `/de/`.
GitHub Pages serves `main` → `/docs` using the branch build. No project CNAME is used.

Build: `node scripts/build-public.mjs`. The `dist/` snapshot and its original generators/tests are preserved as the application source. The public build adds metadata, six-locale sitemaps, original JPEG cards, More Apps styling and the existing NaNa publisher integrations. It does not upload selected user files.

Publisher integrations: AdSense, Google Tag Manager, Google Analytics and Cloudflare Web Analytics, with the same publisher settings as the NaNa Lab homepage. These scripts can make network requests independently of local file processing.

Original JPEG cards are rendered from original text and geometry using an installed system font; no font files are redistributed. Existing source references and third-party notices remain applicable.
