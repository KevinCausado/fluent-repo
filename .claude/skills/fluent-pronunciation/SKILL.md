---
name: fluent-pronunciation
description: Interactive pronunciation drill for English learners — learner reads words and writes how they think they sound in Spanish figurative transcription, tutor corrects and explains the rule behind each sound. Focuses on sounds that don't exist in Spanish: flap T, schwa, short vs long vowels, silent letters, -ed endings. Triggered only when the learner types /fluent-pronunciation. Updates databases at the end.
allowed-tools: Read, Write, Bash
disable-model-invocation: true
---

# Pronunciation Drill Session

## Overview

The learner sees a word or phrase and writes how they think it sounds using Spanish figurative transcription (no IPA). The tutor corrects it, explains the rule, and gives the Spanish-readable transcription. One word at a time. Goal: train the learner's ear and mouth by making the sound system of English explicit and memorable.

This skill exists because Kevin reads English constantly but is often unsure if his mental pronunciation is correct. Typed output stands in for oral production.

## When to Use

Trigger only when the learner types `/fluent-pronunciation`. Gated with `disable-model-invocation: true`.

Skip for learners below A1 mastery 2 — they need a word bank first.

## Transcription Rules (Spanish figurative — no IPA)

Always use these conventions. Never use IPA symbols.

| Sound | Convention | Example |
|---|---|---|
| Aspirated h | j suave | house = *jaus* |
| sh | sh | she = *shi* |
| th sonora (the) | d suave | the = *de* |
| th sorda (think) | z | think = *zink* |
| Long vowel | double letter | see = *sii*, food = *fuud* |
| Short vowel | single letter | sit = *sit*, foot = *fut* |
| Schwa (unstressed) | uh | about = *uh-BAUT* |
| Flap T (American) | r suave or d suave | water = *WA-der*, written = *RI-en* |
| Silent letter | omit it | knife = *naif*, written = *RI-ten* |
| Stress | **bold** on stressed syllable | banana = *buh-**NA**-nuh* |
| Linked sounds | underscore | did you = *di_yu* |

## Instructions

### 1. Load context

```bash
python3 "${CLAUDE_PLUGIN_ROOT:-${CLAUDE_PROJECT_DIR:-.}}/.claude/hooks/read-db.py"
```

Need: `learner-profile` (level, name), `mistakes-db` (any existing pronunciation patterns).

### 2. Opening

```markdown
# 🔊 Pronunciation Drill

Hey {name}! We're going to train your ear for English sounds.

I'll show you a word. You write how you think it sounds — in Spanish, like you'd read it aloud. No IPA, no pressure.

Example:
- I show you: **water**
- You write: *wa-ter* or *gua-ter* or however you'd say it
- I correct: *(WA-der)* — flap T, the t sounds like a soft d in American English

One word at a time. Ready?
```

### 3. Select words

Pick 10-15 words per session. Mix these categories:

**Priority 1 — known problem areas for Spanish speakers:**
- Flap T words: water, butter, better, little, written, getting, matter, bottle, metal, city
- Schwa: about, family, problem, camera, banana, today, memory,otten
- Short vs long vowels: bit/beat, full/fool, bed/bade, shot/short, cut/cute
- Silent letters: knife, know, write, wrong, hour, honest, island, psychologist
- -ed endings: helped (t), robbed (d), wanted (ed/separate syllable)
- th sounds: think, the, they, three, that, month, smooth
- False friends in pronunciation: focus, video, police, garage, nature

**Priority 2 — words Kevin has encountered in sessions:**
Check `mistakes-db` and recent vocabulary for words worth drilling.

**Priority 3 — words from roleplay/context:**
Pull from words Kevin has typed or asked about recently.

### 4. One word at a time

```markdown
## Word {N}/{total}

**{word}**

How do you think this sounds? Write it in Spanish — like you'd read it aloud.
```

### 5. Evaluate the attempt

Compare the learner's transcription to the correct one. Check:

1. **Stress** — did they put emphasis on the right syllable?
2. **Vowel quality** — short vs long, schwa vs full vowel?
3. **Consonants** — flap T, th, silent letters?
4. **Linking** — for phrases, did they connect sounds naturally?

Feedback format:

```markdown
{✅ correct / 🟡 close / ❌ different}

**You wrote:** {their version}
**Correct:** {correct transcription in bold}

**Rule:** {one-line explanation of the sound rule}

**Tip:** {memory hook or comparison to Spanish}

---
```

Severity guide:
- 🟢 Stress wrong but sound right — minor
- 🟡 One sound off — moderate
- 🔴 Completely different — critical, add to mistakes-db

### 6. Rule grouping

After every 4-5 words from the same rule, add a mini-summary:

```markdown
**Pattern so far:** {rule name}
Words you've seen: {list with correct transcriptions}
These all follow the same rule: {brief explanation}
```

### 7. Learner can also ask

If the learner writes a word mid-session with "?" or "how do I say..." — answer immediately with transcription + rule, then continue the drill from where it left off.

### 8. Session summary

```markdown
## Pronunciation Session Complete!

**Words drilled:** {N}
**Accuracy:** {X}%

### Rules covered today
- {rule 1}: {words}
- {rule 2}: {words}

### Sounds to keep drilling
- {word}: you said {wrong} → correct is {right}

### Your strongest sound today
{rule or sound they got consistently right}

Keep going — pronunciation improves with repetition, not perfection.
```

### 9. Update databases

Use `fluent-db-updater`:

- `command_used: "/fluent-pronunciation"`, `skills_practiced: ["speaking"]`
- `skill_scores.speaking: {exercises: N, correct: count_correct, time_minutes}`
- `errors[]` — only 🔴 critical misses (completely wrong transcription)
- `new_vocabulary[]` — any new words introduced during the session
- `focus_next_session[]` — top 2-3 sound rules to revisit

Save to `/results/fluent-pronunciation-session-{NNN}.md`.

## Sound Rules Reference (English for Spanish Speakers)

### Flap T (American English)
When *t* or *tt* appears between two vowels, or between a vowel and *r/l*, it softens to a quick tap — sounds like a very fast *d* or even disappears.
- water → *(WA-der)*
- butter → *(BU-der)*
- written → *(RI-en)* or *(RI-ren)*
- getting → *(GE-ding)*
- city → *(SI-dee)*
- little → *(LI-dul)*

British English keeps the *t* hard. American English almost always flaps it.

### Schwa (ə)
The most common sound in English. Any unstressed vowel can become a schwa — a lazy, neutral "uh". Spanish speakers tend to pronounce every vowel fully; English reduces them.
- about → *(uh-BAUT)* — the *a* is a schwa
- family → *(FAM-uh-lee)*
- problem → *(PROB-lum)*
- camera → *(KAM-ruh)*
- today → *(tuh-DEI)*

### Short vs Long Vowels
English has pairs of vowels that look similar but sound very different. Meaning changes.
- bit *(bit)* vs beat *(biit)*
- full *(ful)* vs fool *(fuul)*
- cut *(kat)* vs cute *(kyuut)*
- shot *(shot)* vs short *(short)*

### Silent Letters
English has many letters that are written but not pronounced.
- knife → *(naif)* — silent k
- know → *(nou)* — silent k
- write → *(rait)* — silent w
- hour → *(aur)* — silent h
- honest → *(ON-est)* — silent h
- island → *(AI-land)* — silent s
- psychologist → *(sai-KOL-uh-yist)* — silent p

### -ed Endings (Past Tense)
Three different sounds depending on the final consonant of the base verb:
- After voiceless sounds (p, k, f, s, sh, ch): sounds like **t** → helped *(jelpt)*, worked *(werkt)*
- After voiced sounds (b, g, v, z, m, n, l, r): sounds like **d** → robbed *(robd)*, lived *(livd)*
- After *t* or *d*: adds a full syllable **ed** → wanted *(WON-ted)*, needed *(NII-ded)*

### TH Sounds (two types)
- Voiceless th (think, three, month): tongue between teeth, no voice → sounds like **z** or **s** in Spanish figurative: *zink, zrii, manz*
- Voiced th (the, they, that): tongue between teeth, with voice → sounds like soft **d**: *de, dei, dat*

### Stress Patterns
English stress is unpredictable but critical — wrong stress makes you hard to understand.
- REcord (noun) vs reCORD (verb)
- PROtest (noun) vs proTEST (verb)
- PHOtograph vs phoTOgraphy vs photoGRAPHic

## Critical Rules

- **One word at a time.** Wait for the learner's attempt before showing the answer.
- **Never use IPA.** Spanish figurative transcription only.
- **Explain the rule, not just the answer.** The goal is pattern recognition, not memorization.
- **Never auto-invoke.** Gated; must fire only on explicit `/fluent-pronunciation`.
- **Don't over-correct accent.** The goal is intelligibility, not a perfect American accent.
- **Praise pattern recognition.** When the learner generalizes a rule correctly to a new word, call it out.
