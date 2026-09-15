# Rules reference

The Rule Engine ships with **54 rules**. Each one is a small piece of data that says: under these
measured conditions, in these editor states, this is the probable cause and these are the fixes.

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

## All rules

Every rule links to its own page: what it means, when it fires, why it happens and how to fix it.

### Rendering

| Rule | Severity |
|---|---|
| [RE_001 · Too many draw calls without instancing](RE_001.md) | High |
| [RE_002 · Excessive triangle count without Nanite](RE_002.md) | High |
| [RE_003 · High shading cost with few draw calls (60 FPS)](RE_003.md) | Medium |
| [RE_004 · Critical shading cost with few draw calls (30 FPS)](RE_004.md) | High |
| [RE_005 · Sustained GPU-bound (60 FPS)](RE_005.md) | Medium |
| [RE_006 · Critical GPU-bound (30 FPS)](RE_006.md) | High |
| [RE_013 · Dense geometry impacting the GPU without excess draw calls](RE_013.md) | Medium |
| [RE_015 · Possible fill-rate or post-process bound](RE_015.md) | Medium |
| [RE_017 · High draw calls — early warning](RE_017.md) | Low |
| [RE_018 · High triangle count — early warning](RE_018.md) | Low |
| [RE_019 · GPU time elevated within budget — early signal](RE_019.md) | Low |
| [RE_020 · Excess draw calls with render thread saturated (not GPU-bound)](RE_020.md) | High |
| [RE_021 · Excess draw calls with render thread critical (not GPU-bound, 30 FPS)](RE_021.md) | High |
| [RE_022 · Extremely high draw calls — multiplatform scalability risk](RE_022.md) | High |
| [RE_023 · Extreme triangle count — scalability risk](RE_023.md) | High |
| [RE_024 · Early signal of fill-rate / post-process bound](RE_024.md) | Low |
| [RE_030 · High triangle count without GPU-bound — possible culling/visibility cost](RE_030.md) | Medium |
| [RE_039 · Early combined warning: draw calls + GPU time](RE_039.md) | Low |
| [RE_041 · High TSR cost](RE_041.md) | Medium |
| [RE_042 · High Sky Atmosphere cost](RE_042.md) | Medium |
| [RE_043 · High Volumetric Cloud cost](RE_043.md) | Medium |
| [RE_044 · High Niagara GPU cost](RE_044.md) | Medium |
| [RE_045 · Meshes missing LODs in the level](RE_045.md) | Medium |
| [RE_046 · Dense meshes with Nanite disabled](RE_046.md) | Medium |
| [RE_047 · Instancing candidates in the level](RE_047.md) | Low |
| [RE_048 · Few triangles per draw call](RE_048.md) | Medium |
| [RE_049 · Landscape without Nanite](RE_049.md) | Low |
| [RE_050 · LOD chain that doesn't reduce](RE_050.md) | Medium |

### CPU

| Rule | Severity |
|---|---|
| [RE_009 · Game thread saturated — CPU-bound (60 FPS)](RE_009.md) | Medium |
| [RE_010 · Critical game thread saturation — CPU-bound (30 FPS)](RE_010.md) | High |
| [RE_026 · Elevated game thread — early warning (60 FPS)](RE_026.md) | Low |
| [RE_027 · Frame dominated by game thread with render thread low](RE_027.md) | Medium |
| [RE_028 · Extreme CPU-bound — possible gameplay hitch](RE_028.md) | High |
| [RE_032 · Game thread near the limit with GPU idle](RE_032.md) | Medium |

### Memory

| Rule | Severity |
|---|---|
| [RE_007 · High texture memory usage](RE_007.md) | Medium |
| [RE_008 · Critical texture memory usage](RE_008.md) | High |
| [RE_016 · Elevated texture memory — early warning](RE_016.md) | Low |
| [RE_025 · High texture memory correlated with a slow GPU](RE_025.md) | High |
| [RE_031 · High texture memory without measured GPU impact](RE_031.md) | Low |
| [RE_051 · System RAM almost exhausted](RE_051.md) | High |
| [RE_052 · System memory near the limit (percentage)](RE_052.md) | Medium |
| [RE_053 · Sustained editor memory growth](RE_053.md) | High |

### Blueprint

| Rule | Severity |
|---|---|
| [RE_033 · Costly Blueprint tick (60 FPS)](RE_033.md) | Medium |
| [RE_034 · Critical Blueprint tick](RE_034.md) | High |
| [RE_035 · Many Blueprint classes ticking simultaneously](RE_035.md) | Medium |
| [RE_036 · High aggregate Blueprint tick cost](RE_036.md) | Medium |
| [RE_037 · Critical aggregate Blueprint tick cost](RE_037.md) | High |
| [RE_038 · Tick cost concentrated in few classes](RE_038.md) | Medium |

### General

| Rule | Severity |
|---|---|
| [RE_011 · Frame outside the 60 FPS budget](RE_011.md) | Low |
| [RE_012 · Frame outside the critical 30 FPS budget](RE_012.md) | High |
| [RE_014 · CPU and GPU saturated simultaneously](RE_014.md) | High |
| [RE_029 · Extreme frame time spike — possible hitch](RE_029.md) | High |
| [RE_040 · CPU and GPU both elevated without exceeding budget yet](RE_040.md) | Medium |
| [RE_054 · Recurring hitches during play](RE_054.md) | Medium |

## Writing your own

The same schema is open to you — see [Write your own rules](../guides/custom-rules.md).
