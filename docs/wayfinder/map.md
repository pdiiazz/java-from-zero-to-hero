# Wayfinder map: Java course documentation

> Tracker status: mirrored to ClickUp list `JavaDocs` (1500910000001458).
> Parent task `1an27bp8nfr` (`wayfinder:map`) carries 20 child tickets: the
> original 19 plus the new skeleton-rebuild parent, which itself has one subtask
> per module (M00–M13). Exact edits are staged in
> `docs/agents/clickup-rebuild-plan.md` until the ClickUp API quota resets.

## Destination

Ship the **blueprint, the repo skeleton and one golden-sample topic** for a Java
course that takes a competent programmer from zero Java to advanced core Java.
The full course body is a separate, later effort; this map ends when the
standard is set and repeatable.

## Locked decisions

| #   | Decision | Answer |
| --- | -------- | ------ |
| D1  | Audience | Programmers who already program, formal-course level. No programming basics. Every topic carries exercises, and advanced concepts get real depth. |
| D2  | Language | English everywhere: prose, identifiers, comments, commit messages. |
| D3  | Ceiling | Core Java only: language, JDK library, toolchain, testing. No frameworks. Spring is a separate future path; leave a seam, nothing more. |
| D4  | Baseline | Java 25 LTS. No previews on the main track; preview appendix plus LTS-to-LTS upgrade path. |
| D5  | Cheat sheet | Translated to English, kept as the quick-reference companion. The Spanish original is mined for coverage. |
| D6  | Freshness | "Addons" was a typo, dropped. Current means new Java/JDK features and standard tooling only. |
| D7  | Form | Documentation first. Markdown course with code snippets; no runnable code repo yet. |
| D8  | Exercises | Statement plus expected output. No test harness, no auto-verification. |
| D9  | Build tool | Gradle (Kotlin DSL) is what the docs teach, in `m01-06-gradle`. No build project: snippets only (D12). |
| D10 | Topic size | ~40–70 topics: one doc of 20–40 minutes plus 3–5 exercises each. |
| D11 | Freshness mechanics | `docs/java-version-matrix.md`, a "new in Java N" appendix, and a manual review checklist run at each LTS. No CI. |
| D12 | No automation | No CI, no build project, no compiled examples. Code appears only as illustrative snippets in the docs. |
| D13 | Future consumer | The docs are published in a public GitHub repo and will later feed a course interface as a database. Every topic therefore carries stable ids, YAML frontmatter and fixed section headings from the first commit. |
| D14 | Topic model | One file per topic with fixed sections: Why it exists → Syntax → Examples → Gotchas → Exercises → Solutions → References. |
| D15 | OOP as one module | Objects, inheritance, polymorphism, abstraction, nested classes, sealed types, records, enums and pattern matching live in one module (M04, 13 topics) split into three `part` values for navigation. No separate inheritance module. |
| D16 | Coverage source | The Spanish cheat sheet is the coverage baseline: all 58 sections mapped to a topic or an explicit omission. Gaps (Java 9–25 features, `java.time`, regex, GC, diagnostics, testing, build tools, JSON, HTTP) are added as first-class topics. |
| D17 | Count | 66 topics + 1 appendix across 14 modules. **Approved by the user** in the rebuild decision. |
| D18 | JDBC | `m10-01-jdbc` is a standalone module (M10). It is JDK-only (`java.sql`), no framework, and teaches `PreparedStatement`, transactions and resource handling. The future database track builds on it instead of repeating it. |
| D19 | Task creation | `clickup_create_task` fails workspace-wide with `Max usage for custom task types reached` (Free plan quota). Workaround: the `task.create_many` operator with `task_type_id: 0` (the built-in default type). Use that path for every new ticket. |
| D20 | Networking | Raw sockets and WebSockets stay out of the core course; the old `m09-06-sockets` ticket is closed as `wontfix`. HTTP is now core via the JDK's `java.net.http.HttpClient` (`m09-06-http`), and JSON is taught with Jackson (`m09-05-json`). A WebSocket *server* still means Jakarta/Netty/Spring, which breaks D3. |
| D21 | Testing order | Testing is the last topic of M13 (`m13-04-testing`), after SOLID, design patterns and diagnostics. |

## Notes

- Existing material: `Java0aAvanzadoCheatSheet.md`, Spanish, 58 sections, 1,835 lines.
  Confirmed gaps: no build tools, testing, JPMS, text blocks, switch expressions,
  sealed classes, pattern matching, virtual threads, or anything from Java 22+.
- The repo **is** under git: one commit on `main`, remote
  `https://github.com/pdiiazz/java-docs.git`.

## Decisions so far

- **Syllabus approved by the user**: 14 modules, 66 topics + preview appendix.
- Gradle in M01, JDBC in M10, pattern matching in M04, JSON and HTTP in M09.
- Curriculum draft: `docs/wayfinder/curriculum-draft.md`.
- OOP consolidated into M04 with three parts: modeling, inheritance,
  data carriers.
- Cheat sheet mapped section by section; corrections list kept in the draft.
- Skeleton rebuilt locally: 14 module folders, 66 topic stubs, appendix stub,
  golden sample at `04-oop/m04-05-inheritance-super.md`.
- Ticket edits staged in `docs/agents/clickup-rebuild-plan.md` (ClickUp quota
  exhausted on 2026-11-21; retry after reset).

## Fog

- Depth calibration: which advanced topics earn their own topic versus a section.
- Translation and linking of the 1,835-line Spanish cheat sheet.

## Tracker

List `JavaDocs` (1500910000001458), space `901211033073`, folder `Mío`.

Statuses in use (folder-level): `to do` (open), `in progress` (custom),
`deployed` (custom), `complete` (closed).

Parent: `Java course docs — wayfinder map`, id `1an27bp8nfr`, tagged
`wayfinder:map`, assigned to Pablo.

| # | Ticket | Id | Status after rebuild |
| - | ------ | -- | -------------------- |
| 1 | Build repo skeleton (14 modules) | `1an27bp8nft` | `in progress` → `complete` |
| 2 | Generate 66 topic stubs + appendix | `1an27bp8nfu` | `complete` |
| 3 | Write golden sample topic `m04-05-inheritance-super` | `1an27bp8nfv` | `to do` |
| 4 | M00 Getting started (1 topic) | `1an27bp8nfw` | `to do` |
| 5 | M01 The platform (6 topics) | `1an27bp8nfx` | `to do` |
| 6 | M02 Language fundamentals (7 topics) | `1an27bp8nfy` | `to do` |
| 7 | M03 Text, numbers and time (5 topics) | `1an27bp8nfz` | `to do` |
| 8 | M04 Object-oriented programming (13 topics, 3 parts) | `1an27bp8ng0` | `to do` |
| 9 | M05 Type system (5 topics) | `1an27bp8ng1` | `to do` |
| 10 | M06 Collections (5 topics) | `1an27bp8ng2` | `to do` |
| 11 | M07 Functional Java (4 topics) | `1an27bp8ng3` | `to do` |
| 12 | M08 Errors and resources (2 topics) | `1an27bp8ng4` | `to do` |
| 13 | M09 I/O and files (6 topics) | `1an27bp8ng5` | `to do` |
| 14 | M10 Data access with JDBC (1 topic) | *new* | `to do` |
| 15 | M11 Concurrency (6 topics) | `1an27bp8ng6` | `to do` |
| 16 | M12 Language evolution (1 topic + appendix) | `1an27bp8ng7` | `to do` |
| 17 | M13 Design, testing and diagnostics (4 topics) | `1an27bp8ng8` | `to do` |
| 18 | Translate the cheat sheet to English | `1an27bp8ng9` | `to do` |
| 19 | Initialise git and publish to GitHub | `1an27bp8nga` | `in progress` |
| 20 | Rebuild repo skeleton for 14 modules (M00–M13) | *new* | `to do` |
| — | Add m09-06-sockets (closed: superseded) | `1an27bp8nj2` | `complete` + `wontfix` |

## Resolved blocker

`Max usage for custom task types reached` blocked every `clickup_create_task`
call, workspace-wide, because the Free plan quota for custom task types is
exhausted and every List defaults to the custom type `Task`.

Resolution: `task.create_many` with `task_type_id: 0` uses the built-in default
type and succeeds. No upgrade and no type merge needed. Recorded as D19; use this
path for every future ticket.

Current blocker: the ClickUp MCP quota was exhausted at 100/100 on 2026-11-21
(retry ~5 h). All writes are staged, not applied.

## Open questions

_(none — the rebuild decisions are closed)_
