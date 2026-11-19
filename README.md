<!--
  TODO BEFORE PUBLISHING (search this file for "TODO"):
  1. LinkedIn + email links in the hero (commented out below)
  2. Optional: enable the contribution snake (see bottom of Analytics section)
  3. Optional: add a live-demo link for waste-Water once you enable GitHub Pages on YOUR repo
-->

<div align="center">

<img src="assets/hero-banner.svg" alt="Amisha Gupta, lavender editorial portfolio cover. Software Engineering, Full-Stack Development, Problem Solving. IIT (BHU), Varanasi." width="100%">

<br>

*Building things, solving problems & figuring out how everything works.*

**[GitHub](https://github.com/amisha6604)**
<!-- TODO: add LinkedIn →  &nbsp;·&nbsp; **[LinkedIn](https://www.linkedin.com/in/YOUR-HANDLE)** -->
<!-- TODO: add email    →  &nbsp;·&nbsp; **[Email](mailto:YOUR-EMAIL)** -->

<img src="assets/divider.svg" alt="" width="100%">

</div>

<img src="assets/section-about.svg" alt="Section 01: A little about Amisha" width="100%">

I'm a student at **IIT (BHU), Varanasi**, studying Biochemical Engineering, and I keep gravitating toward software. I like knowing how things work under the hood: why a shortest-path search picks the route it does, what a session cookie is actually protecting, how a map tile ends up on screen.

Lately that has meant building **MahaPath**, a full-stack discovery platform, and **Voyara**, a travel-routing engine written in C++. I'm aiming for software engineering roles where I can work across algorithms, backend systems and thoughtful design.

`Software development` `Algorithms` `Backend engineering` `System design`

<table>
  <tr><td><b>Studying</b></td><td>Biochemical Engineering, IIT (BHU) Varanasi, since 2022</td></tr>
  <tr><td><b>Interested in</b></td><td>Backend engineering, graph algorithms, system design</td></tr>
  <tr><td><b>Beyond the code</b></td><td>Painting, calligraphy, travelling, dance, badminton</td></tr>
</table>

**Toolbox** <sub>(only what's actually in my repositories)</sub>

<table>
  <tr><td><b>Languages</b></td><td><code>JavaScript</code> <code>C++17</code> <code>Python</code> <code>SQL</code> <code>HTML/CSS</code></td></tr>
  <tr><td><b>Backend</b></td><td><code>Node.js</code> <code>Express</code> <code>REST APIs</code> <code>Session auth</code> <code>bcrypt</code> <code>express-validator</code> <code>Rate limiting</code> <code>CSRF protection</code></td></tr>
  <tr><td><b>Databases</b></td><td><code>MongoDB</code> <code>Mongoose</code> <code>PostgreSQL</code></td></tr>
  <tr><td><b>Frontend</b></td><td><code>EJS</code> <code>Bootstrap 5</code> <code>Leaflet.js</code> <code>Vanilla JS</code></td></tr>
  <tr><td><b>Algorithms</b></td><td><code>Dijkstra</code> <code>Label-setting search</code> <code>Haversine distance</code> <code>Brute-force oracle testing</code></td></tr>
  <tr><td><b>Tools</b></td><td><code>Git &amp; GitHub</code> <code>Make / g++</code> <code>Faker (data seeding)</code></td></tr>
</table>

<img src="assets/divider.svg" alt="" width="100%">

<img src="assets/section-projects.svg" alt="Section 02: Selected projects" width="100%">

<a href="https://github.com/amisha6604/MahaPath"><img src="assets/project-mahapath.svg" alt="MahaPath project card. Illustration of a map with the Sangam river confluence and pins for events, ghats, medical and parking. Node.js, Express 5, MongoDB, Mongoose, EJS, Leaflet.js, Bootstrap 5." width="100%"></a>

**The problem.** Mahakumbh is crowded, unfamiliar and time-sensitive. Visitors need events, facilities, safety information and help in one place instead of scattered across notice boards and word of mouth.

**What's in the code**
- **Accounts and roles:** session auth with bcrypt-hashed passwords, three roles (visitor, organizer, admin), an admin dashboard, pending-request management and an admin-assisted password reset.
- **Events and maps:** event CRUD, event detail pages with a location map, and an interactive Leaflet/OpenStreetMap view over 23 curated Prayagraj-area locations.
- **Nearest facility:** finds the closest hospital, police booth, parking or medical camp using Haversine distance.
- **Safety and help:** incidents, traffic advisories and diversions, crowd reports, lost & found, helplines, FAQs and a rule-based help chatbot.
- **Everything else:** global search, pagination, notification banners, text-size accessibility controls, and seeded demo content.
- **Hardening:** `express-validator` input validation, `express-rate-limit`, and custom CSRF-token middleware on state-changing forms.

**Engineering decisions** <sub>(from my own [`SYSTEM_DESIGN.md`](https://github.com/amisha6604/MahaPath/blob/main/SYSTEM_DESIGN.md))</sub>
- **MVC with server-rendered EJS** instead of a separate SPA: one codebase, one deploy, simpler session auth. The tradeoff is a less app-like feel.
- **One unified `Facility` model** with a `type` enum rather than five collections: no schema duplication, at the cost of slightly looser type-specific validation.
- **Append-only `CrowdReport`:** every report is a new document, so history is preserved and concurrent updates can't race. "Current" density is derived from the latest report.

**Status:** runs locally (Node + MongoDB). There is no public deployment yet.

[**View project →**](https://github.com/amisha6604/MahaPath)

<br>

<a href="https://github.com/amisha6604/Voyara"><img src="assets/project-voyara.svg" alt="Voyara project card. Illustration of a flight graph between airports with a highlighted best route, a dashed alternative and constraint labels. C++17, Dijkstra, Node.js, Express, PostgreSQL." width="100%"></a>

**The problem.** Booking sites rank flights by one number. Voyara is an *algorithm-first* experiment: given flights and what a traveller cares about (money, time, number of stops), produce ranked, explainable itineraries that respect real constraints like connection times.

**What's implemented**
- **C++17 routing engine** on a flight graph with **time-dependent edges**: a connection is only valid if the next departure leaves after the previous arrival plus a minimum connection time.
- **Label-setting Dijkstra with dominance pruning**, optimizing either **price** or **elapsed journey time** (first departure to final arrival, layovers included).
- **Hard constraints:** budget, max stops, max travel time, min connection time and max layover.
- **Alternative itineraries:** returns up to 50 feasible candidates, ordered deterministically (rules documented in `docs/engine_search_semantics.md`).
- **Multi-objective scoring module** (cost, time, stops) with unit tests. It is not exposed through the API yet.
- **Node/Express API** (`/api/health`, `/api/flights`, `/api/route`) that runs the engine as a subprocess over a documented JSON protocol, backed by an 11-table **PostgreSQL** schema and seed loader.
- **Data:** 15 airports, 36 flights and 41 hotels of seed data, plus a generated 500-airport / 3,000-flight benchmark set.

**Testing.** One command (`make test`) builds the engine and runs scorer unit tests, 28 JSON fixture scenarios, and two **brute-force oracle checks** that compare the engine against exhaustive search on random networks, including candidate mode.

**An honest engineering call.** My decision log records that classic bitmask-DP TSP is a good DSA exercise but *not* a faithful model of real multi-city flight routing, which is time-aware. I'd rather build the right model than claim the neat one.

**Status:** actively in development.
**On the roadmap, not built yet:** A\* with a benchmark against Dijkstra, multi-city planning, and the React + Vite + Tailwind frontend. (The repo README lists these in its stack; the code today is the engine, API and data layer.)

[**View project →**](https://github.com/amisha6604/Voyara)

<br>

<a href="https://github.com/amisha6604/waste-Water"><img src="assets/project-wastewater.svg" alt="waste-Water project card. Illustration of a lab beaker with labels for BOD, COD, DO, TSS/TDS, pH and trickling filter. HTML, CSS, JavaScript." width="100%"></a>

**What it does.** A browser-based wastewater engineering calculator, a small tool that turns lab and design formulas into inputs and results. Screens for **BOD** (multi-sample average with dilution), **organic solids** (TS/TDS/TSS/TVS/TFS from crucible weights), **COD** (FAS titration), **dissolved oxygen** (titration), a **water-fitness check** (pH, temperature, alkalinity, TDS), a **rotating biological contactor** and a **trickling filter**.

**Implementation.** Plain HTML, CSS and JavaScript, with no framework: a multi-screen UI driven by DOM events. It's an early project (built in November 2024, about 1,100 lines),, and an early step in turning engineering formulas into software.

[**View project →**](https://github.com/amisha6604/waste-Water)
<!-- TODO (optional): enable GitHub Pages on YOUR repo (Settings → Pages), then add:  &nbsp;·&nbsp; [**Live demo →**](https://amisha6604.github.io/waste-Water/) -->

<img src="assets/divider.svg" alt="" width="100%">

<img src="assets/section-journey.svg" alt="Section 03: Little by little, building my way into engineering." width="100%">

<img src="assets/journey-timeline.svg" alt="Timeline. 2022: joined IIT (BHU) Varanasi for Biochemical Engineering. Nov 2024: first public project, a wastewater calculator. Aug 2026: began MahaPath and Voyara. Sep 2026: MahaPath grows into a platform with roles, admin, maps, validation and CSRF protection. Sep to Oct 2026: Voyara gains time-dependent constrained routing, alternatives and oracle tests. Now: system design, backend architecture and DSA." width="100%">

<img src="assets/divider.svg" alt="" width="100%">

<img src="assets/section-workbench.svg" alt="Section 04: Currently on my workbench" width="100%">

<table>
  <tr><td width="24%"><b>Building</b></td><td><b>Voyara:</b> the C++ engine, most recently generating alternative feasible itineraries.<br><b>MahaPath:</b> polishing the platform and its seeded content.</td></tr>
  <tr><td><b>Learning</b></td><td>System design, backend architecture, DSA and problem-solving. I'm studying forks of <a href="https://github.com/amisha6604/system-design-primer">system-design-primer</a> and <a href="https://github.com/amisha6604/Complete-Python-Bootcamp">Complete-Python-Bootcamp</a> as learning material (not my own work).</td></tr>
  <tr><td><b>Recent challenge</b></td><td>Routing over a <i>time-dependent</i> flight graph. With connection and layover rules, the cheapest or fastest partial route isn't always the one worth keeping. Voyara keeps labels and prunes only dominated ones, then checks results against a brute-force oracle.</td></tr>
  <tr><td><b>Exploring next</b></td><td>A* and a benchmark against Dijkstra, multi-city planning, a React frontend for Voyara, and deploying MahaPath.</td></tr>
</table>

<sub>Last updated: November 2026. Edit the table above by hand whenever things change.</sub>

<img src="assets/divider.svg" alt="" width="100%">

<img src="assets/section-analytics.svg" alt="Section 05: Live GitHub analytics" width="100%">

<p align="center">
  <img height="170" src="https://github-readme-stats.vercel.app/api?username=amisha6604&show_icons=true&hide_border=false&border_radius=14&bg_color=F6F0FC&border_color=C9B6EC&title_color=6B5B95&text_color=2E1A3C&icon_color=8E7CC3" alt="GitHub stats for amisha6604 (loads from github-readme-stats; open github.com/amisha6604 if it doesn't appear)">
  <img height="170" src="https://github-readme-stats.vercel.app/api/top-langs/?username=amisha6604&layout=compact&langs_count=6&border_radius=14&bg_color=F6F0FC&border_color=C9B6EC&title_color=6B5B95&text_color=2E1A3C" alt="Most-used languages for amisha6604 (loads from github-readme-stats; open github.com/amisha6604 if it doesn't appear)">
</p>

<!--
  OPTIONAL contribution snake (self-hosted, no third-party service at view time):
  1. Push .github/workflows/snake.yml, then Actions → "Generate Snake" → Run workflow.
  2. After it succeeds, uncomment the block below.

<p align="center">
  <img src="https://raw.githubusercontent.com/amisha6604/amisha6604/output/github-contribution-grid-snake.svg" alt="Contribution graph with an animated snake">
</p>
-->

<img src="assets/divider.svg" alt="" width="100%">

<img src="assets/section-beyond.svg" alt="Section 06: Outside the terminal" width="100%">

<img src="assets/beyond-code.svg" alt="Scrapbook cards: painting, calligraphy, travelling, dance and badminton." width="100%">

<div align="center">

<img src="assets/divider.svg" alt="" width="100%">

<sub>Thanks for stopping by. Always happy to talk about algorithms, backend design or a good paintbrush.</sub>

</div>
