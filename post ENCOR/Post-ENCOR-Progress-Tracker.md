# Post-ENCOR Progress Tracker

**Purpose:** keep yourself honest. One row per week. Update Sunday night, no exceptions.

**Companion CSV:** `Post-ENCOR-Progress-Tracker.csv` (same data, importable into Excel/LibreCalc/Notion).

---

## Weekly Status Table

| Week | Theme | Spine (A) | Design (C) | Deep dive (D/E) | Lab done | Anki added | Practice Q % | CLI drills | Confidence (1-5) | Needs review |
|------|-------|-----------|------------|-----------------|----------|------------|--------------|------------|------------------|--------------|
| 1 | VRF-Lite + BFD | A-4 (mods 19-23), A-5 | C-1 | E-4, E-5 | L1 ☐ | ☐ | ___% | D-01, D-02 | ___ | |
| 2 | Route manipulation + PBR | A-1, A-3 | C-11 | D-L6, E-4 | L2 ☐ | ☐ | ___% | D-03, D-04 | ___ | |
| 3 | Redistribution | A-1 (re-read) | C-4 | D-L7, E-1, E-2, E-3 | L3 ☐ | ☐ | ___% | D-05 | ___ | |
| 4 | EIGRP deep (named-mode) | A-6, A-7, A-8, A-9, A-10 | C-3 | D-L1, D-L2, E-6 | L4 ☐ | ☐ | ___% | D-06, D-07 | ___ | |
| 5 | OSPFv3 + auth | A-11, A-12, A-13 | C-4 | D-L3, D-L4, E-7 | L5 ☐ | ☐ | ___% | D-08 | ___ | |
| 6 | OSPF types/areas/V-link | A-14 to A-19 | C-4 (cont.) | none | L6 ☐ | ☐ | ___% | D-09, D-10 | ___ | |
| 7 | OSPF path pref + IS-IS | A-20 | C-2 | none | L7 ☐ | ☐ | ___% | D-11, D-42 | ___ | |
| 8 | BGP AF + RR + filtering | A-21, A-22, A-23, A-24 | C-5, C-6, C-7, C-8 | D-L5, E-9, E-10, E-11, E-12 | L8 ☐ | ☐ | ___% | D-12, D-13, D-14, D-15 | ___ | |
| 9 | BGP troubleshooting + IPv6 | A-25 | C-9 | E-13 | L9 ☐ | ☐ | ___% | D-16, D-17 | ___ | |
| 10 | MPLS + L3VPN | A-26, A-27 | (uses C-8) | D-L8, E-14, E-15 | L10 ☐ | ☐ | ___% | D-18, D-19, D-20 | ___ | |
| 11 | DMVPN + IPsec | A-28 | C-16, C-17, C-18 | D-L9, E-16, E-17, E-18 | L11 ☐ | ☐ | ___% | D-21, D-22, D-23, D-24 | ___ | |
| 12 | Infra hardening | A-29 to A-33 | (OCG read) | D-L10, E-19, E-20, E-21, E-22, E-23 | L12 ☐ | ☐ | ___% | D-25, D-26, D-27, D-28 | ___ | |
| 13 | Services & telemetry | A-34 to A-39 | C-21, C-22, C-23 | D-L11, E-24, E-25, E-26, E-27 | L13 ☐ | ☐ | ___% | D-29, D-30, D-31, D-32, D-33 | ___ | |
| 14 | DNAC skim + design wrap | A-40 | C-10, C-12, C-13, C-14, C-15, C-19, C-20, C-24 | none | L14 ☐ | ☐ | ___% | (review) | ___ | |
| 15 | Automation pivot | none | none | none | L15 ☐ | ☐ | ___% | D-34, D-35, D-36, D-37, D-38, D-39, D-40, D-41 | ___ | |
| 16 | Capstone | none | none | none | L16 ☐ | ☐ | ___% | D-43 to D-60 | ___ | |

**Legend:**
- **Spine/Design/Deep-dive cells:** library prefix + module number(s) from your owned listings (e.g., `A-4` = Library A module 4 = "Configure and Verify VRF-Lite"; `C-11` = Library C module 11 = "Explain the Hierarchical Network Model")
- **Lab done:** check when the EVE-NG lab for that week is built, proven, and rebuilt from notes
- **Anki added:** check when that week's practice Qs are converted into Anki cards
- **Practice Q %:** your score on the matching block in `Post-ENCOR-Practice-Questions.md`
- **CLI drills:** the drill IDs you completed (typed from memory, passed)
- **Confidence (1-5):**
  - 1 = lost, need to repeat the week
  - 2 = followed but couldn't reproduce
  - 3 = can do it with the notes open
  - 4 = can do it from memory in calm conditions
  - 5 = can do it from memory at 3 AM after being woken up
- **Needs review:** specific topics to revisit (e.g., "NHRP shortcut redirect timing", "BGP cluster-list loop")

---

## Trigger rules (be honest with yourself)

| Condition | Action |
|---|---|
| Confidence ≤ 3 on a week | Re-watch the trouble module + re-lab one task before moving on |
| Confidence ≤ 3 two weeks in a row | **Hard stop.** Take a full week of review before continuing. Burnout is the #1 reason these plans fail. |
| Practice Q score < 70% in any block | Re-lab the matching week. Don't fool yourself. |
| Practice Q score < 80% in week 16 capstone block | Don't book the exam yet. Do another pass through weeks 8-12. |
| Lab not built in a given week | Did NOT complete the week. Roll over to next week and double up only once. |
| Three consecutive weeks of falling behind | Reduce plan to 12 weeks by cutting weeks 13-14 (skip telemetry deep dive + design wrap-up if needed; you can do them post-exam). Don't drop weeks 9-12 (BGP/MPLS/DMVPN/Hardening) under any circumstance. |

---

## Reflection prompts (write 2-3 sentences each Sunday)

1. **What's the one thing I understand better now than I did last Monday?**
2. **What's the one thing I still don't fully understand?** (becomes the focus of next week's deep-dive)
3. **Which command did I type wrong from memory most often this week?** (becomes a flashcard)
4. **Did I commit my lab configs to Git this week?** (yes/no — if no for two weeks in a row, the automation track is collapsing)
5. **Energy / motivation level (1-5):** if ≤ 2 for two weeks, take a Saturday completely off networking. The plan finishes on time anyway.

---

## End-of-week template (copy-paste into your tracker each Sunday)

```
## Week N — [Topic] — Sunday DD-MM-YYYY

Spine: A-NN, A-NN ✓
Design: C-NN ✓
Deep dive: D-Lx ✓ E-NN ✓
Lab: LN ☑ (rebuilt from notes ✓)
Anki: 15 cards added to deck `Post-ENCOR/W-N`
Practice Q: Block N — 17/20 (85%)
CLI drills: D-NN ✓ D-NN ✓
Confidence: 4/5
Needs review: NHRP redirect timing in Phase 3 spoke-spoke flow
Energy: 4/5

Reflection:
- Better than last week: explicit ext-community signaling in L3VPN.
- Still unclear: how exactly OMP differs from MP-BGP in SD-WAN.
- Most-missed command: `address-family ipv4 vrf X` placement under `router bgp`.
- Git commits: 8
```

---

## Cumulative dashboard (update at the end of each week)

| Metric | Target | Current |
|---|---|---|
| Weeks completed | 16 | ___ |
| Total labs built | 16 | ___ |
| Average Practice Q score | ≥80% | ___% |
| CLI drills mastered | 60 | ___ |
| Anki cards in deck | ≥250 | ___ |
| Daily Anki review streak | ≥80% of days | ___% |
| Git commits in `network-automation` repo | ≥100 by Week 16 | ___ |
| Confidence average across all weeks | ≥4 | ___ |
| Capstone topology operational | Yes by Week 16 Sun | ☐ |
| README + diagram presentable to recruiter | Yes by Week 16 Sun | ☐ |

---

## Post-plan decisions (fill in at end of Week 16)

- [ ] Sit **CCNP ENARSI 300-410** exam? Date: ____________
- [ ] Sit **Juniper JNCIS-SP** exam? Date: ____________
- [ ] Push capstone repo public on GitHub? Date: ____________
- [ ] Update LinkedIn with cert + repo link? Date: ____________
- [ ] Next study direction: ENARSI → CCIE / JNCIP-SP / RHCE / CKA / Linux-networking / something else?

Write a 5-sentence self-review of the whole plan: what worked, what didn't, what you'd change for the next learning cycle.
