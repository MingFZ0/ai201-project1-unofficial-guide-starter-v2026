# The Unofficial Guide

Mingfeng Zhong
Corpus: campus_life
<!-- Replace this line with your name and which corpus you picked. -->

> **This file is your submission.** Fill it in as you go — most sections get
> written during the milestone that produces them, not at the end.
>
> How the starter works, and every command you'll need, is in `RUNNING.md`.
> Leave that file alone.
>
> **Paste everything as text.** No screenshots, no video. A typed table gets
> full credit; a picture of the same table gets none.
>
> Delete these instruction blocks as you replace them. The `<!-- -->` comments
> are notes to you and don't show up when the page renders — you can leave them
> or remove them.

---

# Unit 1

## What This Does

<!-- Three or four sentences. Which corpus you picked, and the kinds of
     questions your system answers. Write it for someone who has never seen
     this repo.

     Milestone 5. -->

## Chunking Strategy

**Chunk size:**

For the corpus that I picked, which is campus_life, I looked at the average size of the documents. The documents themselves proved to be too big to be cut down when the chunk size is 800. So therefore, I went with a number that match the documents more closely, with a chunk size of 317 which is the average sizes of the documents.

**Overlap:**

I think the current overlap worked fine, and I did not see any issues. So therefore, I want to test how it will come out and I did not change the overlap and kept it at 120 characters.

## Sample Chunks

<!-- Five chunks, pasted as text. Label each one and name the file it came from
     AND the function that produced it — the grader checks your code against
     what you claim here.

     `python app.py chunks -n 5` prints all three for you. Copy them straight
     across.

     Milestone 3. -->

**Chunk 1** — source: `admin_add_drop_deadline.txt#0` — produced by: `chunker.py::split_documents`

```
On the add/drop deadline

You can add a course through the end of the second week. Dropping is a longer window — through the end of week six — but a drop after week two shows as a W on your transcript. Nothing anywhere on the registrar's site says this plainly, and students find out from each other.
```

**Chunk 2** — source: `course_biol_160_workload.txt#0` — produced by: `chunker.py::split_documents`

```
Workload for BIOL 160 Cell Biology

People keep asking so: 9 to 11 hours a week, the heaviest first-year course by reputation. That's real time, not optimistic time.

It's front-loaded — the first month is heavier than the rest, partly because you're learning the format.
```

**Chunk 3** — source: `course_phys_130.txt#0` — produced by: `chunker.py::split_documents`

```
PHYS 130 Mechanics

Just finished a year in this building. Format is lecture with a compulsory lab that meets fortnightly. Assessment: three midterms, no final, plus a lab practical. Not curved, but the lowest midterm is dropped.

Expect 7 hours a week, plus 3 on lab weeks.

The one piece of advice: the lab practical is worth 20% and almost nobody prepares for it.
```

**Chunk 4** — source: `dining_verrill_street_grill_followup.txt#1 ` — produced by: `chunker.py::split_documents`

```
Also worth saying: one register, so the queue is a single line no matter how busy. Nobody tells you this at orientation.
```

**Chunk 5** — source: `housing_morrow_house.txt#0` — produced by: `chunker.py::split_documents`

```
Morrow House — what it's actually like

Just finished a year in this building. Built 1954, partially renovated 2008. Rooms are singles and doubles, hall bathrooms.

The good: cheapest housing tier by about $900 a year, and the singles are real singles.

The bad: known damp problem on the ground floor; two rooms were taken offline in 2024.

Laundry costs $1.50 wash, $1.25 dry, coin or card. On noise: loud until about 1am on weekends, no enforced quiet hours.
```

## Sample Answer

**Question:**

How long does it take to walk across campus?

**Answer:**

```
(best distance 0.346, cutoff 0.7, top-k 3)

Based on the provided documents, walking times vary by location: Aldridge
Hall to the science quad takes 4 minutes, Fenwick Court to central campus
takes 18 minutes, Morrow House to Kestrel Commons takes 7 minutes, and the
Library to Ridgeway Café takes 3 minutes. Additionally, you should add four
minutes to these times in the winter because the path past the pond ices
over (transit_walking.txt).

Sources retrieved: health_center.txt, transit_shuttle.txt, transit_walking.txt
```

I lowered top-k from the default 5 to 3 after noticing that k=5 pulled in two
clearly irrelevant chunks (a dining follow-up post and an on-campus-jobs post)
alongside the three genuinely related ones. At k=3, the answer stayed
identical and used fewer tokens (500 vs. 711), though one loosely related
chunk (health_center.txt — matched on the word "walk," not the topic) still
made the cut.

**My relevance cutoff:** 0.7

**Relevance cutoff:** 0.7

**In-scope questions (best distance for each):**
| Question | Best distance |
|---|---|
| How long does it take to walk across campus? | 0.346 |
| What time does the campus open for students? | 0.481 |
| What kind of food is offered on campus? | 0.496 |
| How much financial aid should a student expect to get? | 0.577 |
| What do students say about the difficulties of the classes? | 0.587 |

**Out-of-scope questions (best distance for each):**
| Question | Best distance |
|---|---|
| What is the capital of Mongolia? | 0.825 |
| What is the recommended dosage of ibuprofen for a headache? | 0.844 |
| Who won the 1994 World Cup? | 0.886 |
| How do I write a for loop in Rust? | 0.896 |
| How do I change the oil in a diesel engine? | 0.934 |

The two groups separate cleanly, with a gap between 0.587 and 0.825. I set the
cutoff at 0.7 — roughly the midpoint of that gap — rather than at the
starter's default of 0.6, since 0.6 left almost no margin above my highest
in-scope distance (0.587).

## How I Used AI

<!-- Two specific moments. For each: what you asked for, what came back, and
     what you changed about it.

     "I asked Claude to write the chunking function from my notes. It ignored
     the overlap, so I added that myself" is the level of detail we're after.
     "I used AI to help me code" is not.

     Milestone 5. -->

**1.**

**2.**

<!-- ── Stretch features ─────────────────────────────────────────────────────
     Doing one? Say so here BEFORE you start. A feature this README never
     claims earns nothing.
     ───────────────────────────────────────────────────────────────────────── -->

---

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

<!-- Your five criteria, three runs each. `python run_eval.py --label before`
     runs the questions, puts the OUT_OF_SCOPE ones through the gate, and
     writes it all into results/ for you. Targets come from criteria.md; the
     verdict column is your call.

     Criterion 3 is measured in one deterministic pass rather than three, so
     the same number goes in all three run columns. That's correct, not lazy.

     Milestone 1. -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

<!-- Underneath, paste the REAL output for each criterion from one of your
     runs — the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 |  |  |  |
| 2 |  |  |  |
| 3 |  |  |  |
| 4 |  |  |  |
| 5 |  |  |  |

## Diagnoses

<!-- For each miss: which stage caused it, and how. The stage alone isn't
     enough — you need the mechanism.

     Not a diagnosis: "Question 3 didn't work."
     A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                       one sentence that got split across two chunks, so
                       neither chunk on its own contains it."

     The five stages: loading → chunking → embedding → retrieval → generation.

     Look for a pattern. If three misses all ask about numbers, that's one
     problem, not three.

     Missed nothing? Say so, then say honestly whether your targets were set
     low, and which one you'd tighten and to what.

     Milestone 3. -->

## The Improvement

**What I changed:**

**Why I picked it:**

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. | | | | | |
| 5. | | | | | |

**Did it help?**

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->

## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
