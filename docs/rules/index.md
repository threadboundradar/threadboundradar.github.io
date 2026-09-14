# Rules reference

The Rule Engine ships with **54 rules**. Each one is a small piece of data that says: under these
measured conditions, in these editor states, this is the probable cause and these are the fixes.

!!! note "This reference is being written"
    The per-rule pages are still in progress. The taxonomy below is accurate today, and the page for
    each rule will be linked from the plugin itself.

## How a rule is organised

| Field | Meaning |
|---|---|
| **id** | Stable identifier such as `RE_001`. It never changes, so you can search for it |
| **category** | `rendering`, `cpu`, `memory`, `blueprint` or `general` |
| **severity** | `high`, `medium` or `low` |
| **regimes** | Which editor states the rule is allowed to fire in |
| **conditions** | The measurements that must all be true at once |
| **cause** | Why this probably happens |
| **solutions** | What to do about it, in order of usefulness |

## Regimes

A rule only fires in the regimes it declares:

| Regime | The editor is… |
|---|---|
| `playing` | Running a Play-in-Editor session |
| `loading` | Loading a map or streaming packages |
| `compiling` | Compiling shaders |
| `editor_idle` | Doing none of the above |

Most frame-budget rules are limited to `playing`, because the same 55 ms means something different
while a map loads. Level-scan rules are not gated: a composition problem is a fact about the level
whether you are playing or not.

## How to read severity

Severity is about **how much of your frame is at stake**, not about how urgent it is for your project.
A `high` rule in a scene you are about to delete does not matter; a `low` one in your main level might.

## Writing your own

The same schema is open to you — see [Write your own rules](../guides/custom-rules.md).
