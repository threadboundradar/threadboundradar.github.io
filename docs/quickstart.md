# Getting started

## 1. Enable the plugin

Install it from your Fab library, enable it in **Edit → Plugins**, and restart the editor when asked.

## 2. Open the panel

From the **Window** menu, open the **Performance Analyzer** group. It gives you three panels, and each
one answers a different question.

### Live Metrics — what is happening right now

Frame, game thread, render thread and GPU timing while you play, the CPU-versus-GPU chart, the verdict in
plain words, and the list of diagnostics that fired. This is the panel you keep open while you work.

!!! tip "Dock it down one side and leave it there"
    The layout adapts when the panel is narrow, so it works as a tall column beside the viewport instead
    of a wide strip that covers your level. That matters more than it sounds: the point is to *notice* a
    problem while you are building, not to go looking for one afterwards.

### Gameplay Profiler — which of my content costs the most

Cost per instance of the things you authored: Blueprint entry points and Niagara or Cascade systems. It
keeps a "worst seen this session" list, so a spike that lasted one second is still there when you go
looking for it a minute later.

Use it when Live Metrics says the game thread is the problem and you need to know *which* of your
Blueprints is responsible.

### Trace Search — what happened inside a recording

Opens a `.utrace` file and shows which rules fired, in which frame, ranked by severity, together with the
combinations that repeat. It is the panel for looking backwards at a problem you already captured, and
[step 7](#7-go-deeper-record-a-trace) walks through it.

## 3. Pick your target frame rate

The 30 / 60 FPS selector changes everything downstream: the colour thresholds of every metric and which
rules are allowed to fire. A rule about a 60 FPS budget will not bother you if your game targets 30.

## 4. Press Play and read the panel, top to bottom

**The regime band** tells you what the editor is doing right now — idle, compiling shaders, loading, or
playing. Rules are filtered by it, so you do not get frame-budget alerts while a map is still loading.

**The live chart** draws CPU against GPU over the last seconds, so you see not just *which* one is the
bottleneck but by how much.

**The verdict** reads those two lines in plain words. It uses a short hysteresis, so it will not flicker
between states on a single noisy frame.

**The diagnostics table** lists the rules that fired this session, with their id, severity and how many
times each one fired. The most frequent problem matters more than the most recent one.

Click a row to read its **probable cause** and its **solutions**.

## 5. Stop, change something, play again

When you stop, the session is saved and compared with the previous one, metric by metric: green if it
improved, red if it got worse. That comparison is the honest answer to "did my optimization help?".

## 6. Export a report

One click writes a self-contained HTML report — metrics, active diagnostics, cost breakdowns and the
session comparison — and opens it in your browser. Each export is kept separately, so you can compare
before and after a round of optimization.

## 7. Go deeper: record a trace

The panel tells you **what** is slow and **why** it probably is. A trace tells you **where** — the exact
frame, and the exact call that spent the time. Recording one is not an advanced extra: it is the normal
next step whenever the panel points at something you cannot fix from a number alone.

### Record it from the panel

The **Live Metrics** toolbar has a **Start Trace** button, which turns into **Stop Trace** while a
recording is running. Press it, reproduce the problem, press it again.

Next to it, **Channels** decides what gets recorded. Leave it empty for the engine's default set;
`bookmark`, `frame` and `gpu` are always added on top of whatever you type, because they are what the
plugin needs to place and find its own marks.

Two things worth knowing. It uses the same mechanism as the engine's own Session Frontend, so it is a
normal Unreal Insights recording, nothing proprietary. And **starting a trace clears the Diagnostics
totals**, so the counts you read afterwards belong to the recording instead of to everything that
happened since you opened the editor.

If you prefer the console, `IPA.Trace.Start` and `IPA.Trace.Stop` do exactly the same thing.

### Read it back in Trace Search

While the trace is recording, every rule that fires for the first time writes a bookmark into it. Open
that `.utrace` in the **Trace Search** panel and the frames where rules fired come grouped and ranked by
severity, so the first row is the worst frame and not merely the earliest one. Repeated combinations are
detected too, which is how you tell a background problem from a one-off spike.

### Three moments when you want a trace

**You saw a spike in the chart.** An average hides it: fifty-nine frames at 16 ms and one at 412 average
out to something that reads as "a bit slow". The trace has the frame itself.

**The Gameplay Profiler flagged a Blueprint.** Now you know the class but not which part of it. Bracket
the suspicious branch with [Begin / End Named Trace Scope](guides/custom-bookmarks.md), record again, and
your scope shows up as a real timer beside the ones the engine writes.

**You want to start from the trace.** If you already have a recording made while the plugin was active,
you do not need to reproduce anything: open it in Trace Search and work backwards from what fired.

Either way, a button opens the same file in Unreal Insights when you want the full view. The plugin tells
you what to look for; Insights takes you there.
