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
- HOOK (0-5 sec): the selected hook, word for word
- OPEN LOOP (5-15 sec): tease what they will learn, do not reveal yet
- CONTEXT (15-45 sec): why this matters right now
- MAIN CONTENT (45 sec - 3 min): the actual value, broken into 3 clear points
- RE-HOOK (at 60 sec mark): one sentence to keep them watching
- RE-HOOK (at 2 min mark): one sentence to keep them watching  
- CTA (last 20 sec): subscribe, comment, next video

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
