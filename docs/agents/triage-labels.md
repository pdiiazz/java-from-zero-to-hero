# Triage Labels

The skills speak in terms of five canonical triage roles. This file maps those roles to
the actual strings used in this repo's issue tracker.

In ClickUp there are no issue labels: these strings are **Space-wide tags** in the
`Espacio del equipo [ES]` Space (`901211033073`). Create any missing one with
`clickup_create_space_tag`, then apply or remove with `clickup_add_tag_to_task` /
`clickup_remove_tag_from_task`. The five below have already been created.

| Label in mattpocock/skills | Label in our tracker | Meaning                                  |
| -------------------------- | -------------------- | ---------------------------------------- |
| `needs-triage`             | `needs-triage`       | Maintainer needs to evaluate this issue  |
| `needs-info`               | `needs-info`         | Waiting on reporter for more information |
| `ready-for-agent`          | `ready-for-agent`    | Fully specified, ready for an AFK agent  |
| `ready-for-human`          | `ready-for-human`    | Requires human implementation            |
| `wontfix`                  | `wontfix`            | Will not be actioned                     |

When a skill mentions a role (e.g. "apply the AFK-ready triage label"), use the
corresponding string from the "Label in our tracker" column.

Edit the "Label in our tracker" column to match whatever vocabulary you actually use.
