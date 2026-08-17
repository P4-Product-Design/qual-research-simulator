---
name: qual-research-simulator
description: Simulates qualitative user research sessions against a Figma design by running it through data-grounded personas that react independently, like real participants in a usability study. Use this whenever the user wants persona-based feedback on a Figma design, wants to "test" or "run" a design past simulated users, wants to understand qualitative reasons behind quantitative test results, or asks to have personas react to, critique, or answer questions about a design. Also trigger for phrases like "run this design through our personas," "what would our users think of this," "simulate a usability session," or "help me understand why this test performed the way it did." Always prompt for category (Sleep, Fitness, or Aging) and a description of the design's intended purpose/hypothesis if not specified, and always confirm whether to save the session before ending.
---

# Qualitative Research Simulator

Simulates independent, data-grounded persona reactions to a Figma design — like running a moderated usability study, but with personas built from real behavioral/demographic data rather than real recruited users. Supports both **exploratory sessions** (no prior data, just "how would these people react to this design") and **explanatory sessions** (quant test results provided). In explanatory sessions, personas react blind to the design first — the quant data is only introduced afterward, as a separate step comparing the completed qualitative findings against the numbers to see what could explain them.

**Every output must be clearly labeled SIMULATED / not real user research.** This tool produces plausible, data-grounded hypotheses to inform design thinking — it does not replace real user research, and should never be presented or saved in a way that could be mistaken for it.

**Personas never learn, evolve, or carry anything across sessions. This is a hard rule, not a default.** Every time this skill runs, each persona starts from exactly the same baseline defined in its reference file — the same behavior, motivations, trusts/distrusts, and demographic notes as every previous session, with zero accumulated memory of prior designs, prior reactions, prior feedback from the user, or prior sessions of any kind. A persona reacting to a design today must be reconstructable from the reference file alone, with no drift from having "seen" other designs before. This holds even across a long working relationship with the same user, even if the user references a past session, and even if it would make the persona's reactions feel more consistent or convenient to let something carry over. If the user provides new underlying behavioral/demographic data for a category, that updates the reference file itself (a deliberate, explicit edit) — it is never something a persona "picks up" mid-session or between sessions on its own.

## When to use this skill

Trigger this skill whenever the user:
- Shares a Figma link and wants persona/user feedback on it
- Wants to know how specific user segments would react to a design
- Has quant test results and wants help understanding the qualitative reasons behind them
- Asks to "run," "test," or "simulate" a design against personas
- Wants a synthesized research summary (themes, sentiment, recommendations) from a design review

## Step-by-step workflow

### 1. Gather required inputs

Always confirm you have, or ask for:
- **A Figma link.** If not provided, ask for it directly — don't proceed without it.
- **Category.** Always prompt explicitly: "Which category should I use — Sleep, Fitness, or Aging?" Do not try to infer this from the design itself.
- **A design description and hypothesis.** Ask the user to briefly describe the design and what it's intended to do (e.g. "this adds a trust badge to drive more clicks" or "this simplifies the pricing table to reduce drop-off"). This hypothesis is used ONLY in the synthesis step to assess whether the design appears to be working as intended — it must never be shown to or referenced by personas during the blind qualitative read (see Step 4).
- **A task to perform (optional).** Ask if the user wants personas to narrate performing a specific task, as if the Figma prototype were a live website (e.g. "click through to check the price," "find the phone number and call it," "add this to a comparison list"). If given, personas narrate their process of attempting that task in real time — where they'd look, what they'd click, where they'd hesitate or get lost — not just react to the design in the abstract. If no task is given, personas react to the design generally per the prompt set below.
- **Quant data (optional).** Ask if there's associated test/performance data to explain (explanatory mode), or if this is a fresh design review with no prior data (exploratory mode). If the user already provided a quant data source (a link, file, or pasted data) anywhere in their initial request, treat that as answered — do not ask for it again later in the session.
- **Prompts.** Ask whether to run the standard narration prompts, custom prompts, or a mix — and briefly explain the format, since it's a continuous think-aloud monologue per persona, not a Q&A transcript:
  > Personas narrate a stream-of-consciousness monologue as they take in the design — first impression, figuring out what it is, what builds or undermines trust, any hesitation, and what they'd actually do next — the way a real usability-test participant thinks out loud, rather than answering questions one at a time.

  See `references/interview-script.md` for the full prompt beats and how to adapt them (add to them, trim them, replace them entirely, or mix in custom prompts).

If the user's request already answers one or more of these (e.g. they paste a Figma link and say "run this through the Sleep personas asking X, Y, Z"), don't re-ask — just confirm your understanding briefly and proceed.

### 2. Load the right persona set

Based on the category given, read the matching file:
- Sleep → `references/personas-sleep.md`
- Fitness → `references/personas-fitness.md`
- Aging → `references/personas-aging.md`

Each file contains the full fixed persona set for that category (5-7 personas depending on category). Always run **all** personas in a category's set for a given session — the set is fixed, not picked per-session. (The user can ask to expand beyond this set — see Step 7.)

**Load fresh from the reference file every single time, with zero carryover from any prior session.** Do not adjust, soften, sharpen, or otherwise evolve a persona based on how it reacted last time, what the user said about a past session, or anything else outside the reference file itself. If the file hasn't been edited, the persona today must be functionally identical to the persona in every past and future session.

These files are a bundled snapshot of the live persona database and may drift out of sync over time. If the user says the personas have been updated, or it's been a while, suggest re-syncing this reference file from the source of truth before running a session.

### 3. View the Figma design

Use available Figma tools to view the design (load the relevant Figma skill/tool guidance before calling any Figma tool — check `/mnt/skills/plugins/figma:*` skills). Get a clear enough view of the actual design (not just a text description) that each persona's reactions can plausibly reference specific visual/content details.

### 4. Run each persona independently — blind to any quant data AND to the design hypothesis

For each persona in the category's set:
- React **only** based on that persona's behavior, motivations, trusts/distrusts, and demographic notes — do not let one persona's reaction leak into another's, and do not let design heuristics (see below) shape how a persona reacts. Personas react as themselves, not as UX experts.
- **Always generate a general first-look monologue first, with no artificial pull toward whatever element or change is actually being evaluated.** The persona has no idea what the researcher is interested in — their attention should go wherever their real behavioral profile would naturally take it, not toward the thing this session happens to be testing. If a persona's realistic scanning pattern wouldn't notice a given element (a small link, a subtle label change, anything), the monologue should simply not mention it — silence about something is valid, real signal, not a gap to fill in. Do not have every persona comment on the same feature just because it's the focus of the exercise; that's a form of contamination and destroys the value of running multiple distinct personas in the first place.
- **If the user provided a task (Step 1), run it as a separate, second monologue after the general one — never merged into it.** The general monologue always comes first, as if the persona is simply looking at the design with no instructions. Only after that's complete does the persona get "directed" to attempt the specific task, the way a moderator would give an instruction only after observing a participant's unprompted first impression. Label these as two distinct blocks per persona (e.g. "General reaction" and "Task: [task description]").
- **Even if quant data or a stated design hypothesis was provided in Step 1, do not reference either here.** Personas react to the design exactly as if no test had ever run and no one had told them what the design was "supposed" to do — no persona should be told "here's what we observed" or "this is meant to increase X." Both the qualitative read and the hypothesis check must be generated independently, so they can be compared afterward without contamination.
- The Reluctant End User persona in Aging is a partial exception: it should react adversarially/critically to the design and copy rather than acting as a rational decision-maker evaluating features.

Produce this as a **transcript**: persona name, then their monologue(s), clearly separated per persona and, when a task is present, clearly separated into "General reaction" and "Task" blocks. Naming personas here is fine — each monologue is already labeled, so there's no ambiguity about who's speaking.

### 5. Synthesize (qualitative-only)

After all personas have responded, write a synthesis with these distinct sections. **In this step, write thematically, not persona-by-persona.** The person reading this synthesis has not memorized the persona roster and shouldn't need to — don't lean on proper-noun persona names as if they're familiar characters. When a specific persona's reaction is worth citing, briefly describe what they represent inline (e.g. "the persona researching this on behalf of an aging parent" rather than just "The Protective Caregiver") rather than assuming the name alone carries meaning.

- **Cross-persona themes** — where multiple personas converged (positively or negatively), and where they diverged. Ground every theme in specific reactions, described thematically (e.g. "reactions split along whether someone was buying for themselves or for someone else" rather than a roll call of names).
- **Sentiment summary** — overall read across the persona set (e.g. "most reactions were positive toward X, but Y was a near-universal friction point"), described in terms of behavioral/motivational groupings rather than persona names.
- **Hypothesis check** — using the design description/hypothesis gathered in Step 1 (never shown to personas), assess whether the blind qualitative findings suggest the design is achieving its intended effect. State plainly where the findings support the hypothesis, where they cut against it, and where the evidence is genuinely mixed or inconclusive.
- **Heuristic notes** (separate section) — pull in relevant principles from `references/heuristics.md` (usability heuristics and/or consumer cognitive heuristics) ONLY where they add real explanatory value to what personas actually said. Flag explicitly if a heuristic and a persona's reaction seem to point in different directions — don't quietly resolve the tension.
- **Recommendations / next steps** — concrete, specific suggestions for the design, and where relevant, what to test or validate next.

This entire step is generated without reference to any quant data, even if some was provided — it should read exactly the same whether or not a quant result exists. The hypothesis check is the one exception that draws on Step 1 information, and it does so only after the blind themes/sentiment are already established.

### 6. If quant data was provided: explain the data (separate, after-the-fact step)

Only now — after the blind qualitative transcript and synthesis are complete — bring in the quant data and compare it against what came out of the qualitative read. This is a distinct analytical step, not something the personas participated in.

Produce a clearly separated section, e.g. **"Applying the Qualitative Read to the Quant Results"**:
- State the quant result plainly (e.g. "Variant B saw a 15% drop in CTA clicks").
- Go back through the qualitative themes/sentiment already generated and identify which ones, if true of real users, would plausibly produce that quant pattern.
- Present 2-3 ranked candidate explanations, each explicitly tied to a specific theme from Step 5 — never invent a new explanation here that didn't already surface in the blind qualitative read. Keep this thematic rather than persona-by-persona, consistent with Step 5 — if citing a specific persona's reaction adds real clarity, briefly describe what they represent rather than relying on the name alone.
- Frame these explicitly as hypotheses a real follow-up study could test, not as confirmed conclusions. If the qualitative themes don't actually offer a plausible explanation for the quant result, say so directly rather than forcing a fit.

### 7. Offer to expand the session

After Step 6 (or Step 5, if there was no quant data to explain), ask the user if they'd like to expand the session, using short, plain-language explanations — write these as if for someone who has no idea what either option is, don't assume familiarity with the terms:

> Want to go further with this? Two options:
> 1. **More variety within one persona** — see a few versions of the same persona reacting slightly differently (e.g. someone more price-sensitive vs. less), so you can check if the design holds up across that range.
> 2. **A combined persona** — a new, more specific persona made by blending two of the existing ones, for a situation where someone might realistically feel two things at once (e.g. in a hurry AND wanting reassurance before buying).

If the user picks one (or both), read `references/expansion-methods.md` and apply the relevant method:
- **Intensity variants** (statistical-feeling breadth): 2-3 labeled variants along one data-grounded axis of a single persona, reported under a "Simulated Distribution" heading with the mandatory caveat that this is not a real sample.
- **Blended personas** (qualitative depth): a deliberately authored combination of two personas that creates a genuine behavioral tension, reported as a separate "Qualitative Deep Dive," never mixed into the base set's transcript or any tallied counts.

These two methods serve different purposes and must stay visibly separate in the output — never merge variant tallies and blended-persona reactions into one undifferentiated list, and never let either read as equivalent to an independent human response. If the user declines, skip straight to Step 8.

### 8. Offer to save

At the end of every session, ask whether the user wants to save the results (and where — e.g. as a new Notion page), or keep it in-chat only. Do not auto-save.

## Notes on tone and rigor

- Personas should sound like real people thinking out loud, not like marketing copy or a survey summary — use their sample voice as a calibration anchor, and let the monologue wander and breathe the way a real think-aloud session does rather than reading as a polished, structured answer.
- Don't let every persona like everything. If the data suggests a persona would be skeptical, distrustful, or turned off by something, let that come through clearly, even if it's not what the user wants to hear.
- Be specific about what in the design triggered a reaction (a specific headline, image, button, layout choice) rather than generic feedback that could apply to any page. In task mode, be specific about the actual interaction attempted (what was clicked, where they looked, what they expected to happen).
- If a design genuinely doesn't give a persona enough to react to (e.g., missing pricing for a price-sensitive persona, or a task that isn't achievable in the current prototype), say so explicitly rather than inventing a reaction.
- No persona's baseline ever changes between sessions. See the statelessness note near the top of this file and in Step 2 — this applies regardless of how many times this skill has been run before, in this conversation or any other.
- **Do not assume every persona notices the specific thing being tested.** This is one of the easiest ways to quietly ruin a session: knowing what element or change is under evaluation and then having every persona happen to comment on it produces fake consensus and destroys the point of running distinct personas at all. Some personas realistically won't notice a subtle change, a small link, or a minor copy difference — let that be true when it's true. A finding like "half the personas never registered this at all in a natural first look" is often more valuable than uniform commentary would have been.
