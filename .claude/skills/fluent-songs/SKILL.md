---
name: fluent-songs
description: Song-based pronunciation and listening skill — learner picks a song, tutor delivers the full lyrics with Spanish figurative transcription line by line, explains pronunciation traps, reductions, and rhythm patterns. Builds ear training through music. Triggered only when the learner types /fluent-songs.
allowed-tools: Read, Write, Bash
disable-model-invocation: true
---

# Song Pronunciation Session

## Overview

The learner picks any English song. The tutor delivers the lyrics with Spanish figurative transcription below each line, highlights pronunciation traps (flap T, schwa, reductions, linked sounds, rap rhythm), and explains why the artist sounds different from "textbook" pronunciation.

Goal: train the ear using material the learner already enjoys. Songs work because the learner has heard them many times — the brain already has the audio pattern, the transcription connects it to explicit knowledge.

## When to Use

Trigger only when the learner types `/fluent-songs`. Gated with `disable-model-invocation: true`.

## Instructions

### 1. Ask for the song

```markdown
# 🎵 Song Pronunciation

Which song do you want to work on? Give me the title and artist.

I'll give you the full lyrics with pronunciation in Spanish so you can follow along and train your ear.
```

### 2. Deliver lyrics with transcription

Present the song in **chunks of 4-6 lines** — not the whole song at once. After each chunk, wait for the learner to confirm before continuing. This forces active reading, not passive scrolling.

Format per line:

```
"{original lyric line}"
*(figurative transcription in Spanish)*
```

Example:
```
"'Cause you're scared, I ain't there"
*(kaz yor skerd — ai EINT der)*
```

### 3. Transcription conventions

Use the same system as `/fluent-pronunciation`:

| Sound | Convention | Example |
|---|---|---|
| Aspirated h | j suave | have = *jav* |
| sh | sh | she = *shi* |
| th sonora | d suave | the = *de* |
| th sorda | z | think = *zink* |
| Long vowel | double letter | see = *sii* |
| Short vowel | single letter | sit = *sit* |
| Schwa | uh | a = *uh* |
| Flap T | d suave | better = *BE-der* |
| Silent letter | omit | knife = *naif* |
| Stress | **bold** | today = *tuh-**DEI*** |
| Linked sounds | underscore | did you = *di_yu* |
| Reduced/swallowed | apostrophe | going to = *gon'* |
| Stretched vowel (rap/song) | repeated letter | ain't = *AAAINT* |

### 4. Flag pronunciation traps per chunk

After the transcription of each chunk, add a **Pronunciation Notes** section flagging:

- **Reductions** — words that collapse in natural speech (*because → 'cause*, *going to → gonna*, *want to → wanna*)
- **Flap T** — any *t* that softens to *d*
- **Schwa** — any stressed vowel that reduces in fast speech
- **Linked sounds** — words that blend together
- **Artist choices** — stretched vowels, dropped consonants, rhythm-driven changes
- **False friends** — words that look like Spanish but sound different

Keep notes brief — 2-4 bullets per chunk max. Don't over-explain.

### 5. Chunk flow

```markdown
## Verse {N} — Lines {X}-{Y}

"{line 1}"
*(transcription)*

"{line 2}"
*(transcription)*

...

**Pronunciation notes:**
- {note 1}
- {note 2}

Type **"next"** to continue, or ask about any word.
```

### 6. Learner questions mid-song

If the learner asks about a specific word or line — answer immediately, then offer to continue.

### 7. End of song summary

```markdown
## Song Complete — {Title} by {Artist}

### Sounds you trained today
- {rule 1}: {examples from the song}
- {rule 2}: {examples from the song}

### Hardest lines in this song
- "{line}" → {why it's hard}

### Artist patterns
{1-2 lines on how this artist specifically bends pronunciation — Eminem's reductions, etc.}

### Vocabulary from the song
{3-5 words/phrases worth saving to spaced repetition}

**Save vocabulary to review queue?** Type "yes" or "no".
```

### 8. Update databases

Use `fluent-db-updater`:

- `command_used: "/fluent-songs"`, `skills_practiced: ["speaking", "vocabulary"]`
- `skill_scores.speaking: {exercises: chunk_count, correct: chunk_count, time_minutes}`
- `new_vocabulary[]` — if learner said yes to saving
- `focus_next_session[]` — hardest sounds from the song
- `session_notes` — song title, artist, main patterns covered

Save to `/results/fluent-songs-session-{NNN}.md`.

## Artist-Specific Notes

### Eminem
- Extremely fast — reductions are aggressive
- Stretches vowels for emphasis and rhythm: *ain't → AAAINT*, *I → AAAI*
- Drops final consonants constantly: *and → an'*, *just → jus'*
- Rhymes sometimes distort pronunciation intentionally
- Great for training ear on American English at maximum speed

### General pop/rock (Bon Jovi, etc.)
- Clearer than rap — vowels more sustained
- Still uses reductions: *'cause, gonna, wanna, gotta*
- Stress patterns follow melody — sometimes different from spoken stress

### R&B / soul
- Melisma (one syllable stretched over many notes) — not useful for pronunciation training but worth noting
- Strong schwa reduction

Add notes for other artists as the learner requests them.

## Critical Rules

- **Never dump the full song at once.** Chunks of 4-6 lines, wait for "next".
- **Never use IPA.** Spanish figurative only.
- **Flag artist distortions separately** from standard pronunciation — the learner needs to know what's "correct" vs what's a stylistic choice.
- **Vocabulary is opt-in.** Don't force-add every word — ask at the end.
- **Never auto-invoke.** Gated; must fire only on explicit `/fluent-songs`.
- **Song lyrics are copyrighted** — present them for educational/language-learning purposes only, line by line in context.
