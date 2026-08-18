---
name: peppa-transcript-fixer
description: Fix Peppa Pig transcript errors, split into complete sentences, gloss every word, analyze sentence structure and grammar, add Chinese explanations, then update the website.
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

4. Analyze each sentence for study.
   - For each sentence, provide:
     - the corrected German sentence
     - a natural Chinese translation
     - **词汇**：句子里每一个词的含义，按出现顺序，一个不漏
     - **结构**：一句拆出主干（主语 / 谓语 / 宾语或补语），标出从句、省略、插入语
     - **语法**：1-3 个具体语法点（时态、语序、格、可分动词、情态动词、不定式结构、固定搭配等）
   - Gloss the word as it appears in this sentence, including names, articles, pronouns, particles, and separable prefixes.
   - Give the meaning in this sentence, not a dictionary dump. Add a short grammatical hint only when it helps (case, person, separable prefix).
   - Name the grammar pattern, then explain what it does in this sentence. Example: `werden + Infinitiv` = 将来时.
   - Skip generic filler. Every note should teach something the learner can reuse.
   Completion criterion: every sentence has German, Chinese, a gloss for every word, structure breakdown, and at least one grammar point.

5. Update the website.
   - Add the new sentences to `index.html` in the same visual structure already used by the site:
     - one `<article class="line">` per sentence
     - German in `<p class="de">`
     - Chinese in `<p class="zh">`
     - every word in `<ul class="words">`, one `<li>` per word: `<strong>Wort</strong> = 含义`
     - structure and grammar in `<ul class="tips">`, leading with **结构** and **语法** labels
   - Preserve sentence order.
   - Do not add numbering.
   - Keep wording on the page aligned with the corrected transcript you just produced.
   Completion criterion: the new learning content is present in `index.html` and matches the corrected sentence list, including a gloss for every word plus structure and grammar.

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
- **词汇**：Wort = 含义；Wort = 含义
- **结构**：主语 + 谓语 + …
- **语法**：pattern = explanation
```

When the user asks to update the site, make the edit directly and summarize what was added or corrected.

## Word Gloss

- Gloss every spoken word in order. Do not skip function words (`und`, `es`, `zu`, `doch`, `nicht`).
- Use the form in the sentence (`kaufen`, `ein`, `mir`, `die`), not a rewritten lemma unless the lemma is needed to explain a split verb.
- Names: `Peppa` = 佩奇（人名）.
- If two words form one unit in this sentence, still list each word, then mention the unit in **语法**.

## Grammar Focus

Prioritize patterns that recur in Peppa Pig dialogue:

- word order (V2, verb-final in subordinate clauses, inverted questions)
- cases (Nominativ, Akkusativ, Dativ) and preposition + case
- separable verbs (`ein|kaufen`, `an|fangen`)
- modal verbs and questions (`kann ich …?`, `darf ich …?`)
- `es` as placeholder (`lieben es, … zu …`)
- omitted repeated material (`Peppa eigentlich auch.`)
- imperatives (`Leg …!`, `Hilf …!`)
- fixed phrases (`gut gemacht`, `hier entlang`, `am liebsten`)
