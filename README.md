# HADR Monitor

A monitoring agent for humanitarian assistance and disaster response (HADR).

## What HADR means

**HADR** — Humanitarian Assistance and Disaster Response (often written "…and
Disaster Relief") — is the work of getting help to people after a sudden crisis:
earthquakes, cyclones, floods, volcanic eruptions, wildfires, tsunamis. It spans
the whole arc of a disaster — the quiet watching *before*, the scramble to
understand and respond *during*, and the long recovery *after*.

The people doing this work — UN agencies, national disaster offices, the Red
Cross and Red Crescent, NGOs, militaries running relief logistics — share one
hard problem before they can do anything else: **situational awareness**. In the
first hours after an event, the questions are always the same.

- **What** happened, and what kind of hazard is it?
- **Where**, exactly — and is that near people or empty ocean?
- **How bad** — magnitude, depth, wind speed, water level?
- **Who** is affected — how many people, in which country, how vulnerable?
- **Is this new**, or the same event you already heard about from another source?

Answering those questions fast, and repeatedly as the picture changes, is what a
monitoring agent is for. It is not the whole of HADR — it does not move supplies
or coordinate responders — but it is the sensing layer everything downstream
depends on. A responder who wakes up already knowing what happened overnight is
hours ahead of one who has to go and find out.

### Why this is genuinely hard

The difficulty is not fetching data — the feeds are public and documented below.
The difficulty is judgement, and it is the same judgement a human analyst applies:

- **Noise vs. signal.** Hundreds of small earthquakes happen every day. Almost
  none of them matter. The agent's core job is deciding what *doesn't* deserve a
  report.
- **The same event, many faces.** One earthquake can arrive from GDACS, USGS and
  ReliefWeb under three different identifiers, at three different times, with
  three different magnitudes. Are they one event or three?
- **Facts that change underneath you.** Magnitudes get revised, alert levels get
  upgraded, events occasionally get retracted. What do you do about a report you
  already published when the world it described has moved on?
- **Silence as a feature.** A good monitor stays quiet when nothing has changed.
  Crying wolf every morning trains its readers to ignore it.

These are not incidental — they are the assignment. See the `## Open questions`
section in each file under `feeds/`; they are where the real design lives.

## The feeds

Three live sources, each with a different character, documented in `feeds/`:

- **GDACS** (`feeds/gdacs.md`) — EU/UN multi-hazard alert system. Colour-coded
  (green/orange/red) alert levels across earthquakes, cyclones, floods,
  volcanoes, drought and wildfire. Fast, automated, broad.
- **USGS** (`feeds/usgs.md`) — US Geological Survey real-time earthquake feed.
  Earthquakes only, regenerated every minute, extremely precise.
- **ReliefWeb** (`feeds/reliefweb.md`) — UN OCHA's humanitarian service. Curated
  and slower: an event appears here once *humans* decide it matters. The richest
  context, but hours or days behind the machines.

The interesting design pressure lives in the gaps between them: a machine feed
that is fast but noisy, and a human feed that is trustworthy but late.

## What you could build with this

This repository asks for one specific thing — a scheduled morning situation
report — but the domain opens onto a much larger space. Once an agent can watch
feeds, dedupe events and assess severity, it can also:

- **Alert, not just report** — page a human immediately when a red-level event
  lands near a population centre, instead of waiting for 08:30.
- **Enrich** — cross-reference an event against population density, prior
  disasters in the same region, or infrastructure exposure.
- **Prioritise** — rank the overnight events so the reader sees the one that
  matters first, not the one that arrived first.
- **Explain its reasoning** — show *why* something was flagged or filtered, so a
  human can trust it (or correct it).
- **Learn its audience** — a report for a logistics team looks nothing like one
  for a public dashboard. Same events, different framing.
- **Stay honest under failure** — say something useful on a morning a feed is
  down, rather than silently reporting less than the truth.

The point of the exercise is less the specific dashboard and more the pattern:
an autonomous agent that senses a messy real-world stream, exercises judgement,
and produces something a human can act on and trust.

## The end state

By Wednesday afternoon this repository contains an agent that:

- watches live disaster feeds — GDACS, USGS and ReliefWeb (see `feeds/`)
- filters out the noise and assesses what remains: what happened, where, how bad, who is affected
- publishes a morning situation report to `dashboard.html` at 08:30 Singapore time
- runs on a schedule, unattended, and stays quiet when nothing has changed

How it does any of that is not specified anywhere in this repository. That is the course.

## The three days

1. **Plan** — interrogate the feeds, write the PRD, cut it into vertical slices
2. **Autonomy** — build the first slice, write a skill, wire up the 08:30 routine, launch the overnight loop
3. **Trust** — review code you didn't write, harden the pipeline, demo

## Artefacts expected by the end

`prd.html` · `system-view.html` · `implementation-notes.md` · `dashboard.html` · `goal.md` · at least one skill

## Day 1 setup

1. Sign in to Claude Code with your Team seat
2. Create your own repository from this template, then clone it
3. Run `/install-github-app` so @claude reviews your pull requests from Day 2
4. Install OpenCode and sign in with your Go key

Fill in `CLAUDE.md` before your first prompt.
