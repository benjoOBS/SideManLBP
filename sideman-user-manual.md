# Sideman — User Manual

Sideman listens through a mic or a loaded recording and acts like a real-time mixing assistant. Before anything else, know what it actually is: **a rule-based tool reading real frequency data, not a trained "AI ear."** Every recommendation comes from measuring your captures against each other, against a target curve you picked, or against a room profile you described — never from a model guessing what "good" sounds like.

---

## Getting started

**Open it as its own page, not inside a chat preview.** If you're seeing "mic access denied" or a blocked camera, that's almost always a sandboxed preview blocking those APIs outright, not a real permission problem. Host the file (GitHub Pages works well) or open it locally — outside a sandbox, your browser's normal permission prompts take over.

**Three input sources**, switchable at the top of the Analyzer tab:
- **Microphone** — live listening. A device dropdown appears once you've granted permission once, letting you pick a specific interface or virtual audio cable instead of the default mic.
- **Recorded mix** — load an audio file and play it through the same analysis pipeline.
- **System audio** — captures whatever's playing on your computer (e.g. monitoring OBS's output), via screen/tab sharing.
  - *Limitation:* Chrome or Edge on a desktop only. Not supported in Firefox, Safari, or on mobile. You must tick "Share audio" in the picker — it's easy to miss and fails silently if you don't.

The mic is never routed to your speakers (that would cause feedback). Recorded Mix is; System Audio isn't (it's already playing independently of Sideman).

---

## Analyzer tab

The real-time view — spectrum curve, level meter, and a 7-band numeric readout (Sub, Bass, Low-Mid, Mid, High-Mid, Presence, Air).

**Mic compensation sliders** nudge what you *see* to account for a phone mic's known bass roll-off and treble limitations — a "low-end lift" for Sub/Bass, an "air roll-off fix" for Presence/Air. Only shown, and only applied, when **Microphone** is the active source — it hides entirely on Recorded Mix or System Audio, since a phone-mic correction makes no sense on audio that never passed through a phone mic.
> *Limitation:* Display-only, deliberately. Captures and recommendations always use the raw, uncompensated reading, on every source — these sliders can never skew what counts as "too hot" or "too light." Their only job is making the live curve on this one tab feel closer to what your ears are hearing while you eyeball it.

**Noise floor** — capture a few seconds of silence (nobody playing) as a baseline. Every captured instrument afterward shows its headroom above that baseline, and thin margins (under 10 dB) get flagged.
> *Limitation:* Tells you the margin is tight, not *why*. Could be gain, mic placement, or genuine room noise — it doesn't diagnose the cause on its own (pairing it with the gain-position field on an instrument sharpens this, see Capture tab below).

**Audition an EQ move** — a real parametric EQ band and compressor sitting on Recorded Mix playback, so you can actually *hear* whether a suggested cut helps before touching your real gear.
> *Limitation:* Recorded Mix source only (mic is never sent to speakers at all; system audio plays independently of Sideman's graph, so processing it here wouldn't be audible). Nothing here is saved — it resets every time you reload the page.

---

## Capture tab

**Guided session** walks through your instrument list one at a time — "Play Kick only," a countdown, then a ~3.5s capture — and shows a narrative immediately after each one (see below).

**Instruments list** — each row has:
- A **category** (Drums, Bass, Strings/Plucked, Keys/Pads, Vocals, Other), auto-guessed from the name, overridable. Only changes how findings are *worded*, never what's compared.
- **Lead / Support** tags — when two conflicting instruments have both set, the conflict recommendation names which one to carve space for. Auto-suggested (never auto-decided) from the instrument's name — "lead," "solo" → Lead; "bgv," "background," "harmony" → Support — only for Strings/Keys/Vocals, always overridable. Hidden entirely for anything tagged Drums/Percussion, since it's rarely the right question for an individual kit piece.
- **Mic type / DI** tag — dynamic, condenser, ribbon, or DI. Adds an honest caveat to harshness findings ("part of this could be the mic's own character") and flags when a reading is bleed-free (DI).
- **Gain position** (7 o'clock through 5 o'clock) — a rough dial position, not a dB number.
  > *Limitation:* Deliberately not a raw dB field. A dB number is meaningless without knowing that specific preamp's range; dial position is honestly interpretable regardless of gear. Feeds the noise-floor recommendation's wording only.
- **Export** — downloads that single instrument's captured fingerprint as a small JSON file, shareable with anyone else running Sideman.

**Post-capture narrative** — right after any single-instrument capture (guided or standalone), shows what was captured, the harshness/thin/boxy findings for just that instrument, and either a comparison to a **per-instrument reference capture** (if you've made one) or a softer, hedged note about whether its peak range fits your selected style target.

**Per-instrument reference capture** — load an isolated reference for one specific instrument via Recorded Mix, tap "Capture reference," and from then on that instrument gets a real measured comparison instead of a soft hint.
> *Limitation:* Needs a genuinely isolated source (a solo stem, not a full mix) to mean anything — comparing against a reference that has other instruments bleeding in will produce misleading results.

---

## Venue tab

**Room questionnaire** — four plain-language questions (space type, surfaces, ceiling height, occupancy) plus **"What are you mixing for?"** (Studio/recording, Live performance, Livestream, or Live + Livestream). Feeds a qualitative "room profile" note in Mix Check, plus a mode-specific heads-up: Live in a fairly dead room or Studio in a fairly live one flags that what you're hearing may just be the room; Livestream notes that stream listeners hear the room through a phone speaker or earbuds, where ambience that reads as "live" in person often just reads as muddy — so lean controlled regardless of the room questions above; Live + Livestream flags the real tension between the two and suggests a separate, drier feed for the stream where possible.
> *Limitation:* This is a rough qualitative score from your answers, not a measurement. Sideman has no way to detect actual reverb/decay time — that would require tracking level over time after a transient, which none of its capture methods currently do.

**Room dimensions** (optional) — computes real axial room-mode resonances from the physics (speed of sound ÷ 2× each dimension).
> *Limitation:* Assumes an idealized, empty, rectangular room with hard parallel walls. Real rooms with furniture, doors, and non-parallel walls behave differently — treat it as a starting point.

**Stage photo + pins** — snap a still photo, tap to drop labeled pins for PA/speaker placement reference (FOH/Mix, Main L, Main R, Subwoofer, Monitor 1, Monitor 2, Delay Speaker, Stage Front, or a custom label).
> *Limitation:* Purely visual reference. No positions are measured; it's a labeled photo, not a to-scale plan.

**Distances** — shows the currently active unit (Meters/Feet, set by the toggle at the top of this tab) right in the panel. Type in a measurement if you know it, or use **"Walk it off"**: tap once per step (× your stride length) or time yourself walking (× an assumed pace). The From/To fields offer whatever labels you've already placed as stage-photo pins, so you're picking a real spot instead of typing blind.
> *Limitation:* "Walk it off" is explicitly a guesstimate, not a measurement — labeled that way in the result itself. The pin suggestions only appear once you've placed pins on a stage photo first.

A **Meters/Feet toggle** at the top of this tab controls units for both dimensions and the distance tools.

---

## Mix Check tab

**Your EQ** — tell Sideman what hardware you're actually working with (parametric, graphic with real band centers, or a 2-/3-band tone stack), and every recommendation that names a frequency range also names the actual control ("Try your 8k Hz graphic band" instead of just "cut around 4–16kHz").
> *Limitation:* A graphic EQ's nearest band is sometimes genuinely far from the target frequency — Sideman says so rather than pretending a distant band will fully reach it.

**Capture full mix** — with everyone playing, captures the whole mix for comparison against your selected target and your individually-captured instruments.

**Recommendations**, in priority order: frequency conflicts between instruments → thin noise-floor headroom → per-instrument harshness/thinness/boxiness → overall tonal balance vs. target → room resonance → room/venue profile. Each card has a **Dismiss** button — dismissing is tied to the specific capture that produced it, so recapturing that instrument makes the finding reappear fresh if it's still there. Once everything's resolved or dismissed, you'll see "You're all set."

---

## Style tab

**Five built-in target-curve archetypes** (Live Worship, Modern Pop, Classic Rock, EDM/Club, Vintage Warm) — tonal-balance shapes, not verified recreations of any real engineer's actual settings.

**Reference track target** — play a well-mixed reference song via Recorded Mix, capture it, and it becomes a selectable target alongside the presets — a real measured curve instead of an estimate.

---

## Log tab

**Saved setups** — snapshot your entire working state (instruments, venue, style, reference targets, EQ profile) under a name. **Load** restores it; **Compare** diffs whatever you've *already* recaptured live against the saved version, band by band, matched by instrument name.
> *Limitation:* Compare only catches instruments you've recaptured live under the *same name* as the saved version — rename an instrument between sessions and it won't match up.

**Export/Import** — whole setups as JSON files, same mechanism as single-instrument export.

**Session log** — a running history of each full-mix analysis (timestamp, source, style, number of findings).

**Reset saved band profile** — clears everything: instruments, venue data, reference targets, noise floor, dismissed findings, EQ profile. Cannot be undone.

---

## Data & storage

Everything lives in your browser's local storage on whatever device you're using — nothing is sent anywhere. That also means:
- It's **per-browser, per-device**. Switch phones or browsers and you're starting fresh unless you export/import.
- **Storage has a real ceiling** (typically a few MB). Many saved setups with stage photos attached can approach it — if a save fails, Sideman tells you plainly rather than pretending it worked.
- Clearing your browser's site data wipes everything Sideman has stored.

---

## The honest summary

Sideman is good at: measuring real frequency content, catching genuine conflicts between captured parts, giving you an honest second opinion, and turning a target into something actionable on your actual gear. It is not: a trained ear, a substitute for monitoring on good speakers, a way to measure real-world distances or reverb time, or aware of anything about your mix that isn't reflected in the numbers it captured. When it hedges ("worth checking," "may sound"), that's deliberate — treat those as a nudge to go listen, not a verdict.
