# Write your own rules

Every diagnosis the plugin makes comes from a rule, and rules are plain JSON — not compiled code. You can
add your own for your project's own budgets without recompiling anything.

## Point the plugin at your file

Go to **Project Settings → Plugins → Threadbound Radar → Custom Rules** and set **Custom Rules File
Path** to a `.json` file inside your project.

Your rules are loaded **on top of** the built-in ones at startup, never instead of them. If one of your
ids collides with a built-in id, yours is skipped with a warning in the log and the built-in rule wins —
so pick a prefix of your own, like `MY_001`, and collisions stop being a concern.

!!! tip "Restart to reload"
    The file is read once at startup. After editing it, restart the editor to see your changes.

## The shape of a rule

The file is a JSON array of rule objects:

```json
[
  {
    "id": "MY_001",
    "name": "Wave spawns blow our handheld budget",
    "category": "blueprint",
    "severity": "high",
    "editor_states": ["playing"],
    "conditions": {
      "game_thread_ms": { "gt": 22 },
      "blueprint_total_tick_cost_ms": { "gt": 5 }
    },
    "cause": "Blueprint Tick is costing {blueprint_total_tick_cost_ms} on a frame our handheld target cannot afford.",
    "solutions": [
      "Stagger the wave spawner so enemies activate over several frames",
      "Move enemy logic that does not need every frame from Tick to a Timer"
    ],
    "docs_url": "https://example.com/our-wiki/handheld-budget"
  }
]
```

| Field | Required | Values |
|---|---|---|
| `id` | yes | Unique string. Use your own prefix |
| `name` | yes | Short title shown in the diagnostics table |
| `category` | yes | `rendering`, `cpu`, `memory`, `blueprint`, `general` |
| `severity` | yes | `high`, `medium`, `low` |
| `editor_states` | no | Which editor states allow the rule to fire. **Omitting it means all of them** |
| `conditions` | yes | Measurements that must all be true — see below |
| `cause` | yes | One or two sentences on why this happens |
| `solutions` | yes | Array of concrete actions, most useful first |
| `docs_url` | no | Link shown for further reading |

!!! warning "A missing `editor_states` field means *all* of them, not *playing*"
    This is deliberate. A field you forgot should not silently narrow your rule — a rule that stopped
    firing outside Play would be indistinguishable from a broken one.

## Conditions

`conditions` is an object of `metric → { operator: value }`. **All of them must be true at the same
time**: the engine only combines with AND. If you need OR, write two rules.

Five operators are supported:

| Operator | Meaning |
|---|---|
| `gt` | greater than |
| `gte` | greater than or equal |
| `lt` | less than |
| `lte` | less than or equal |
| `eq` | equal |

### Metrics you can use

Units follow the suffix: `_ms` milliseconds, `_mb` megabytes, `_pct` percent.

**Frame and threads**

`frame_time_ms` · `game_thread_ms` · `render_thread_ms` · `gpu_time_ms` · `gpu_bound` (use `{ "eq": true }`) · `hitch_count`

**Rendering**

`draw_calls` · `mesh_draw_calls` · `triangle_count` · `triangles_per_mesh_draw` · `texture_memory_mb` ·
`tsr_cost_ms` · `sky_atmosphere_cost_ms` · `volumetric_cloud_cost_ms` · `niagara_gpu_cost_ms`

**Blueprint**

`blueprint_total_tick_cost_ms` · `blueprint_max_tick_cost_ms` · `blueprint_tick_class_count`

**Memory**

`system_memory_free_mb` · `system_memory_free_pct` · `process_memory_growth_mb_per_min`

**Level scan** — facts about the loaded level rather than the current frame

`level_meshes_missing_lods` · `level_meshes_weak_lods` · `level_meshes_missing_nanite` ·
`level_instancing_candidates` · `level_landscape_components` · `level_landscape_nanite`

## Quote the measured value in your text

Inside `cause`, wrap a metric name in braces and it is replaced by whatever actually triggered the rule,
with its unit:

```json
"cause": "Free system memory dropped to {system_memory_free_mb}, so the editor is paging."
```

This is better than writing the threshold by hand, because a hand-written number goes stale the moment
someone edits the setting it refers to — and saying what was measured is the more useful sentence anyway.

## Keep rules cheap

Rules are evaluated about once per second, on purpose: the plugin is meant to stay on for a whole
session without changing the performance you are trying to measure. Write conditions that read metrics,
not conditions that need work to compute.

## A checklist before you save

- [ ] The `id` uses your own prefix
- [ ] Every metric name is spelled exactly as listed above — an unknown name simply never matches
- [ ] The rule makes sense in the editor states you declared
- [ ] `solutions` are actions, not observations
