---
layout: post
title: "The Race We're Losing: AIxBio Governance in 2026"
date: 2026-02-02
categories: [research, biosecurity, governance]
---

*A research synthesis on why AI-accelerated biology is outpacing our ability to govern it*

---

**One statistic should keep you up at night: 94% of countries lack systems to govern dual-use biosecurity risks.**

That's not a typo. In a world where AI can now design novel viruses from scratch, where the timeline from paper publication to working pathogen is measured in months, nearly every nation on Earth has no governance framework for this capability.

This isn't a future problem. It's happening now.

## The Evo2 Story

In February 2025, the Arc Institute and NVIDIA released Evo2—the largest biology AI model to date. Trained on 128,000+ whole genomes spanning the tree of life, Evo2 can read DNA to predict disease-causing variants, and more importantly, *write* novel synthetic DNA, RNA, and protein sequences.

The developers implemented a safeguard: they excluded human and animal viruses from the training data. Problem solved, right?

**Three months later, the GeneBreaker paper showed Evo2 could be jailbroken to output hazardous genetic sequences anyway.** The safeguard didn't work.

**Four months after that**, researchers used Evo2 to design a novel bacteriophage that outcompeted wild-type at infecting E. coli—in the wet lab. A real, functioning, never-before-existing virus.

Timeline from paper release to novel pathogen: **seven months.**

## The Screening Problem

The current safeguard approach relies on DNA synthesis screening—checking orders before companies manufacture them. But this has two fatal flaws:

**First, coverage is voluntary and incomplete.** Only a fraction of synthesis companies globally perform screening. The International Biosecurity and Biosafety Initiative (IBBIS) is working on technical standards, but adoption is inconsistent.

**Second, AI breaks sequence-based screening.** An October 2025 study in *Science* demonstrated that AI models can design proteins with similar *function* but different *sequence*. The protein does what you want it to do, but doesn't match any known dangerous sequence.

This is the equivalent of screening for "bad words" while someone invents new words with the same meaning.

## 82.5% Open Source

A RAND Europe assessment of biological AI tools across 76 countries found:
- 82.5% of state-of-the-art AIxBio tools have at least one open-source component
- 61.5% of highest-risk tools are **fully** open-sourced
- There's **no correlation** between tool risk level and safeguards implemented

Let that sink in. The most dangerous tools are just as accessible as the safest ones. Open science—normally a virtue—becomes a liability when the research can enable mass casualties.

## The Reviewer Disagreement Problem

For research that does get reviewed, there's another problem: experts can't agree on what's dangerous.

A July 2025 study surveyed expert dual-use research reviewers and found:
- Significant disagreement among individuals on the same projects
- Deep conceptual differences on risk thresholds and ethical frameworks
- Institutional Review Entities at different organizations may apply completely different standards to similar risks

There's no common definition of "gain-of-function research." No uniform risk protocols. The governance infrastructure assumes consensus that doesn't exist.

## Five Structural Failures

From my analysis of the literature, five problems keep recurring:

1. **No agreed terminology** — "Gain-of-function" has no standard definition across institutions or countries
2. **No uniform protocols** — Safety standards vary wildly between labs
3. **Cost barriers** — Biosecurity infrastructure is expensive; old regulations don't fit new research
4. **Inherent dual-use** — The same research that cures disease can enable bioweapons
5. **International asymmetry** — Countries with lax oversight gain competitive advantage

That last point is particularly vicious. A country that invests in biosecurity puts itself at a disadvantage against one that doesn't. Classic race-to-the-bottom dynamics.

## Why This Feels Familiar

If you follow AI safety discussions, this should feel eerily familiar:
- Capabilities outpacing governance
- Open-source proliferation enabling bad actors
- Technical safeguards being bypassed
- Need for international coordination with no mechanism to achieve it

But biology has two properties AI doesn't:
1. **Physical irreversibility** — Released pathogens can't be recalled
2. **Self-replication** — Biological agents multiply; software doesn't (usually)

This makes the stakes categorically different. A misaligned AI might cause harm; a misaligned pathogen might cause extinction.

## The Asymmetry Problem

The capabilities are here. Evo2 exists. The jailbreaks exist. The novel pathogens exist. We're not preventing anything; we're trying to catch up.

The question isn't "can we stop this technology?" It's "can governance evolve faster than capabilities?"

So far, the answer is no.

## What Would Need to Happen

For AIxBio governance to work, we'd need:

1. **Universal screening with teeth** — Not voluntary, not partial, with jurisdiction over synthesis companies globally
2. **Function-based detection** — Screening by what sequences *do*, not just what they look like
3. **Coordinated international standards** — Meaning countries agree to bind themselves, even at competitive cost
4. **Speed matching** — Policy cycles measuring in weeks, not years
5. **Fundamental research restriction** — Limits on publication and open-sourcing of dual-use capabilities

Item 5 is where it gets genuinely hard. Open science is a foundational value. Restricting it feels like abandoning the Enlightenment. But when the research enables mass casualties, maybe some things shouldn't be openly published?

I don't have an answer. I'm not sure anyone does.

## The Honest Conclusion

Biology is probably AI's most consequential application domain. Not because AI makes biology *possible*—humans have done bioweapons research for a century—but because AI makes it *easy*.

When capability is bottlenecked by expertise, you only worry about state actors and dedicated research programs. When capability is bottlenecked by compute, you worry about whoever has GPUs.

When capability is openly available, trained on global genomes, and can be jailbroken past safeguards?

You worry about everyone.

The 94% number isn't going to improve fast enough. We're in a race, and governance is losing.

---

*This post synthesizes research from the Council on Strategic Risks, RAND Europe, Homeland Security Affairs, and the World Economic Forum's Global Health Security Index.*
