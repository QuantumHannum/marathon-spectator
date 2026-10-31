# NYC Marathon Spectator Navigator

A single-file, phone-first, static spectator guide for the **2026 New York City Marathon (Sunday, November 1, 2026)**. Hosted free with GitHub Pages; no backend, database, paid map API, or account needed for site visitors.

Website: https://quantumhannum.github.io/marathon-spectator/

> **Privacy:** This README intentionally does not identify the runner or disclose their bib number. The public `index.html` may include a configured bib number; changes to a public repository are publicly visible.

## Purpose and core workflow

The site helps a spectator arriving at Moynihan Train Hall at **10:58 AM** navigate by subway and on foot, potentially see a runner several times, take a quick vegan lunch break, and get back to Moynihan **by 4:20 PM**. The navigation experience is deliberately simple:

1. Read the **current step**: where to go, what to do, predicted runner arrival and travel/return warnings.
2. Open **Google Maps / Apple Maps** directions or inspect the embedded subway-area map.
3. Check the runner in the **official NYRR live tracker**, then enter an actual NYRR split or an in-person sighting in **Update runner timing**.
4. The app recalculates future arrival times and evaluates feasible remaining viewing points in the background. Advance the navigation step yourself using **Next** or **Back**.
5. View the 0–26.2 mile **runner progress bar** near the bottom. This is a prediction, **not GPS tracking**.

## Baseline race assumptions

| Input | Default |
|---|---|
| Wave start | 10:55 AM |
| Estimated start-line crossing | 11:05 AM (10-minute delay) |
| Sustained running pace | 12:00 minutes/mile |
| Opening crowd adjustment | Approximately 2 additional minutes after mile 2 |
| Distance | 26.2 miles |
| Spectator station arrival | Moynihan Train Hall, 10:58 AM |
| Initial station buffer | Until 11:25 AM |
| Required return | Moynihan by 4:20 PM |

Baseline arrival estimates use **11:05 AM + 12 minutes × miles + about 2 minutes early-crowd allowance**. Estimated: mile 8 **12:43 PM**; mile 16.6 **2:26 PM**; mile 18.2 **2:45 PM**; mile 21.8 **3:29 PM**; finish **4:21 PM** (rounding varies).

The predictions begin automatically without a manually entered checkpoint. They evolve after confirmed observations. One observation anchors the forecast; two or more support observed split-pace estimation, with dampening for implausible changes and an allowance for later slowing. Calculated times and estimated position are always predictions, not official observations.

## Itinerary and transport

- **Start:** Moynihan Train Hall / Penn Station. Walk to **34 St–Herald Sq**, board Brooklyn-bound **Q**, exit **Atlantic Av–Barclays Ctr**.
- **Viewing 1:** **Fourth Avenue at Flatbush Avenue**, Brooklyn (approximately mile 8; baseline **12:43 PM**).
- **Lunch:** quick vegan food around **292 Ashland Place, Brooklyn**: Next Level Burger / vegan-labeled selections at Whole Foods. Verify opening hours on race day.
- **Viewing 2:** **First Avenue at East 72nd Street**, Manhattan (approximately mile 16.6; baseline **2:26 PM**). Return via **Q** from Atlantic Av–Barclays Ctr to **72 St**, then walk east.
- **Optional extra viewing:** **First Avenue at East 96th Street** (approximately mile 18.2; baseline **2:45 PM**). A second sighting after E 72 requires Q to 96 St plus walking and crowds; the runner has only ~19 minutes between locations. At baseline, **assume infeasible** unless a live assessment establishes sufficient margin.
- **Conditional Harlem viewing:** **Fifth Avenue at 125th Street** (approximately mile 21.8; baseline **3:29 PM**). From First Avenue / E 72: walk to Lexington Av / 77 St, take uptown **6** to 125 St / Lexington, then walk west. This stop is **NOT safe to count on** at baseline because the east-side return departure cutoff is **3:20 PM**, before the predicted runner arrival.
- **Return:** On the **west** side of the Fifth Avenue racecourse, walk west to **125 St 2/3** and take downtown 2/3 to **34 St–Penn Station** (west-side planning cutoff **3:35 PM**). On the **east** side, avoid attempting to cross Fifth Avenue; return via the **125 St Lexington Av 6**, take it to **33 St**, then walk west to Moynihan (east-side cutoff **3:20 PM**, preferably earlier). The **4:20 PM arrival limit overrides all sightings**.

**Important:** Three sightings are a goal, not a guarantee. The current baseline plan strongly supports Brooklyn and East 72nd Street; Harlem and the 96th Street bonus are conditional. The optimizer may recommend skipping sightseeing in favor of the train home. Route times are estimates, not live MTA schedules.

## Website functionality (evolution of the project)

### Navigation and maps

- Small-screen-first, single-page HTML/CSS/JavaScript experience with one visible navigation step at a time.
- Current location/destination map with subway station markers, optional browser geolocation, Google walking/transit links, Apple walking directions, and a direct walk-to-station shortcut.
- Full schedule and emergency return information under collapsible details.
- Optional fourth viewing location preserved in the itinerary model and evaluated conditionally.

### Official runner tracker shortcut

- Button opens the official NYRR tracker in a new tab.
- Separate clipboard-copy button reduces typing; a public, documented bib-prefilled URL is not assumed.
- NYRR times must be entered manually. The app does **not** scrape NYRR or connect to an undocumented API.

### Manual timing and prediction engine

- Timing dropdown includes NYRR timing points and personal viewing locations (Brooklyn, First Ave/E 72 St, optional First Ave/E 96 St, and Fifth Ave/125 St).
- Entry types: **official NYRR**, **personally observed**, or **estimated/unverified**. Personal viewing distances are approximate, not official split markers.
- Accepts New York wall-clock time; optional H:MM:SS elapsed time requires a known start-line crossing. Inputs are validated. Checkpoints can be corrected/deleted, and duplicate distances are replaced rather than appended.
- Recomputes observed pace and projected arrival times on each change, using confirmed observations when available; estimated entries are not promoted to confirmed results.
- Predicted current mile and remainder display on a **0–26.2 mile progress bar**, with a separate indicator for the latest confirmed timing point. Estimates advance with the New York race clock while the page is open.

### Adaptive route optimization, kept behind the scenes

- Evaluates feasible sequences of remaining stops using estimated runner arrival times, spectator location/ready time, travel legs, walking and crowd allowances, and a configurable default **10-minute safety buffer**.
- Optional E 96th St sighting is proposed only when it has enough margin and does not compromise more important stops or the return journey.
- Route recommendations are surfaced in the **current-step card**; the former large Live Decision Assistant and Optional Fourth Sighting panels are hidden to avoid clutter.
- No automatic physical-position detection: **the user must advance the step**. A computed runner location does not confirm that the spectator boarded a train or saw the runner.
- 4:20 PM Moynihan return is treated as a hard planning constraint under modeled transit assumptions; real trains and street access can still differ.

### Data and offline behavior

- One static `index.html`, no build pipeline or backend.
- Existing `localStorage` key **`nycmarathon2026site`** retained to preserve previous state. Legacy entries without verification metadata migrate conservatively as estimated.
- Saving a checkpoint updates predictions and the suggested plan; saved data stays **on that browser/device**, not shared across devices.
- Once loaded, text and calculations can continue without internet if the page remains available. Map tiles, GPS location maps, official tracker, and external directions may require a connection. **Download the PDF backup guides for true offline use.**

## Offline backups

- `NYC_Marathon_Quick_Chart_Offline_2026.pdf`: condensed mobile-readable timetable, baseline runner arrival predictions, optional-stop and return warnings.
- `NYC_Marathon_Step_by_Step_Offline_2026.pdf`: detailed subway/walking directions, vegan lunch suggestions, backup procedures and return routes.

The PDFs use the fixed **11:05 AM** estimated start crossing and **12:00 min/mile** baseline, so they **will not** reflect updates entered into the website. Save them in the iPhone **Files** app before travelling.

## Deployment on GitHub Pages

1. Create or use a **public** GitHub repository.
2. Place `index.html` in the repository **root** of branch `main`.
3. Go to **Settings → Pages → Build and deployment**, choose **Deploy from a branch**, `main`, `/(root)`; save.
4. Open your GitHub Pages URL. Publishing typically takes a few minutes (allow up to ten).
5. To update, replace root-level `index.html`, commit to `main`, refresh the website, and consider a Safari private tab to diagnose stale browser caching.
6. On iPhone Safari, use **Share → Add to Home Screen**.

## Caveats / safety

- The course layout and spectator crossing points are controlled by NYRR and NYPD; follow onsite officials, not a mapping app through barricades.
- NYRR's course page identifies designated crossings at **First Avenue and E 69th, E 72nd, E 83rd, and E 86th Streets**. Do not assume a pedestrian crossing at Fifth Avenue / 125th Street.
- Verify MTA service on race morning. Subway/walking times are assumptions; no app can guarantee a connection.
- Do not treat predicted runner positions, times, or optimization outputs as confirmed race timing.
- **The homebound train and 4:20 PM return deadline have priority over additional sightings.**

### Useful links

- [NYRR NYC Marathon course and access](https://www.nyrr.org/tcsnycmarathon/the-course)
- [NYRR live results](https://liveresults.nyrr.org/)
- [MTA subway status](https://www.mta.info/)
