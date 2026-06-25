# Post-ENCOR Progress Tracker

**Purpose:** keep yourself honest. One row per Pass 2 block. Update after finishing each block — not on a fixed calendar day.

**Companion CSV:** `Post-ENCOR-Progress-Tracker.csv` (same data, importable into Excel/LibreCalc/Notion).

---

## How this tracker works with the two-pass plan

Pass 1 is intentionally lightweight — no labs, no Anki, no deep notes. There is nothing to track in detail during Pass 1 except that you watched the content. Use the Pass 1 log below for that.

Pass 2 is where the detailed tracking happens. One row per topic block.

---

## Pass 1 log (Weeks 1–16)

Simple checkboxes. Update when you finish each week's INE + CBT viewing.

| Week | Topic | INE watched | CBT watched | Design (C) watched | 5 bullets written | Notes |
|---|---|---|---|---|---|---|
| 1 | VRF-Lite + BFD | ☐ | ☐ | ☐ | ☐ | |
| 2 | Route manipulation + PBR | ☐ | ☐ | ☐ | ☐ | |
| 3 | Redistribution | ☐ | ☐ | ☐ | ☐ | |
| 4 | EIGRP deep | ☐ | ☐ | ☐ | ☐ | |
| 5 | OSPFv3 + auth | ☐ | ☐ | ☐ | ☐ | |
| 6 | OSPF areas + network types | ☐ | ☐ | ☐ | ☐ | |
| 7 | OSPF path pref + IS-IS | ☐ | ☐ | ☐ | ☐ | |
| 8 | BGP deep | ☐ | ☐ | ☐ | ☐ | |
| 9 | BGP troubleshooting + IPv6 | ☐ | ☐ | ☐ | ☐ | |
| 10 | MPLS + L3VPN | ☐ | ☐ | ☐ | ☐ | |
| 11 | DMVPN + IPsec | ☐ | ☐ | ☐ | ☐ | |
| 12 | Security | ☐ | ☐ | ☐ | ☐ | |
| 13 | Telemetry + services | ☐ | ☐ | ☐ | ☐ | |
| 14 | Design wrap-up | ☐ | ☐ | ☐ | ☐ | |
| 15 | Automation | ☐ | ☐ (B) | — | ☐ | |
| 16 | Pass 1 self-check | — | — | — | ☐ | Weak topics: |

**Pass 1 complete when:** 80%+ of topics can be explained in 2 minutes without notes. Write your weak-topic list in the Week 16 notes column — those get extra attention in Pass 2.

---

## Pass 2 block tracker

| Block | Topic | INE re-watch | GNS3vault labs done | Hand-written lab done | OCG re-read | Notes written | Anki added | CLI drills | Confidence (1–5) | Needs review |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | VRF-Lite + BFD | ☐ | vrf-lite, vrf-routing | BFD hand-written | Ch. 3 | ☐ | ☐ | D-01, D-02 | ___ | |
| 2 | Route manipulation + PBR | ☐ | policy-based-routing, bgp-filtering | Extension + MikroTik | Ch. 4 | ☐ | ☐ | D-03, D-04 | ___ | |
| 3 | Redistribution | ☐ | ospf-single-area, eigrp-basic | Full redistribution | Ch. 5 | ☐ | ☐ | D-05 | ___ | |
| 4 | EIGRP deep | ☐ | eigrp-basic/auth/summarization | Named-mode extension | Ch. 6 | ☐ | ☐ | D-06, D-07 | ___ | |
| 5 | OSPFv3 + auth | ☐ | ospf-authentication, ospf-nssa, ospf-vlink | OSPFv3 AF + FRR | Ch. 7 | ☐ | ☐ | D-08 | ___ | |
| 6 | OSPF areas + types | ☐ | stub/nssa/vlink/summarization labs | Network types (no FR) | Ch. 8 | ☐ | ☐ | D-09, D-10 | ___ | |
| 7 | IS-IS | — (no INE module) | — (no GNS3vault) | FRR + vIOS-L3 full | Ch. 9 | ☐ | ☐ | D-11, D-42 | ___ | |
| 8 | BGP deep | ☐ | 12 GNS3vault BGP labs | IPv6 AF extension | Ch. 10 | ☐ | ☐ | D-12, D-13, D-14, D-15 | ___ | |
| 9 | BGP troubleshooting | ☐ | bgp-ebgp-multihop/update-source/md5 | 6-bug gauntlet | Ch. 11 | ☐ | ☐ | D-16, D-17 | ___ | |
| 10 | MPLS L3VPN | ☐ | mpls-ldp, basic-mpls-vpn, mpls-vpn-pe-ce | eBGP PE-CE + 6VPE | Ch. 12 | ☐ | ☐ | D-18, D-19, D-20 | ___ | |
| 11 | DMVPN + IPsec | ☐ | gre-tunnel-basic, gre-over-ipsec, s2s-ipsec | Full DMVPN P1→P3 + IKEv2 | Ch. 13 | ☐ | ☐ | D-21, D-22, D-23, D-24 | ___ | |
| 12 | Security | ☐ | aaa/acl/copP/zbf GNS3vault labs | IPv6 FHS + production CoPP | Ch. 14-15 | ☐ | ☐ | D-25, D-26, D-27, D-28 | ___ | |
| 13 | Telemetry + services | ☐ | snmp/ntp/sla/kron GNS3vault labs | LibreNMS/gNMI/cross-VRF DHCP | Ch. 16 | ☐ | ☐ | D-29–D-33 | ___ | |
| 14 | Design translation | — | — | Notebook activity | Design modules | ☐ | ☐ | — | ___ | |
| 15 | Automation build | — | — | Full Git repo | Ch. 17-18 | ☐ | ☐ | D-34–D-41 | ___ | |
| 16 | Capstone | — | — | Full topology + Ansible | — | ☐ | ☐ | D-43–D-60 | ___ | |

---

## Confidence scale

| Score | What it means |
|---|---|
| 1 | Lost. Need to redo the block from scratch. |
| 2 | Followed but couldn't reproduce without the solution open. |
| 3 | Can do it with notes open. |
| 4 | Can do it from memory in calm conditions. |
| 5 | Can do it from memory at 3am after being woken up. |

Target: average 4 across all blocks before the capstone.

---

## Trigger rules — be honest with yourself

| Condition | Action |
|---|---|
| Confidence ≤ 3 on a block | Re-watch the weak INE module + re-do one lab before moving to the next block |
| Confidence ≤ 3 on two consecutive blocks | Hard stop. Take a full week of review. Burnout is the #1 reason these plans fail. |
| Lab not built in a block | Did NOT complete the block. Don't mark it done. Roll over. |
| CLI drill failed (had to peek) | Becomes tomorrow's Anki card. Repeat the drill next session. |
| Three consecutive blocks falling behind | Cut Block 14 (design translation) — you can do it post-capstone. Don't cut Blocks 10–12 under any circumstance. |

---

## Reflection prompts — write 2–3 sentences after each block

1. **What's the one thing I understand better now than before this block?**
2. **What's the one thing I still don't fully understand?** — becomes the first target in the next re-watch
3. **Which command did I type wrong from memory most often?** — becomes a flashcard
4. **Did I commit lab configs to Git this block?** — if no for two blocks in a row, the automation track is collapsing
5. **Energy/motivation (1–5):** if ≤ 2 for two blocks in a row, take a session completely off. The plan finishes anyway.

---

## Block completion template — copy-paste after each block

```
## Block N — [Topic] — finished DD-MM-YYYY

INE re-watch: [modules, or "skipped — Pass 1 was solid"]
GNS3vault labs: [lab names] ✓
Hand-written lab: [lab name] ✓ (rebuilt from notes ✓)
OCG re-read: Ch. X–Y ✓
Notes written: ✓
Anki: [N] cards added to deck Post-ENCOR/B-N
CLI drills: D-NN ✓ D-NN ✓
Confidence: N/5
Needs review: [specific topic or command]
Energy: N/5

Reflection:
- Clearer now: [one thing]
- Still unclear: [one thing — becomes next re-watch target]
- Most-missed command: [command]
- Git commits this block: [N]
```

---

## Cumulative dashboard — update after each block

| Metric | Target | Current |
|---|---|---|
| Pass 1 weeks completed | 16 | ___ |
| Pass 2 blocks completed | 16 | ___ |
| Total GNS3vault labs done | ~50 | ___ |
| Total hand-written labs done | 12 | ___ |
| Average confidence across blocks | ≥ 4 | ___ |
| Anki cards in deck | ≥ 250 | ___ |
| Daily Anki review streak | ≥ 80% of days | ___% |
| CLI drills mastered (typed cold) | 60 | ___ |
| Git commits in `network-automation` repo | ≥ 100 by Block 16 | ___ |
| Capstone topology operational | Yes by Block 16 | ☐ |
| README + diagram presentable | Yes by Block 16 | ☐ |

---

## Post-plan decisions — fill in after Block 16

- [ ] Sit **CCNP ENARSI 300-410**? Date: ____________
- [ ] Sit **Juniper JNCIS-SP**? Date: ____________
- [ ] Push capstone repo public on GitHub? Date: ____________
- [ ] Update LinkedIn with cert + repo link? Date: ____________
- [ ] Next direction: ENARSI → CCIE / JNCIS-SP / RHCE / CKA / Linux networking / other?

Write a 5-sentence self-review: what worked, what didn't, what you'd change for the next learning cycle.
