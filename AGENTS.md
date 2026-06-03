# Content Engine — Agent Instructions
# Claude Code reads this file to run each agent.
# Outputs are written to the outputs/ folder.

---

## AGENT 1: Research Agent

Read config.py and extract NICHE, PLATFORM, and CHANNEL_STYLE.

Your job is to find the 5 best video topic ideas for a {CHANNEL_STYLE} 
{NICHE} creator on {PLATFORM} right now.

Think about:
- What topics are trending this week in {NICHE}
- What questions beginners are asking about {NICHE}
- What controversies or new releases are happening in {NICHE}
- What pain points {NICHE} creators always get wrong

For each topic output:
1. Title — punchy and specific
2. Why trending — one sentence
3. Target audience — exactly who watches this
4. Competition — LOW / MEDIUM / HIGH
5. Viral score — 1 to 10

Write the output to outputs/topics.md in this exact format:

---
# Topic Ideas
Generated for: {NICHE} | {PLATFORM} | {CHANNEL_STYLE}

## 1. [Topic Title]
**Why trending:** one sentence
**Audience:** specific audience
**Competition:** LOW/MEDIUM/HIGH
**Viral score:** 8/10

## 2. [Topic Title]
...and so on for all 5 topics
---

Do not write anything else. Only write the formatted output to outputs/topics.md.

---

## AGENT 2: Hook Factory

Read outputs/topics.md.
The user has selected topic number: {SELECTED_TOPIC}

Generate 8 different hooks for that topic.
Each hook must use a different formula from this list:
1. Curiosity gap — "Nobody is talking about this..."
2. Bold claim — "This changed everything for me..."
3. Controversy — "Everyone is wrong about..."
4. Personal story — "I lost X until I discovered..."
5. Statistic shock — "93% of people don't know..."
6. Direct challenge — "You're doing X wrong. Here's proof."
7. Fear — "Stop doing X before it's too late..."
8. Future vision — "In 6 months, X will be dead. Here's what's next."

For each hook output:
- The hook text (max 2 sentences, punchy)
- Formula used
- Why this works for the selected topic (1 sentence)
- Predicted scroll-stop score: 1 to 10

Write output to outputs/hooks.md in this exact format:

---
# Hooks for: [Topic Title]

## Hook 1 — Curiosity Gap
**Hook:** "Nobody is talking about..."
**Why it works:** one sentence
**Scroll-stop score:** 8/10

## Hook 2 — Bold Claim
...and so on for all 8 hooks
---

Do not write anything else. Only write the formatted output to outputs/hooks.md.

---

## AGENT 3: Script Engine

Read outputs/hooks.md.
The user has selected hook number: {SELECTED_HOOK}

Write a complete video script using that hook as the opening.

Script structure:
Target length: 30-40 seconds total (short form first)

- HOOK (0:00 - 0:05): the selected hook, word for word
- OPEN LOOP (0:05 - 0:10): tease the payoff, don't reveal yet
- VALUE (0:10 - 0:25): the core insight in 3 punchy sentences max
- RE-HOOK (0:25 - 0:30): one line that makes them want the next video
- CTA (0:30 - 0:38): one clear ask — comment, follow, or save

Keep every line under 10 words. Write for spoken delivery, not reading.
No filler. No "in this video". Every second must earn its place.

For each section mark:
- [TALKING HEAD] — person speaks to camera
- [B-ROLL] — what footage to show
- [TEXT OVERLAY] — what text appears on screen
- Timestamp marker — (0:00), (0:05) etc.

Write output to outputs/script.md

---

## AGENT 4: TRIBE v2 Script Scorer

Read outputs/script.md

Analyze the script and predict TRIBE v2 neural activation scores.
Score these 3 parameters from 0 to 100:

1. HOOK STRENGTH
   - Does the opening trigger curiosity or fear circuits?
   - Is it specific enough to feel real?
   - Does it promise a clear payoff?

2. RETENTION SCORE  
   - Are there re-hooks placed correctly?
   - Does the pacing keep information density high?
   - Are open loops resolved at the right time?

3. VIRAL PROBABILITY
   - Does it trigger an emotion strong enough to share?
   - Is there a surprising or counterintuitive moment?
   - Does the CTA create urgency?

Then go through the script section by section and give specific improvement suggestions.

For each weak section (score below 70) write:
- What seconds it covers
- What the problem is
- Exactly how to rewrite it

Write output to outputs/scores.json in this exact format:

{
  "hook_strength": 84,
  "retention_score": 71,
  "viral_probability": 79,
  "overall": 78,
  "verdict": "Strong — publish with minor fixes",
  "improvements": [
    {
      "section": "0:15 - 0:45",
      "problem": "Context section loses momentum",
      "suggestion": "exact rewrite suggestion here"
    }
  ]
}

---

## AGENT 5: Metadata Agent

Read outputs/script.md and outputs/scores.json

Generate everything needed to publish the video:

1. Three title variations (A/B testable)
2. Full description (with timestamps, keywords, CTA)
3. 15 tags
4. Thumbnail concept (expression, text overlay, background color, layout)
5. Best time to post based on PLATFORM from config.py

Write output to outputs/metadata.md

---

## AGENT 6: Repurpose Agent

Read outputs/script.md

Cut the script into 3 short-form versions for:
1. Instagram Reels (max 30 sec) — fast, punchy, hook in first 2 sec
2. TikTok (max 60 sec) — conversational, trending audio suggestion
3. YouTube Shorts (max 60 sec) — value-first, subscribe hook at end

For each platform:
- Rewrite the hook for that platform's style
- Cut the script to fit the time limit
- Add platform-specific CTA
- Suggest caption and hashtags

Write output to outputs/repurpose.md


## Agent 4B — Suggestion Agent

**Trigger:** User runs "suggestions" agent from dashboard
**Input files:** `outputs/scores.json`, `outputs/script.md`
**Output file:** `outputs/suggestions.md`

**Instructions:**

Read `outputs/scores.json` and find all seconds where the score is below 60.
Read `outputs/script.md` to understand the full script content.

For each weak second (low neural activation), figure out which part of the script corresponds to that timestamp (assume ~2-3 words per second from the start of the script).

Then write specific, creative, brutally honest suggestions for each weak moment.

Format: 
Second X — [what's happening in script here]
Problem: [what's weak about this moment]
Fix: [exact edit — specific words, cut, zoom, B-roll suggestion]

Be direct. Name exact words. Suggest exact edits. No fluff.
Write minimum 5 suggestions, maximum 10.
Save output to `outputs/suggestions.md`.



## Agent 8 — Autopilot

**Trigger:** User runs "autopilot" agent from dashboard
**Output files:** outputs/topics.md, outputs/hooks.md, outputs/script.md, outputs/suggestions.md

**Instructions:**

You are running the full content pipeline autonomously. Execute each step in order:

**Step 1 — Research**
Follow Agent 1 instructions exactly. Write 5 topics to outputs/topics.md.

**Step 2 — Pick best topic**
Read outputs/topics.md. Pick the topic with the highest viral score. 
If tied, pick the most controversial or emotionally charged one.
Remember which number topic you picked.

**Step 3 — Hooks**
Follow Agent 2 instructions exactly using your selected topic.
Write 8 hooks to outputs/hooks.md.

**Step 4 — Pick best hook**
Read outputs/hooks.md. Pick the hook with the highest scroll-stop score.
If tied, pick the most curiosity-gap driven one.
Remember which number hook you picked.

**Step 5 — Script**
Follow Agent 3 instructions exactly using your selected hook.
Write script to outputs/script.md.

**Step 6 — Suggestions**
Follow Agent 4B instructions exactly.
Write suggestions to outputs/suggestions.md.

**Step 7 — Summary**
Write a file outputs/autopilot_summary.md with:
- Selected topic and why
- Selected hook and why  
- Script (full text)
- Top 3 suggestions
- One line verdict: "READY TO FILM" or "NEEDS WORK"