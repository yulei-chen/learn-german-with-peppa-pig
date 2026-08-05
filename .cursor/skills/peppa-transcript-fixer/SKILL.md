---
name: peppa-transcript-fixer
description: Fix Peppa Pig transcript errors, split into complete sentences, add Chinese explanations and German learning notes, then update the website.
disable-model-invocation: true
---

# Peppa Transcript Fixer

Use this skill when the user pastes a Peppa Pig transcript and wants it corrected, broken into sentences, explained for German study, and added to this website.

## Workflow

1. Gather the source.
   - Use the pasted transcript as the source of truth unless the user also provides a screenshot or a cleaner subtitle version.
   - Assume YouTube auto captions may contain recognition errors, merged sentences, missing punctuation, repeated fragments, and wrong word boundaries.
   Completion criterion: you have the raw transcript span to process.

2. Repair the transcript before explaining it.
   - Normalize obvious OCR/ASR noise such as timestamps, duplicated words caused by caption overlap, and broken casing.
   - Restore likely German wording conservatively. Prefer minimal correction over invention.
   - If a word or clause is genuinely ambiguous, say so and give the most likely reconstruction.
   - Do not drop content just because it looks messy. Every spoken part should either be preserved, merged into the right sentence, or called out as uncertain.
   Completion criterion: every usable fragment has been accounted for.

3. Split into sentences with no omissions.
   - Produce complete spoken units in order.
   - Merge fragments when they clearly belong to one sentence.
   - Keep short standalone utterances like `Ja, Mama.` or `Hier entlang.` as their own sentences.
   - Do not number the sentences when updating the website.
   Completion criterion: the full passage is represented as an ordered sentence list with nothing missing.

4. Explain each sentence for study.
   - For each sentence, provide:
     - the corrected German sentence
     - a natural Chinese translation
     - 1-3 short learning notes focused on useful vocabulary, grammar, or idiom
   - Keep notes practical. Prefer items like separable verbs, case usage, fixed phrases, word order, and omitted repeated material.
   - Avoid filler analysis that only restates the translation.
   Completion criterion: every sentence has German, Chinese, and concise study notes.

5. Update the website.
   - Add the new sentences to `index.html` in the same visual structure already used by the site:
     - one `<article class="line">` per sentence
     - German in `<p class="de">`
     - Chinese in `<p class="zh">`
     - notes in `<ul class="tips">` when there are useful points
   - Preserve sentence order.
   - Do not add numbering.
   - Keep wording on the page aligned with the corrected transcript you just produced.
   Completion criterion: the new learning content is present in `index.html` and matches the corrected sentence list.

6. Verify the update.
   - Read the edited section back or inspect the diff.
   - Check that no sentence from the processed span was skipped.
   - Check that any correction previously confirmed by the user stays intact.
   Completion criterion: the page content, transcript correction, and explanation set are consistent.

## Rules

- Be conservative with corrections. Fix likely caption mistakes, not style.
- Preserve the episode's spoken order.
- Prefer one strong note over several weak ones.
- If the user corrects a word, treat that correction as authoritative.
- If uncertainty remains, mention it in chat, but still update the page with the best current reconstruction unless the user asks to wait.

## Output Shape In Chat

When the user asks for explanation before updating the site, use this per-sentence shape:

```markdown
**German sentence.**
中文翻译。
- **point** = explanation
```

When the user asks to update the site, make the edit directly and summarize what was added or corrected.
