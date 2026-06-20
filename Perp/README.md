# ENCOR 350-401 Self-Study System — README

This is a complete self-study system for the Cisco CCNP **ENCOR 350-401** exam, plus an optional **Post-ENCOR** track (ENARSI / ENSLD / ENAUTO directions) for when you pass the core exam.

It is opinionated: it assumes you are a working network engineer with hands-on background, prefer CLI to GUI, and want to understand things rather than memorize them. It is built around two video libraries (CBT Nuggets Jeff Kish ENCOR + INE legacy CCIE courses), the official Cisco ENCOR OCG, and EVE-NG labs.

**Target audience:** anyone preparing for CCNP ENCOR who wants more than a video binge — a 14-week plan with labs, drills, notes, spaced repetition, and a tracker. Reading the same materials passively will not get you the exam. Doing the work in this system will.

---

## What's in the box

### Core ENCOR (the 14-week run)

| File | What it is | When you touch it |
|---|---|---|
| `ENCOR-Study-Plan.md` | The 14-week plan. Topic per week, video modules mapped to each week, OCG chapters, weekly cadence. **Read this first.** | Every Monday — open it, see what week you're on, see what to watch. |
| `ENCOR-Jeff-Kish-Notes.md` | Your personal notes file. Empty headings per week — you fill it in. | Every Friday (notes pass). |
| `ENCOR-Labs-EVE-NG.md` | 21 EVE-NG labs (A1–AU3) covering every blueprint domain. Paste-ready configs, topology diagrams, verification commands. Has an Image Matrix at the top mapping each lab to vIOS-L2 / vIOS-L3 / CSR1000v. | Wed + Thu (lab nights). |
| `ENCOR-CLI-Drills.md` | ~60 short (5–15 min) single-topic CLI exercises. | Wed/Thu warmup + Fri cooldown + final-mile marathon. |
| `ENCOR-Practice-Questions.md` | ~200 Q&A prompts, ~15 per week. | Every Saturday — turn into Anki cards. |
| `ENCOR-Progress-Tracker.md` | Human-readable weekly tracker. | Every Sunday — fill it in. |
| `ENCOR-Progress-Tracker.csv` | Same tracker as a spreadsheet (one row per week). | Every Sunday. |

### Post-ENCOR (the 16-week sequel — start only after you pass)

Same structure, focused on advanced routing, design, automation, and being employable beyond Cisco-only roles. Vendor-light (Cisco + MikroTik + FRR + Juniper). Files:

- `Post-ENCOR-Study-Plan.md`
- `Post-ENCOR-Labs-EVE-NG.md`
- `Post-ENCOR-CLI-Drills.md`
- `Post-ENCOR-Practice-Questions.md`
- `Post-ENCOR-Progress-Tracker.md` and `.csv`

**Do not start Post-ENCOR until ENCOR is passed.** Mixing the two will dilute focus and you'll fail ENCOR.

---

## Prerequisites

### Knowledge
- CCNA-level fluency (subnetting, basic OSPF, VLANs, ACLs).
- Comfortable on Linux CLI and SSH.
- English at least B1 for reading the OCG.

### Hardware / software
- A machine with **16 GB RAM minimum** for EVE-NG (you can survive on 8 GB but expect to shut down labs aggressively between sessions).
- **EVE-NG Community** (free) or Pro. Bare-metal or as a VM in VMware Workstation / ESXi.
- Three Cisco images uploaded to EVE-NG:
  - `vIOS-L2` (vIOS-L2 Adventerprise)
  - `vIOS-L3` (vIOS Adventerprise)
  - `CSR1000v 17.3.04a` (csr1000v-universalk9.17.03.04a)
- A terminal (Tabby, SecureCRT, PuTTY — anything that does SSH cleanly).
- **Anki** (free, all platforms) for spaced repetition.

### Books / videos (the spine)
- **OCG** — *CCNP and CCIE Enterprise Core ENCOR 350-401 Official Cert Guide*, Edwards/Tatum. The most important single book.
- **CBT Nuggets** — Jeff Kish CCNP ENCOR 350-401 course (the "spine" video series, ~64 modules).
- **INE Legacy CCIE-style courses** — used as deep dives when CBT doesn't click on a topic.
- **Optional but recommended:** Boson ExSim ENCOR practice exams (the closest thing to real exam difficulty).

If you don't own CBT/INE videos, substitute with: OCG cover-to-cover + Jeremy's IT Lab (free YouTube) + Kevin Wallace ENCOR. The plan still works.

### Time budget
- **~10–12 hours per week** for 14 weeks = ~150 hours total. Less than that and you're cramming; more than that and you'll burn out before exam day.
- Realistic minimum if you have a full-time job: 90 min on weeknights × 4 + 3 hours on weekend = 9 hours.

---

## How a typical week works

Read this carefully — it's the whole system in one table.

| Day | Activity | Time | Files touched |
|---|---|---|---|
| **Mon** | Watch ~½ of this week's CBT Jeff Kish modules. Light note-taking only. | 60–90 min | Study Plan (reference) |
| **Tue** | Finish CBT modules. Read the corresponding OCG chapter(s). | 90–120 min | Study Plan, OCG |
| **Wed** | **Lab night #1.** Warm up with 2–3 CLI drills from the week's domain. Then build the week's main lab. Get it working end-to-end. | 90–120 min | Labs file, CLI Drills, EVE-NG |
| **Thu** | **Lab night #2.** Break things on purpose. Misconfigure on one side, see what error appears, fix it. Try the "What if?" variations in the lab notes. | 90–120 min | Labs file, EVE-NG |
| **Fri** | **Notes pass.** Write this week's section of `ENCOR-Jeff-Kish-Notes.md` in your own words. Include a "Things I got wrong" subsection. Cooldown with one CLI drill, no peeking at the solution. | 60–90 min | Notes file, CLI Drills |
| **Sat** | **Anki day.** Open `ENCOR-Practice-Questions.md`, turn this week's ~15 prompts into Anki cards. Add 3–5 of your own from Friday's notes. Run the full Anki due queue. Optional: blank-page recall test (write a 200-word summary of the topic from memory, diff against your notes). | 60–90 min | Practice Questions, Anki |
| **Sun** | **OCG slow re-read** of the week's chapter. Update `ENCOR-Progress-Tracker.csv` and `.md`. Send a "Week N done" status. | 45–60 min | OCG, Tracker |

---

## How each file is meant to be used

### `ENCOR-Study-Plan.md` — the map

Read the **Week N** section every Monday morning. It tells you:
- Topic of the week
- Which CBT modules to watch (named exactly as the folders on your disk)
- Which INE modules to use if you want deeper dive
- OCG chapters
- Which lab to build
- The "Show commands you should know by Sunday night" list — your minimum self-check

The **Weekly cadence** table near the bottom is the same one shown above.

The **Owned Video Library — Inventory** section lists every CBT and INE folder you own with the week each is mapped to. Useful for `Ctrl-F` when you want to find a topic later.

### `ENCOR-Jeff-Kish-Notes.md` — your filter

This is where you write things **in your own words**. It is not a re-transcription of the video. Each week's section should contain:

1. **Concept bullets** — 3–8 per video module, written from memory first.
2. **Things I got wrong this week** — commands or concepts you tripped on. This is the highest-value section in the whole notes file.
3. **Diagrams** — ASCII or simple sketches when the topic is visual (STP, OSPF areas, BGP attributes).
4. **Compare to my work environment** — one sentence per week relating Cisco to MikroTik / Juniper / whatever you use at work. Personalizes the material, helps exam-day recall.

The notes file is for you. Don't worry about polish.

### `ENCOR-Labs-EVE-NG.md` — the hands

Each lab has:
- **Goal** in one sentence
- **Topology diagram** with image labels (which EVE-NG image to use for each node)
- **Links table** (which port connects to which port, which VLAN/subnet)
- **Paste-ready config blocks** per device — top-to-bottom on a fresh boot
- **Verification commands** — what to run to prove it works
- **"What if?" variations** — break things on purpose

**Discipline:** before you look at the solution config, try to write it yourself. Even if you only get 40% right, the act of attempting it cements the syntax 10× better than reading the solution does.

The **Image Matrix** at the top tells you which image (vIOS-L2 / vIOS-L3 / CSR1000v) each lab uses and why.

### `ENCOR-CLI-Drills.md` — muscle memory

~60 drills, each tagged with a domain (L2 / L3 / Services / Multicast / Security / Automation). Used in three modes:

- **Warmup (Wed/Thu):** pick 2–3 drills from the week's domain before the big lab. Loads syntax into your fingers.
- **Cooldown (Fri):** pick 1 drill, do it without looking. If you can't, that becomes a Saturday Anki card.
- **Marathon (Weeks 13–14):** do 5–10 drills per session, mixed domains, no peeking. By exam day you should breeze through Drills 1–60 cold.

Each drill takes 5–15 minutes. They are not labs — they are single-command-sequence exercises.

### `ENCOR-Practice-Questions.md` — recall + Anki seed

~200 Q&A prompts, organized by week. On Saturday:

1. Open the week's block.
2. Create one Anki card per prompt (front = question, back = answer).
3. Tag them `encor::week-N::topic` so you can review by domain later.
4. Add 3–5 of your own cards from Friday's notes.

These questions are **exam-style** — concept and CLI mixed. They will not be on the exam verbatim but they cover the same recall mechanics.

### `ENCOR-Progress-Tracker.md` / `.csv` — visibility

On Sunday evening, write one row per week:
- Videos watched (Y/N or partial)
- Lab built (Y/N + notes)
- Notes written (Y/N)
- Anki cards added (count)
- Confidence 1–5
- Topics that need a second pass

The CSV is for any spreadsheet tool. The MD is for your eyes. Either or both — your choice.

---

## How the four "doing" files fit together

```
Mon/Tue (watch + read)  ──►  ENCOR-Study-Plan.md tells you what
                              │
                              ▼
Wed/Thu (build + break)  ──►  ENCOR-CLI-Drills.md (warmup, ~30 min)
                              │
                              ▼
                              ENCOR-Labs-EVE-NG.md (main lab, ~90 min)
                              │
                              ▼
Fri (write)              ──►  ENCOR-Jeff-Kish-Notes.md (week section)
                              │
                              ▼
                              ENCOR-CLI-Drills.md (cooldown, 1 drill, no peeking)
                              │
                              ▼
Sat (test recall)        ──►  ENCOR-Practice-Questions.md → Anki
                              │
                              ▼
Sun (consolidate)        ──►  OCG re-read + ENCOR-Progress-Tracker.csv
```

---

## Final 2 weeks (Weeks 13–14)

The plan front-loads new material so the last 2 weeks are pure review:

- **Mon/Tue/Wed of Week 13:** OCG full skim. Note every chapter section you can't summarize in 30 seconds.
- **Thu/Fri Week 13:** Anki full deck review. Suspend cards you've nailed 5+ times; promote weak ones.
- **Sat/Sun Week 13:** Boson ExSim or equivalent practice exam, timed (120 min). Target 80%+.
- **Mon–Thu Week 14:** CLI Drill marathon. 10 drills/day, mixed domains, no peeking. Lab-rebuild from blank: pick 3 random labs, build them from memory.
- **Fri Week 14:** Second timed practice exam. If 85%+, you're ready. If below, push the exam date back one week.
- **Sat Week 14:** Light review only. Sleep.
- **Sun Week 14 (exam day):** No new study. Read your "Things I got wrong" sections one last time. Go.

---

## Honest expectations

- **You will want to skip lab nights.** Don't. The exam tests CLI judgment, not video recall.
- **You will hit a wall in Week 5 (BGP) or Week 9 (Wireless).** That's normal. Slow down for one week, do not skip ahead.
- **Anki only works if you do it daily.** 15-minute daily review beats 90-minute weekly cramming by a factor of ~3.
- **The OCG is dry.** Read it anyway. The exam wording matches OCG wording more than video wording.
- **You will fail your first practice exam.** Score the failure, identify the 2 weakest domains, schedule a focused weekend on them.

---

## What's intentionally NOT in this system

- **No SD-WAN / Meraki / DNAC labs.** ENCOR tests these conceptually only; emulation is not free/realistic. Read the OCG chapters; do not waste lab time.
- **No emulated WLC.** Wireless is read-only + Cisco DevNet Always-On 9800 sandbox for any hands-on. No free WLC fits in EVE-NG comfortably.
- **No multi-vendor labs in ENCOR.** Single-vendor (Cisco) for the exam. Multi-vendor is moved to Post-ENCOR. Reason: bugs × bugs = quadruple bugs, and ENCOR is a Cisco exam.
- **No memorization tricks / mnemonics list.** The notes file and Anki replace that. If a mnemonic helps you, write it into your notes — it's yours.

---

## If something in this system breaks or feels wrong

This is a living system. If a lab doesn't work on your EVE-NG, if a drill has a typo, if a week feels too heavy or too light — change it. The files are markdown. Edit them. Your version is the one that matters.

A few common adjustments people make:
- Move lab nights from Wed/Thu to Sat/Sun if your weeknights are busy.
- Compress Weeks 1+2 if you already know L2 cold; use the time for BGP later.
- Skip INE deep dives if CBT + OCG is clicking — they are optional supplements, not required.

---

## Companion: Post-ENCOR

After you pass, the `Post-ENCOR-*` files give you a 16-week sequel covering advanced routing (BGP communities, MPLS L3VPN, DMVPN, multicast scaling), design fundamentals (ENSLD topics), automation (Ansible/Python/NETCONF on real devices), and intentional multi-vendor exposure (MikroTik CHR, FRR, Juniper vMX). It targets ENARSI / JNCIS-SP / CKA as the realistic next certs.

Don't open those files until ENCOR is in your pocket.

---

## Quick start (TL;DR)

1. Read `ENCOR-Study-Plan.md` end-to-end once (~30 min).
2. Make sure EVE-NG runs and all three images boot.
3. Install Anki, create a deck called "ENCOR".
4. Open `ENCOR-Progress-Tracker.csv`, add Week 1 row.
5. Start Monday morning, Week 1, with CBT Module 1.

That's it. The system carries you from there.

Good luck. Pass the exam.
