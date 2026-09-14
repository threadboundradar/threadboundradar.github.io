# Threadbound Radar

Real-time performance diagnostics for Unreal Engine. It does not just measure your frame — it tells you
**what is slow, why, and what to do about it**, then shows whether your fix actually worked.

## The loop

Everything in the plugin serves one cycle:

| Step | What happens |
|---|---|
| **Detect** | Live metrics for frame, game thread, render thread and GPU, with a CPU-vs-GPU chart |
| **Explain** | A rule fires with its probable cause and concrete solutions, not just a red number |
| **Fix** | You apply the change in your project |
| **Verify** | The next Play session is compared against the previous one, metric by metric |

That last step is the point. Finding a problem is easy; knowing whether your change helped is not.

## What makes it different

**It knows what the editor is doing.** Metrics always measure, but rules are gated by *regime*:
editor idle, compiling shaders, loading, or playing. A 400 ms spike while a map loads is not a
performance drop, and the plugin will not report it as one.

**It bridges to Unreal Insights.** When a rule fires during a recording, the plugin writes a bookmark
into your `.utrace`. Later you can open that file in the Trace Search panel and see which rules fired,
in which frame, ranked by severity — including patterns that repeat across frames.

**The rules are data, not code.** All rules live in a JSON file you can read, and you can add your own
without recompiling. See [Write your own rules](guides/custom-rules.md).

**It works offline.** Deterministic C++ rules, no server, no API keys, nothing leaves your machine.

## Where to start

- [Getting started](quickstart.md) — open the panel and read your first diagnosis
- [Write your own rules](guides/custom-rules.md) — add rules for your project's own budgets
- [Mark your own moments in a trace](guides/custom-bookmarks.md) — the three Blueprint nodes
- [Rules reference](rules/index.md) — what each built-in rule means
