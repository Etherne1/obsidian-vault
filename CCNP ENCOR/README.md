# CCNP Enterprise Self-Study System

A complete self-study system for the **Cisco CCNP ENCOR 350-401** exam and a **Post-ENCOR** advanced track covering deep routing, MPLS, DMVPN, security hardening, telemetry, and automation.

Built for working network engineers who want to actually understand the material — not just pass an exam. If you want a video-binge-and-hope approach, this is the wrong system.

---

## Who this is for

- Network engineers with CCNA-level background (VLANs, basic OSPF, ACLs, subnetting — comfortable)
- People who prefer CLI to GUI
- People with irregular study schedules — some days 2 hours, some days nothing
- People who want the knowledge to be useful at work, not just on an exam

**Prerequisites:** CCNA-level fluency, Linux CLI + SSH comfort, English at B1+ for reading documentation.

---

## What's in the system

### ENCOR track (the foundation)

| File | What it is |
|---|---|
| `ENCOR-Study-Plan-TwoPass.md` | **Start here.** 14-week two-pass plan. INE as primary video source, CBT as secondary, GNS3vault labs in Pass 2. |
| `ENCOR-Labs-EVE-NG.md` | Lab book for Pass 2. GNS3vault labs mapped per topic block + hand-written EVE-NG labs for modern topics. |
| `ENCOR-External-Labs-Map.md` | Full GNS3vault archive map — which labs to USE, SKIP, or save for POST-ENCOR, by topic. |
| `ENCOR-CLI-Drills.md` | ~60 short single-topic CLI exercises. Used as warmup and cooldown in Pass 2. |
| `ENCOR-Practice-Questions.md` | ~200 Q&A prompts, ~15 per topic. Convert to Anki cards in Pass 2. |
| `ENCOR-Progress-Tracker.md` / `.csv` | Weekly tracker. Update Sunday night. |

### Post-ENCOR track (start only after ENCOR is done)

| File | What it is |
|---|---|
| `Post-ENCOR-Study-Plan.md` | 16-week two-pass plan. EIGRP deep, OSPF deep, IS-IS, BGP, MPLS L3VPN, DMVPN, security hardening, telemetry, automation. INE as primary. |
| `Post-ENCOR-Labs-EVE-NG.md` | Lab book. GNS3vault as primary source, hand-written labs for DMVPN / IS-IS / OSPFv3 AFs / IPv6 FHS / gNMI / automation / capstone. |
| `Post-ENCOR-CLI-Drills.md` | ~60 drills covering advanced routing, security, and automation. |
| `Post-ENCOR-Practice-Questions.md` | Per-week Q&A prompts → Anki. |
| `Post-ENCOR-Progress-Tracker.md` / `.csv` | Weekly tracker. |

**Do not open Post-ENCOR files until ENCOR is complete.** Mixing the two dilutes focus and you will fail ENCOR.

---

## The core idea — two passes

Every topic is covered twice. This is the whole system in one sentence:

> **Pass 1: see everything once and build a mental map. Pass 2: harden it with labs, notes, and Anki.**

### Pass 1 — The Map

- Watch CBT modules first (concepts, big picture, self-check quizzes)
- Watch INE modules after CBT (same topic, protocol depth and Wireshark-level detail)
- Skim OCG chapter summaries only
- Write 3–5 bullets per topic **from memory** after both videos (close them first)
- **No labs. No Anki. No deep reading.**
- Flexible pace — if a topic takes 10 days instead of 7, extend it. Nothing breaks.

### Pass 2 — The Hardening

- Re-watch INE modules where your Pass 1 bullets were thin
- GNS3vault labs per topic (pre-validated, task-listed, YouTube walkthroughs available)
- Hand-written EVE-NG labs for modern topics GNS3vault doesn't cover
- Full notes in your own words, including a "Things I got wrong" subsection
- Build Anki deck from notes. Review 15–20 min daily.
- Attempt every lab before opening the solution config. Even 40% correct is fine — the attempt hardens the syntax.

### Why this structure works

CBT gives you the concept framework first — the big picture, the "why does this exist", and a quiz to verify you got it. INE then goes deep on the same topic — Wireshark captures, edge cases, protocol internals. Getting the concept before the depth means INE becomes enrichment rather than confusion.

Without a mental map, labs are frustrating — you spend most of the time confused about *what* to type rather than *why* it works. Pass 1 fixes this. By the time you hit Pass 2 labs, you've already seen the topology and the commands twice. You spend your time on the interesting part: why does this break, what does this show command actually tell me.

---

## Video libraries

| Library | What it is | Role |
|---|---|---|
| **CBT Nuggets ENCOR** | Jeff Kish CCNP ENCOR 350-401 (~64 modules) | Primary — watched first. Concepts, big picture, self-check quizzes. |
| **INE legacy CCIE** | René Molenaar's CCIE-caliber routing courses | Secondary — watched after CBT. Protocol depth, Wireshark-level detail. |
| **CBT Nuggets ENARSI** | CCNP ENARSI 300-410 (~40 modules) | Post-ENCOR primary |
| **CBT Nuggets ENAUTO** | DEVCOR/ENAUTO 300-435 (~21 modules) | Post-ENCOR automation track — vendor-neutral modules only |
| **ENSLD Design** | CCNP Enterprise Design 300-420 (~27 modules) | Post-ENCOR parallel track — one module per topic |
| **Pearson CCNP Route** | Legacy Pearson video (~11 lessons) | Tertiary fallback when CBT + INE both don't click |

If you don't own INE, substitute with: OCG cover-to-cover + Jeremy's IT Lab (free YouTube) + Kevin Wallace ENCOR. The plan still works but the depth is lower on routing topics.

---

## Lab infrastructure

### EVE-NG

All labs run in EVE-NG (Community edition is free).

**Minimum hardware:** 16 GB RAM. 8 GB is survivable but you'll need to shut down labs aggressively between sessions.

**Required Cisco images:**
- `vIOS-L2` (vIOS-L2 Adventerprise)
- `vIOS-L3` (vIOS Adventerprise)
- `CSR1000v 17.3.04a`

**Required for Post-ENCOR only:**
- `MikroTik CHR` (free download from mikrotik.com, boots on x86 EVE-NG)
- `FRR on Alpine/Debian Docker` (IS-IS and vendor-neutral cross-checks)
- `Juniper vMX` *(optional — everything works without it)*

### GNS3vault (the main lab source)

Pre-validated labs by René Molenaar. Every lab has a topology diagram, numbered task list, startup configs, final configs, and a YouTube walkthrough.

**Repo:** https://github.com/networklessons/labs/tree/main/gns3vault-archive/

**How to use a GNS3vault lab:**
1. Open the lab's `.md` file — read the scenario and task list.
2. Build the topology in EVE-NG matching the `.png` diagram.
3. Apply `startup-configs/RN.txt` to each router as the starting point.
4. Work through the task list. Don't open `final-configs/` until you've attempted everything.
5. Watch the YouTube walkthrough. Then diff your config against `final-configs/`.

**Platform translation note:** GNS3vault uses IOS 12.4/15.x on 3640/3725/7200. CSR1000v 17.3.04a is upward-compatible for all covered topics. Rename `Serial0/0` → `GigabitEthernet1`, `FastEthernet0/0` → `GigabitEthernet1`. L2 features use vIOS-L2 instead of the NM-16ESW module.

---

## Tools

| Tool | Why |
|---|---|
| **Anki** (free, all platforms) | Spaced repetition. 15–20 min daily in Pass 2. Beats weekly cramming by a factor of ~3. |
| **Terminal** | Tabby, SecureCRT, PuTTY — anything that does SSH cleanly. |
| **Git** | Used in the Post-ENCOR automation track. Set it up in Week 1 of Post-ENCOR. |
| **Python 3 + venv** | Post-ENCOR automation — Nornir, NAPALM, Netmiko, ncclient, requests. |
| **Docker** | Post-ENCOR telemetry stack — LibreNMS, Grafana, Prometheus, ntopng. |

---

## Time budget

- **~2 hours/day, irregular** — the system is designed for this. Some days nothing — that's fine.
- ENCOR: ~24–26 weeks total (14-week Pass 1 + 10-week Pass 2)
- Post-ENCOR: ~28–32 weeks total (16-week Pass 1 + 12-week Pass 2)
- **Full system end-to-end: roughly 12–14 months** at this pace. Not a sprint.

If you have more time, don't rush Pass 1 — the two-pass structure works because of the gap between passes, not in spite of it.

---

## Daily rhythm

| Day type | What to do |
|---|---|
| **Good day (2h)** | 90 min INE/CBT video (Pass 1) or lab session (Pass 2). 30 min bullets from memory — close the video first. |
| **Short day (45–60 min)** | One INE module + 3 bullets. Or Anki review only (Pass 2). |
| **Zero day** | Fine. Don't catch up by doubling the next day. Resume where you left off. |

**The one rule:** always write bullets after watching. Closing the video and writing from memory is worth more than re-watching the same content again.

### Pre-topic self-check (do this before every new topic)

Before opening any video, spend 5–10 minutes testing what you already know. Pull 3–5 questions at random from that topic's block in `ENCOR-Practice-Questions.md` (or `Post-ENCOR-Practice-Questions.md`) and answer them cold — no notes.

| Score | What it means | What to do |
|---|---|---|
| 4–5/5 | You already know this | Skim CBT fast, skip or lightly watch INE, redirect the saved time elsewhere |
| 2–3/5 | Normal gap | Watch both videos as planned |
| 0–1/5 | Real gap | Slow down — this is exactly what Pass 1 is for |

Be honest with the scoring — vaguely recognizing a term isn't a correct answer. The goal is finding real gaps, not confirming what you'd like to believe.

**The one rule:** always write bullets after watching. Closing the video and writing from memory is worth more than re-watching the same content again.

---

## How a typical week looks (Pass 2)

| Day | Activity |
|---|---|
| Mon | Re-watch any weak INE modules from Pass 1 notes |
| Tue | Read the week's OCG chapter (slow pass) |
| Wed | Lab night #1 — GNS3vault lab, attempt task list before opening solution |
| Thu | Lab night #2 — break things on purpose. What error appears? Fix it. |
| Fri | Notes pass — write the week's section in your own words. Include "Things I got wrong". |
| Sat | Anki — convert week's Q&A prompts to cards. Add 3–5 from Friday's notes. Run full due queue. |
| Sun | Update progress tracker. Write 3 reflection sentences (what clicked, what didn't, which command you kept getting wrong). |

Lab nights can move to Sat/Sun if weeknights are busy. The shape matters more than the exact days.

---

## Honest expectations

- **You will want to skip lab nights.** Don't. The exam — and more importantly, your job — tests CLI judgment, not video recall.
- **BGP (ENCOR Week 5) and Wireless (ENCOR Week 10) are walls.** Slow down, don't skip ahead.
- **Anki only works if you do it daily.** 15 minutes daily beats 90 minutes weekly by a significant margin.
- **Pass 1 will feel unproductive.** That's normal. The material isn't sticking *yet* — it sticks in Pass 2, and much faster than it would have without Pass 1.
- **You will fail your first practice exam.** Score it, find the two weakest domains, spend a focused weekend on them.
- **Post-ENCOR is harder than ENCOR.** MPLS, DMVPN Phase 3, and the automation capstone are genuinely difficult. Budget more time per week in Blocks 10–16.

---

## What's intentionally not in this system

- **No SD-WAN / Meraki / DNAC labs.** ENCOR tests these conceptually only. Read the OCG/design modules; don't waste lab time on emulation that isn't realistic.
- **No emulated WLC.** Wireless is read-heavy + DevNet Always-On 9800 sandbox for hands-on. No free WLC fits comfortably in EVE-NG.
- **No memorization tricks.** Notes and Anki replace that. Write your own mnemonics into your notes if they help — they're yours.
- **No Cisco-only tunnel vision in Post-ENCOR.** MikroTik CHR and FRR cross-checks are deliberate — the Russian/CIS market uses a lot of MikroTik and open-source routing. Vendor-neutral depth is more employable than deep Cisco-only knowledge.

---

## Quick start

1. Read `ENCOR-Study-Plan-TwoPass.md` end to end once (~30 min).
2. Make sure EVE-NG runs and all three Cisco images boot.
3. Install Anki, create a deck called `ENCOR`.
4. Open `ENCOR-Progress-Tracker.csv`, add a Week 1 row.
5. Start Pass 1, Week 1 — open INE `7-Hardware & Software Switching Mechanisms` and watch the first module.

That's it. The system carries you from there.

---

## For your mate — where to start if new to the system

If someone hands you this folder without explanation:

1. Read this file first.
2. Read `ENCOR-Study-Plan-TwoPass.md` — it's the map. Everything else is referenced from it.
3. Don't open `Post-ENCOR-*` files until you finish ENCOR. Seriously.
4. The GNS3vault link above is the lab source. Clone it locally or open labs directly on GitHub.
5. Set up EVE-NG and get all three images booting before you reach Pass 2, Week 1 — you don't want to debug image imports when you're trying to do OSPF.

Good luck. Pass the exam — or more importantly, understand the material well enough that you don't need to.
