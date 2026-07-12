# ENCOR 350-401 Progress Tracker

**Purpose:** keep yourself honest. Updated after finishing each topic, not on a fixed calendar day.

**Companion:** `ENCOR-Progress-Tracker.csv` (same data in spreadsheet form).

---

## How this tracker works with the two-pass plan

Pass 1 is lightweight — no labs, no Anki. Track only that you watched the content and wrote bullets.

Pass 2 is where the detailed tracking happens — labs, notes, Anki, confidence.

---

## Pass 1 log (Weeks 1–14)

Update when you finish each week's INE + CBT viewing.

| Week | Topic                                                   | CBT watched | INE watched | OCG skimmed | 5 bullets written |
| ---- | ------------------------------------------------------- | ----------- | ----------- | ----------- | ----------------- |
| 1    | Enterprise design, HA, deployment models                | ☑           | ☑           | ☑           | ☑                 |
| 2    | SD-WAN + SD-Access                                      | ☐           | ☐           | ☐           | ☐                 |
| 3    | QoS, switching mechanisms, device virtualization        | ☐           | ☐           | ☐           | ☐                 |
| 4    | Data path virtualization (VRF, GRE, IPsec, LISP, VXLAN) | ☐           | ☐           | ☐           | ☐                 |
| 5    | Trunking + EtherChannel                                 | ☐           | ☐           | ☐           | ☐                 |
| 6    | Spanning Tree Protocol                                  | ☐           | ☐           | ☐           | ☐                 |
| 7    | EIGRP review + OSPF                                     | ☐           | ☐           | ☐           | ☐                 |
| 8    | BGP                                                     | ☐           | ☐           | ☐           | ☐                 |
| 9    | Wireless design + implementation                        | ☐           | ☐           | ☐           | ☐                 |
| 10   | NTP, NAT, FHRP, Multicast                               | ☐           | ☐           | ☐           | ☐                 |
| 11   | Monitoring tools, NetFlow, SPAN, IP SLA, DNA Center     | ☐           | ☐           | ☐           | ☐                 |
| 12   | Device access control, ACLs, CoPP                       | ☐           | ☐           | ☐           | ☐                 |
| 13   | ZBFW, wireless security, endpoint security              | ☐           | ☐           | ☐           | ☐                 |
| 14   | Automation + Programmability                            | ☐           | ☐           | ☐           | ☐                 |

**Pass 1 complete when:** 80%+ of topics can be explained in 2 minutes without notes. Write weak topics below before starting Pass 2.

**Weak topics from Pass 1 (fill in before Pass 2):**
```
Week 7 OSPF/EIGRP: 
Week 8 BGP: 
Week 4 Overlays: 
Other: 
```

---

## Pass 2 block tracker (Weeks 15–26)

| Block | Topic | INE re-watch | GNS3vault labs | Supplement lab | OCG re-read | Notes written | Anki added | CLI drills | Confidence (1–5) | Needs review |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | STP + Guards | ☐ | pvst/rpvst/mst/guards | — | Ch. 2–4 | ☐ | ☐ | ENCOR drills | ___ | |
| 2 | VLANs + EtherChannel | ☐ | vlans/trunks/pagp-lacp | — | Ch. 5 | ☐ | ☐ | | ___ | |
| 3 | OSPF | ☐ | 8 OSPF labs | — | Ch. 8–10 | ☐ | ☐ | | ___ | |
| 4 | BGP | ☐ | 12 BGP labs | — | Ch. 11–12 | ☐ | ☐ | | ___ | |
| 5 | Multicast | ☐ | 5 multicast labs | — | Ch. 13 | ☐ | ☐ | | ___ | |
| 6 | FHRP + QoS | ☐ | hsrp/vrrp/glbp | — | Ch. 14–15 | ☐ | ☐ | | ___ | |
| 7 | IP Services | ☐ | nat/ntp/snmp/sla | — | Ch. 15, 24 | ☐ | ☐ | | ___ | |
| 8 | Overlays (VRF, GRE, IPsec, VXLAN) | ☐ | gre/ipsec/vrf-lite | V3 + A2 (supplement) | Ch. 16, 23 | ☐ | ☐ | | ___ | |
| 9 | Wireless | — | — | DevNet 9800 sandbox | Ch. 17–21 | ☐ | ☐ | | ___ | |
| 10 | Network Assurance | ☐ | snmp/syslog/ntp/sla | NA2 + NA3 (supplement) | Ch. 24 | ☐ | ☐ | | ___ | |
| 11 | Security | ☐ | aaa/acl/copp/zbf | — | Ch. 25–28 | ☐ | ☐ | | ___ | |
| 12 | Automation | ☐ | — | AU1 + AU2 + AU3 (supplement) | Ch. 29–30 | ☐ | ☐ | | ___ | |

---

## Confidence scale

| Score | What it means |
|---|---|
| 1 | Lost. Need to redo the block from scratch. |
| 2 | Followed but couldn't reproduce without the solution open. |
| 3 | Can do it with notes open. |
| 4 | Can do it from memory in calm conditions. |
| 5 | Can do it from memory at 3am after being woken up. |

Target before the final 2 weeks: average 4 across all blocks.

---

## Trigger rules

| Condition | Action |
|---|---|
| Confidence ≤ 3 on a block | Re-watch the weak INE module + redo one GNS3vault lab before moving on |
| Confidence ≤ 3 on two consecutive blocks | Hard stop. One week of review before continuing. |
| Lab not built | Block is NOT complete. Roll over. |
| Practice Q score < 70% in any block | Re-lab the matching week. |

---

## Final 2 weeks checklist (Weeks 25–26)

**Week 25 — Weak-topic sprint:**
- [ ] Pulled all "Things I got wrong" sections into one page
- [ ] Re-drilled every CLI command I kept getting wrong
- [ ] Full Anki deck review — suspended cards nailed 5+ times
- [ ] One GNS3vault lab from weakest domain — no starter configs

**Week 26 — Integration:**
- [ ] 3 random GNS3vault labs from different domains — from memory, no task list peeking for 30 min
- [ ] Read full notes file start to finish
- [ ] Optional: Boson ExSim timed (120 min) — target 80%+
- [ ] If below 80%: add one week on the 2–3 lowest-scoring domains

---

## Cumulative dashboard

Update after each block.

| Metric | Target | Current |
|---|---|---|
| Pass 1 weeks completed | 14 | ___ |
| Pass 2 blocks completed | 12 | ___ |
| GNS3vault labs done | ~40 | ___ |
| Supplement labs done | 7 | ___ |
| Average confidence across blocks | ≥ 4 | ___ |
| Anki cards in deck | ≥ 150 | ___ |
| Daily Anki review streak | ≥ 80% of days | ___% |

---

## Block completion template — copy-paste after each block

```
## Block N — [Topic] — finished DD-MM-YYYY

INE re-watch: [modules, or "skipped — Pass 1 was solid"]
GNS3vault labs: [lab names] ✓
Supplement lab: [lab name] ✓  (or "none this block")
OCG re-read: Ch. X–Y ✓
Notes written: ✓
Anki: [N] cards added
Confidence: N/5
Needs review: [specific topic or command]

Things I got wrong:
- 
- 
```
