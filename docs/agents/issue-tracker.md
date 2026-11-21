# Issue tracker: ClickUp

Issues and specs for this repo live as ClickUp tasks, reached through ClickUp's MCP
server (`https://mcp.clickup.com/mcp`, tools named `clickup_*`). Auth is OAuth only —
there is no API-key mode — and every call acts as the signed-in user with that user's
ClickUp permissions.

## Where this repo's work lives

- **Workspace**: `90122206326`
- **Space**: `Espacio del equipo [ES]`, id `901211033073`
- **List**: `JavaDocs`, id `1500910000001458`
  (<https://app.clickup.com/90122206326/v/l/li/1500910000001458>)

Resolve ids with `clickup_get_workspace_hierarchy` (Space → Folder → List) and
`clickup_get_list` (accepts `list_name` or `list_id`). Never guess an id.

### This List's statuses

Statuses are defined on the folder `Mío` (`1500910000000649`) and are in English:

| Status        | Type     | Meaning                    |
| ------------- | -------- | -------------------------- |
| `to do`       | `open`   | Not started / needs triage |
| `in progress` | `custom` | Being worked on            |
| `deployed`    | `custom` | Shipped, not yet archived  |
| `complete`    | `closed` | Done                       |

Setting a status to `complete` is how a ticket gets closed.

## Conventions

- **Create an issue**: `clickup_create_task` with `name`, `list_id` and
  `markdown_description` (Markdown). Pass `parent` for a subtask and `tags: [...]` to
  label it. Repeating the call creates a duplicate.
- **Read an issue**: `clickup_get_task` with `task_id`; add `expand_statuses` when you
  need the List's valid statuses, `include` for extra fields.
- **List issues**: `clickup_filter_tasks` filtered by `lists`/`spaces`, `statuses`,
  `tags`, `assignees`. It pages — pass `next_cursor` back as `cursor`.
- **Comment**: `clickup_create_comment` with `entity_id` = the task id (or
  `clickup_create_task_comment`). Read back with `clickup_get_task_comments`, then
  `clickup_get_threaded_comments` for replies.
- **Apply / remove a label**: ClickUp has no issue labels — use Space-wide **tags**.
  `clickup_create_task` creates any missing tag it is given (lowercased), but
  `clickup_add_tag_to_task` requires the tag to exist first, so call
  `clickup_create_space_tag` (`space_id`, `name`) when unsure. Remove with
  `clickup_remove_tag_from_task` (`task_id`, `tag_name`).
- **Blocking**: `clickup_add_task_dependency` with `task_id`, `depends_on` and `type`
  (`waiting on` / `blocking`). A ticket is unblocked when everything it waits on is
  closed.
- **Close**: there is no single "close" verb. Post the explanation as a comment, then
  set `status: "complete"` with `clickup_update_task`. `clickup_delete_task` trashes
  the task and its subtasks — never use it for triage.
- **Search**: `clickup_search` across tasks, Docs, chats and more.

## Gotchas

- **Statuses are defined per List.** An unknown status name fails. Read them first and
  never invent one.
- **Tags must already exist in the Space** before `clickup_add_tag_to_task` accepts
  them.
- **Rate limits** are per Workspace per client per rolling 24 hours, by plan (Free 100,
  Unlimited 300, Business 1,000, Business Plus 2,500, Enterprise 5,000). Over-limit
  calls return `RATE_LIMIT_EXCEEDED` with `retryAfter`. Prefer `clickup_filter_tasks`
  and `task.update_many` over loops.
- **Writes duplicate on retry.** After a write timeout, check whether it landed before
  retrying. Reads are safe to retry.
- **Multiple workspaces**: pass `workspace_id` when the signed-in user has more than
  one, otherwise the server lists the choices.
- **Archiving is reversible** via `clickup_execute_operator`; prefer it over deletion.
- **Quote tasks as inline Markdown links** with the task name as the anchor text, e.g.
  `[Fix login](https://app.clickup.com/t/abc123)`. Never emit a bare task URL.
- **Relative dates**: call `clickup_get_user` and compute from its `current_time`/
  timezone rather than the local clock.

## Pull requests as a triage surface

**PRs as a request surface: no.** The repo is under git (`main`, remote
`https://github.com/pdiiazz/java-from-zero-to-hero`) and the rebuild is committed on
`main` without a PR. Set this to `yes` if PRs start being used.

## When a skill says "publish to the issue tracker"

Create a ClickUp task in the List above with `clickup_create_task`, then report its URL.

## When a skill says "fetch the relevant ticket"

Read it with `clickup_get_task`, plus `clickup_get_task_comments` when the conversation
matters. The user will normally pass a task id or URL.

## Wayfinding operations

Used by `/wayfinder`. The **map** is a task with **subtasks** as tickets.

- **Map**: a task tagged `wayfinder:map`, its description holding Notes /
  Decisions-so-far / Fog. Create the tag per Space first.
- **Child ticket**: a subtask (`parent` = map id) tagged `wayfinder:<type>`
  (`research` / `prototype` / `grilling` / `task`), the question as its description.
  Record blockers with `clickup_add_task_dependency` (`type: waiting on`).
- **Frontier**: `clickup_filter_tasks` for the map's open subtasks; drop assigned ones
  and those with an open blocker; earliest in map order wins.
- **Claim**: `clickup_update_task` assigning the ticket to the signed-in user
  (`clickup_get_user` / `clickup_resolve_assignees` for the id). First write of the
  session.
- **Resolve**: comment the answer, set the closed status, then append a context pointer
  (gist + task URL) to the map's Decisions-so-far.
