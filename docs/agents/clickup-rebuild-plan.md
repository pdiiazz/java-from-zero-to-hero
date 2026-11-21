# ClickUp rebuild plan — staged, NOT applied

Status: **staged only**. The ClickUp MCP quota was exhausted at 100/100 on
2026-11-21 (~5 h reset). Do not report the tasks as updated until every step
below has been applied and verified against the live list.

## Targets

- Workspace `90122206326`, Space `Espacio del equipo [ES]` `901211033073`,
  Folder `Mío` `1500910000000649`, List `JavaDocs` `1500910000001458`.
- User Pablo `246274597` (Europe/Madrid).
- Valid statuses: `to do` (open), `in progress` (custom), `deployed` (custom),
  `complete` (closed). Never use the old Spanish labels.
- New tickets must go through `clickup_execute_operator` with
  `model: "task"`, `operator: "create_many"`, `parameters: {list_id: 1500910000001458}`,
  `body: {tasks: [...], task_type_id: 0}` — see D19 in `docs/wayfinder/map.md`.

## Apply order

1. **Check quota first.** Call `clickup_get_user`. If it returns
   `RATE_LIMIT_EXCEEDED`, stop; do not retry writes.
2. **Ensure the `wontfix` tag exists** with `clickup_create_space_tag`
   (`space_id: "901211033073"`, `name: "wontfix"`). It is idempotent.

3. **Rename and re-describe the existing module tasks in place** with
   `clickup_update_task` (`task_id`, `name`, `markdown_description`). Keep their
   IDs, watchers and comments.

   | Id | New name | Description to set |
   | -- | -------- | ------------------ |
   | `1an27bp8nfw` | M00 Getting started (1 topic) | `m00-01-run-java`. Gradle moved to M01. |
   | `1an27bp8nfx` | M01 The platform (6 topics) | `m01-01-design-goals`, `m01-02-jvm-bytecode`, `m01-03-memory-and-gc`, `m01-04-releases`, `m01-05-jpms`, `m01-06-gradle` (Gradle Kotlin DSL, documentation only). |
   | `1an27bp8nfy` | M02 Language fundamentals (7 topics) | `m02-01-primitives` … `m02-07-arrays`. |
   | `1an27bp8nfz` | M03 Text, numbers and time (5 topics) | `m03-01-string`, `m03-02-text-blocks-formatting`, `m03-03-bigdecimal-biginteger`, `m03-04-regex`, `m03-05-datetime`. |
   | `1an27bp8ng0` | M04 Object-oriented programming (13 topics, 3 parts) | Parts modeling/inheritance/data-carriers; includes `m04-13-pattern-matching` moved from old M11 and the golden sample `m04-05-inheritance-super`. |
   | `1an27bp8ng1` | M05 Type system (5 topics) | Generics, wildcards, erasure, annotations, reflection. |
   | `1an27bp8ng2` | M06 Collections (5 topics) | `m06-01-collection-contracts` … `m06-05-optional`. |
   | `1an27bp8ng3` | M07 Functional Java (4 topics) | Lambdas, streams, collectors, parallelism/gatherers. |
   | `1an27bp8ng4` | M08 Errors and resources (2 topics) | `m08-01-exceptions`, `m08-02-try-with-resources`. |
   | `1an27bp8ng5` | M09 I/O and files (6 topics) | `m09-01-path-files` … `m09-04-serialization`, `m09-05-json` (Jackson), `m09-06-http` (`java.net.http.HttpClient`). JDBC moved to M10; sockets closed. |
   | `1an27bp8ng6` | M11 Concurrency (6 topics) | Was M10. `m11-01-threads` … `m11-06-scoped-values`. |
   | `1an27bp8ng7` | M12 Language evolution (1 topic + appendix) | Was M11. `m12-01-feature-map` plus `appendix-preview-features`. Pattern matching moved to M04. |
   | `1an27bp8ng8` | M13 Design, testing and diagnostics (4 topics) | Was M12. Order `m13-01-solid`, `m13-02-design-patterns`, `m13-03-logging-diagnostics`, `m13-04-testing` (testing last). |
   | `1an27bp8nft` | Build repo skeleton (14 modules) | Folder layout `00-getting-started` … `13-design-testing`, README index, `_template.md`, version matrix, LTS checklist. Acceptance: 14 module READMEs and 66 topic stubs + appendix present. |
   | `1an27bp8nfu` | Generate 66 topic stubs + appendix | One file per topic id from `docs/wayfinder/curriculum-draft.md` with YAML frontmatter and the seven fixed headings. |

4. **Reopen the two completed scaffolding tasks** with `clickup_update_task`
   (`status: "in progress"`). After step 9 verifies the repo state, set both to
   `complete` and comment "Rebuilt for the 14-module curriculum on 2026-11-21."

5. **Close the sockets outlier** `1an27bp8nj2`:
   - `clickup_create_comment` (`entity_id: "1an27bp8nj2"`): "Closed as wontfix:
     raw sockets are out of scope (D20). Superseded by `m09-05-json` and
     `m09-06-http` under the revised M09."
   - `clickup_add_tag_to_task` (`task_id`, `tag_name: "wontfix"`).
   - `clickup_update_task` (`status: "complete"`).

6. **Create the tracking parent**, using `task.create_many` so the default task
   type is used (D19). Set `parent_task_id: "1an27bp8nfr"` so it becomes a child
   of the wayfinder map. Record the returned id as `NEW-PARENT-ID`:

   ```json
   {
     "name": "Rebuild repo skeleton for 14 modules (M00–M13)",
     "description": "Rebuild the documentation skeleton to the approved 14-module / 66-topic + appendix curriculum. Subtasks cover M00–M13. Includes the Gradle move to M01, JDBC extraction to M10, pattern matching move to M04, JSON + HTTP in M09 and testing last in M13.",
     "task_type_id": 0,
     "assignee_ids": [246274597],
     "parent_task_id": "1an27bp8nfr"
   }
   ```

7. **Create the new M10 task and the 14 module subtasks in one `task.create_many`
   call.** For the 14 subtasks use `parent_task_id: NEW-PARENT-ID`; for the M10
   curriculum task use `parent_task_id: "1an27bp8nfr"`. All with
   `task_type_id: 0`.

   | Name | Parent | Description |
   | ---- | ------ | ----------- |
   | M00 Getting started (1 topic) | NEW-PARENT-ID | `m00-01-run-java`. |
   | M01 The platform (6 topics) | NEW-PARENT-ID | Includes `m01-06-gradle`. |
   | M02 Language fundamentals (7 topics) | NEW-PARENT-ID | `m02-01` … `m02-07`. |
   | M03 Text, numbers and time (5 topics) | NEW-PARENT-ID | `m03-01` … `m03-05`. |
   | M04 Object-oriented programming (13 topics, 3 parts) | NEW-PARENT-ID | Includes `m04-13-pattern-matching`. |
   | M05 Type system (5 topics) | NEW-PARENT-ID | `m05-01` … `m05-05`. |
   | M06 Collections (5 topics) | NEW-PARENT-ID | `m06-01` … `m06-05`. |
   | M07 Functional Java (4 topics) | NEW-PARENT-ID | `m07-01` … `m07-04`. |
   | M08 Errors and resources (2 topics) | NEW-PARENT-ID | `m08-01`, `m08-02`. |
   | M09 I/O and files (6 topics) | NEW-PARENT-ID | Adds JSON (Jackson) and HTTP (`HttpClient`). |
   | M10 Data access with JDBC (1 topic) | NEW-PARENT-ID | `m10-01-jdbc`. |
   | M11 Concurrency (6 topics) | NEW-PARENT-ID | Renumbered from old M10. |
   | M12 Language evolution (1 topic + appendix) | NEW-PARENT-ID | Renumbered from old M11. |
   | M13 Design, testing and diagnostics (4 topics) | NEW-PARENT-ID | Testing last. |
   | M10 Data access with JDBC (1 topic) | `1an27bp8nfr` | **Curriculum task** — `m10-01-jdbc`, extracted from old M09. |

   Note: the two "M10 Data access with JDBC (1 topic)" rows intentionally exist
   at different levels — one is a subtask of the rebuild tracker, one is the
   curriculum module task alongside the other module tasks.

8. **Update the umbrella task** `1an27bp8nfr` with `clickup_update_task`:
   `markdown_description` describing the 14-module / 66-topic + appendix
   curriculum, pointing at `docs/wayfinder/map.md` and
   `docs/wayfinder/curriculum-draft.md`, and noting the new tracking task id.

9. **Link the tracker to the two scaffolding tasks** with
   `clickup_add_task_link` (`task_id: NEW-PARENT-ID`,
   `links_to: "1an27bp8nft"` and `"1an27bp8nfu"`).

10. **Verify**: `clickup_get_task` on `1an27bp8nfr` (expect 20 children), on
    `NEW-PARENT-ID` (expect 14 children), and `clickup_filter_tasks` on the list
    to confirm no stale module names or Spanish statuses remain. Then set the
    two scaffolding tasks to `complete` per step 4.

## Reversibility

No task is deleted or archived. The only closed-and-tagged task is
`1an27bp8nj2`, and the reason is recorded in a comment.
