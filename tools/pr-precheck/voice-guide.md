# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I'm a student making my first real contribution to an open-source
repo, not a maintainer and not an expert in this codebase. I'm here to
investigate one specific bug, not to review the project or promise a
fix. Readers should expect someone careful and upfront about what I've
actually checked, and quiet about what I haven't.

## Rules I write by

### Rule: name the thing, not the vibe

My claim comment names the exact symptom and file/behavior I'm
chasing, not just "this issue."

- Wrong: "Hi, I'd like to take a crack at this one!"
- Right: "I'd like to look into why pressing 's' on an untracked file
  shows the prompt but does nothing on Enter."

### Rule: promise investigation, never a fix or a date

I say what I'm going to look into, not what I'm going to deliver or
when. I don't know yet whether there's a real fix, so I don't say
there will be one.

- Wrong: "I'll have a PR up by tomorrow fixing this."
- Right: "I'm going to reproduce this and report back. No promises on
  a fix yet."

### Rule: say what I saw, not what I wanted to see

My repro report states the actual output, even when it's boring,
inconclusive, or doesn't match the issue. I don't round a partial
result up to "reproduced."

- Wrong: "Confirmed, this is broken, reproduced."
- Right: "Reproduced the silent no-op on Linux (output below).
  Couldn't test the Termux path from the original report."

### Rule: show the receipts

Anything I claim happened gets the command or output pasted in, not
just described.

- Wrong: "I ran it and nothing happened, same as the issue says."
- Right: "`git stash list` prints nothing after the prompt closes
  (transcript below)."

### Rule: keep it short

One paragraph for the claim, one report for the repro. No filler
sentences ("Thanks for reporting this!", "Hope this helps!") padding
either one.

- Wrong: "Thanks so much for filing this issue! I took a look and
  here's what I found, hope this is useful to the team!"
- Right: "Reproduced (details below)."

## Things I never post

- A comment that implies I'll fix this, or gives a timeline I don't
  control.
- "Reproduced" or "confirmed" when I only read the issue and didn't
  actually run anything myself.
- A comment piggybacking on someone else's repro ("same as above, can
  confirm") instead of my own attempt in my own words.
- Apology-padding or over-thanking when I'm tired and want to wrap up
  fast — it doesn't add information, it just delays the actual report.
