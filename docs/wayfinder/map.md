# Wayfinder map: Java course documentation

> Tracker status: mirrored to ClickUp list `JavaDocs` (1500910000001458).
> Parent task `1an27bp8nfr` (`wayfinder:map`) carries 18 child tickets, one per
> unit of work. See "Tracker" below.

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
| D9  | Build tool | Gradle (Kotlin DSL) is what the docs teach and use for snippets. |
| D10 | Topic size | ~40–60 topics: one doc of 20–40 minutes plus 3–5 exercises each. |
| D11 | Freshness mechanics | `docs/java-version-matrix.md`, a "new in Java N" appendix, and a manual review checklist run at each LTS. No CI. |
| D12 | No automation | No CI, no build project, no compiled examples. Code appears only as illustrative snippets in the docs. |
| D13 | Future consumer | The docs are published in a public GitHub repo and will later feed a course interface as a database. Every topic therefore carries stable ids, YAML frontmatter and fixed section headings from the first commit. |
| D14 | Topic model | One file per topic with fixed sections: Why it exists → Syntax → Examples → Gotchas → Exercises → Solutions → References. |
| D15 | OOP as one module | Objects, inheritance, polymorphism, abstraction, nested classes, sealed types, records and enums live in one module (M04, 12 topics) split into three `part` values for navigation. No separate inheritance module. |
| D16 | Coverage source | The Spanish cheat sheet is the coverage baseline: all 58 sections mapped to a topic or an explicit omission. Gaps (Java 9–25 features, `java.time`, regex, GC, diagnostics, testing, build tools) are added as first-class topics. |
| D17 | Count | 64 topics + 1 appendix across 12 modules. Merges preferred over new topics where a section is under ~10 minutes of material. **Approved by the user.** |
| D18 | JDBC | `m09-05-jdbc` stays in core Java: it is JDK-only (`java.sql`), no framework, and it teaches `PreparedStatement`, transactions and resource handling. The future database track builds on it instead of repeating it. |
| D19 | Task creation | `clickup_create_task` fails workspace-wide with `Max usage for custom task types reached` (Free plan quota). Workaround: the `task.create_many` operator with `task_type_id: 0` (the built-in default type). Use that path for every new ticket. |
| D20 | Networking | Sockets, `HttpClient` and WebSockets stay out of the core course. The JDK ships only a WebSocket *client*; a server means Jakarta/Netty/Spring, which breaks D3. WebSockets belong to the future Spring path. |

## Notes

- Existing material: `Java0aAvanzadoCheatSheet.md`, Spanish, 58 topics, 1,835 lines.
  Confirmed gaps: no build tools, testing, JPMS, text blocks, switch expressions,
  sealed classes, pattern matching, virtual threads, or anything from Java 22+.
- No code in the repo yet. Not under git.

## Decisions so far

- **Syllabus approved by the user**: 12 modules, 64 topics + preview appendix.
- JDBC stays in core Java (D18).
- Curriculum draft: `docs/wayfinder/curriculum-draft.md`.
- OOP consolidated into M04 with three parts: modeling, inheritance,
  data carriers.
- Cheat sheet mapped section by section; corrections list kept in the draft.
- Tickets created in ClickUp (see Tracker).

## Fog

- Depth calibration: which advanced topics earn their own topic versus a section.
- Translation and linking of the 1,835-line Spanish cheat sheet.
- Repo layout details: module naming, index/README shape, navigation for 64 topics.

## Tracker

List `JavaDocs` (1500910000001458), space `901211033073`.

Parent: `Java course docs — wayfinder map`, id `1an27bp8nfr`, tagged
`wayfinder:map`, assigned to Pablo, status `pendiente`.

| # | Ticket | Id |
| --- | --- | --- |
| 1 | Build repo skeleton | `1an27bp8nft` |
| 2 | Generate 64 topic stubs | `1an27bp8nfu` |
| 3 | Write golden sample topic `m04-05-inheritance-super` | `1an27bp8nfv` |
| 4 | M00 Getting started (2) | `1an27bp8nfw` |
| 5 | M01 The platform (5) | `1an27bp8nfx` |
| 6 | M02 Language fundamentals (7) | `1an27bp8nfy` |
| 7 | M03 Text, numbers and time (5) | `1an27bp8nfz` |
| 8 | M04 OOP (12) | `1an27bp8ng0` |
| 9 | M05 Type system (5) | `1an27bp8ng1` |
| 10 | M06 Collections (5) | `1an27bp8ng2` |
| 11 | M07 Functional Java (4) | `1an27bp8ng3` |
| 12 | M08 Errors and resources (2) | `1an27bp8ng4` |
| 13 | M09 I/O and files (5) | `1an27bp8ng5` |
| 14 | M10 Concurrency (6) | `1an27bp8ng6` |
| 15 | M11 Language evolution (2 + appendix) | `1an27bp8ng7` |
| 16 | M12 Design, testing and diagnostics (4) | `1an27bp8ng8` |
| 17 | Translate the cheat sheet to English | `1an27bp8ng9` |
| 18 | Initialise git and publish to GitHub | `1an27bp8nga` |

Statuses in use: `pendiente` → `en curso` → `completado`.

## Resolved blocker

`Max usage for custom task types reached` blocked every `clickup_create_task`
call, workspace-wide, because the Free plan quota for custom task types is
exhausted and every List defaults to the custom type `Task`.

Resolution: `task.create_many` with `task_type_id: 0` uses the built-in default
type and succeeds. No upgrade and no type merge needed. Recorded as D19; use this
path for every future ticket.

## Open questions

_(none — Q8 to Q17 answered)_
