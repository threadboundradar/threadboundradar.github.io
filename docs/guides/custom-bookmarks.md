# Mark your own moments in a trace

The plugin already writes a bookmark into your Unreal Insights recording every time a rule fires for the
first time. These three Blueprint nodes let you mark **your own** moments, so a trace tells your story
and not only the Rule Engine's.

All three live under the **Threadbound Radar | Trace** category in the Blueprint node menu.

## Before anything: record a trace

The nodes only do something while a trace is recording. Start one with the console:

```
TBR.Trace.Start
```

…reproduce whatever you want to capture, then:

```
TBR.Trace.Stop
```

If no trace is recording, every node below is a no-op. That is by design: you can leave the nodes in
your Blueprints permanently without paying for them.

## Add Trace Bookmark (Log)

Marks the current moment with a message of your own.

| | |
|---|---|
| **Input** | `Message` (string) |
| **Shows up in** | The **Log** panel of Unreal Insights — filter by the category `Bookmark` |
| **Costs** | Nothing when no trace is recording |

Use it for the moments only your game knows about: the boss spawning, the inventory opening, the player
entering the area that always stutters.

```
Event BeginPlay → Add Trace Bookmark (Log)   Message = "Wave 3 spawn"
```

In Insights, each Log result is clickable and jumps to that instant in the timeline. So this node is how
you answer "what was the game doing at that spike?" without counting frames by hand.

!!! success "Works in packaged builds"
    These nodes live in the plugin's Runtime module, so a Blueprint that uses them still cooks and runs
    in a packaged build. In Shipping they compile to nothing, because Unreal's tracing is disabled there.

## Begin / End Named Trace Scope

Opens and closes a named CPU scope, which appears as a **real timer** in Insights and in the plugin's
Trace Search panel, alongside the engine's own timers.

| | |
|---|---|
| **Input** | `Name` (string) on Begin; End takes nothing |
| **Shows up in** | Insights timers, and Trace Search's top contributors |
| **Scope** | Editor only (Play-in-Editor) |

This is how you measure a specific group of nodes. The plugin's Blueprint profiler times whole entry
points such as `Event Tick`; if you need to know what one branch inside that Tick costs, bracket it:

```
Begin Named Trace Scope   Name = "Pathfinding"
  … the nodes you want to measure …
End Named Trace Scope
```

!!! danger "They must be paired, on the same thread, properly nested"
    Think of them as parentheses. Every Begin needs exactly one End, and inner scopes must close before
    outer ones.

    **The usual way to break this is an early exit.** A Return node, or a branch that skips the End,
    leaves the scope stack unbalanced and the rest of your trace becomes unreliable. If you have several
    exit paths, make sure all of them pass through the End node.

    An End without a matching Begin is safe: it logs a warning and does nothing, rather than corrupting
    the stack.

## Reading it back

Once the recording is stopped, open the `.utrace` in the **Trace Search** panel. Your bookmarks sit
alongside the rules that fired, so you can see the diagnosis and your own markers on the same timeline —
and a button opens the same file in Unreal Insights when you want the engine's full view.
