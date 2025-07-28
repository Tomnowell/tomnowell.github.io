# Mid-2025 Update: I Don’t Have a Master Plan (and That’s Okay)

I’ve been learning a lot these past few years, but the headline isn’t “crushing goals.” It’s closer to: I’m fallible, I’m experimenting in public, and I don’t fully know which direction to take yet. This post is both a catch-up and a reset of how I’ll share from now on.

⸻

## What the last few years really looked like

### Wins I’m proud of
- I finished and submitted my MSc capstone: Genetic Algorithm Optimised Anomaly Detection for Intrusion Detection Systems. It demanded real trade-offs. Massive samples, memory constraints, and “good enough” metrics instead of chasing theoretical purity.
- I shipped the (very rough) first cut of TicketyDo, a Django time-boxed task app. CI with GitHub Actions, deployment on Render, domain wiring—the unglamorous realities that separate ideas from software.

### The messy bits I’d usually edit out
- I broke production migrations because I didn’t have shell access (free tier because I'm cheap). I tried a few wrong fixes before the obvious one. It was frustrating and humbling—and I learned more from that than any tutorial.
- I got a simple git conflict and still lost time overthinking rebase vs. merge. (Kept the remote README, pushed my app changes—done. But I spiralled first.)
- Career wise - I ping-pong between Cloud/Platform, backend app dev, and security engineering. Each week I’m convinced one of them is “the path,” and the next week I’m not so sure.
- Some experiments (like my macOS virtualization side project) moved in inches, not miles. Entitlements, EFI variable stores, the Apple world is confusing and sometimes my brain is slow.

### Life context
I’m still teaching in Japan, with a young family I love and limited focus budget. That means my “career change” isn’t a sprint; it’s a series of small, deliberate (and hopeful) bets. I want to reflect that here—no  gloss.

⸻

##Why I deleted my TryHackMe walkthroughs

Tldr; I don't think anyone read them...and they didn’t represent how I want to learn—or how I want to show up online.

What replaces them isn’t silence, it’s methodology: how I frame problems, where I get stuck, why I pick one approach over another, and how I verify results.

⸻

## Things I don’t know yet (and how I’m going to find out)

### Which direction?
- Cloud/Platform (SRE/DevOps): I like systems thinking and pipelines, but I need more real-world reps with infra as code and day-2 operations.
- Backend/Product Engineering: I enjoy turning vague problems into shipped features (TicketyDo scratches this itch).
- Security Engineering/Detection: My MSc keeps pulling me back here, but I want to avoid “CTF brain” and focus on measurable detection and hardening.

### How I’ll test these paths (not just theorize)
- Time-boxed spikes: Two-week mini-projects with a single question. Example: “Ship a minimal E2E pipeline with tracing and deploy previews.”
- Artifacts over opinions: If it didn’t result in a PR, a repo, or a runbook, it doesn’t count.
- Reality checks: What felt energizing? What dragged? What would I happily repeat 10 more times?

⸻

## What I’ll publish instead (and how I’ll keep it honest)

### Weeknotes (mess and all).
Short check-ins: what shipped, what broke, one lesson learned, one open question.

### Deep dives. (maybe)
Narrow, practical posts: GA parameter tuning trade-offs; Django migrations on managed hosts; containers → k8s with cost/complexity guardrails.

### Failure logs. (definitely)
A blameless post-mortem template I’ll keep reusing. If you only ever see polished outcomes here, I’ve failed at the promise of honesty.

### Field notes (security with guardrails).
Logging that tells a story without spoilers or gray-area content.

⸻

## What’s next
- Stabilise TicketyDo and make it prettier.
- Publish a friendly guide to my MSc project: clear goals, data handling, metrics that matter, and “what I tried next.” (this is coming soon - I added a rolling window detection system and it was kinda cool)
	•	Keep contributing to open source—small, consistent improvements.

Thanks for reading, and for the patience while I figure this out in the open. I’m not aiming to look impressive—I’m aiming to become reliable. If you’re also in a period of uncertainty, I hope the weeknotes help you feel less alone.

Tom