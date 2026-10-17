# Midterm 1: answers

GitHub username: amantayeva2006
Session code: ember-92
Fingerprint: 33a7a550

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration

I used one AI assistant, Claude (made by Anthropic), for this midterm and for my practice before it. I did not use any other AI tool.

**What Claude did.** I gave Claude the course files and my data (DATA.md, retrieval_ranks.csv, doc_results.csv). The data comes from my GitHub username and the session code. Claude calculated the numbers and solved the tasks. It wrote a short Python script (solve_midterm1.py) that calculates recall, MRR, the Wilson intervals, the yearly costs, and the answers to the five questions in Part 3. Then Claude put the numbers into the tables in ANSWERS.md. It also wrote the first version of my decision (1d), next measurement (1e), memo (2b) and the "Why" lines.

**What I did.** I ran the data generator in the Colab notebook with the session code. I checked that my fingerprint (33a7a550) is the same as the one Claude used. I read all the answers and checked the numbers with my data and the formulas in the README. I checked the word limits. I put the answers in my fork and sent the commit myself. Before the exam, I practiced with the PRACTICE code.

**Responsibility.** I am responsible for everything in this file. In the oral check I will explain every number and every sentence. You can repeat the calculations by running solve_midterm1.py on the files in my_data/.

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A | 0.650 | 0.833 | 0.900 | 0.742 |
| B | 0.217 | 0.450 | 0.583 | 0.352 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 | 0.833 | 0.720 | 0.907 |
| A, k=5 | 0.900 | 0.799 | 0.953 |
| B, k=3 | 0.450 | 0.331 | 0.575 |
| B, k=5 | 0.583 | 0.457 | 0.699 |

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 34415.75 |
| A, k=5 | 46625.61 |
| B, k=3 | 20604.09 |
| B, k=5 | 23606.17 |

### 1d. Decision (at most 150 words)

Chosen candidate: A, k=3

A, k=5 has the highest recall (90.0 %) but costs $46,626, above the $40,500 budget, so it is out. Of the three affordable candidates, A3 costs $34,416 and its interval (72.0–90.7 %) lies entirely above B5 (45.7–69.9 %) and B3 (33.1–57.5 %), so A3 is shown to be better than both B candidates, even though they are cheaper.

Recall of A3 per language (20 questions each): EN 80 % (16/20), RU 80 % (16/20), KK 90 % (18/20). English and Russian users are served worst; the per-language intervals are wide (EN and RU 58–92 %), so I do not claim KK is truly better.

### 1e. Next measurement (at most 60 words)

Collect at least 100 labelled real questions per language and rerun A,3. This shows whether English and Russian recall (80 %, interval 58–92 % on 20 questions) is really below Kazakh, and whether it meets an agreed minimum. If EN or RU stays near 80 %, I would test A,4, whose cost is just over budget.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | 0.844 | KK, degraded | 0.667 | 0.488 | 0.808 | 51240.53 |
| L | 0.811 | KK, degraded | 0.700 | 0.521 | 0.833 | 5400 |

Personal-data pages per year sent abroad under H: 281050

### 2b. Memo (at most 300 words)

To: Head of Operations. Recommendation: pipeline L, with human review.

**Who is served worst.** Overall, H scores 84.4 % (95 % interval 78.4–89.0) and L 81.1 % (74.8–86.2). The intervals overlap, so the difference is not shown. Both pipelines serve the same group worst: degraded Kazakh scans, which H gets right 66.7 % of the time (20/30, interval 48.8–80.8 %) and L 70.0 % (21/30, 52.1–83.3 %). H's overall figure hides this: its clean English scans reach 96.7 %. With 30 invoices per cell, we are not sure of the exact figures.

**Personal data.** In 11 % of 7,000 daily pages a name and ИИН are printed: 281,050 pages a year. Three questions of Lecture 5.1:
- H: the scan goes to a data centre abroad (known). Who stores it, and for how long, I do not know; we must get it in writing. A vendor's statement is not a compliance review, and Law No. 94-V covers the ИИН.
- L: the scan stays on our server in Kazakhstan (known). We store it and we set the retention.

**Safeguards anyone can verify.** Delete page images from the L server after 30 days (audit the log). Restrict server access to named staff. Validate every extracted ИИН (12 digits, check digit) and the total; send failures and all degraded Kazakh scans to human review. Re-test monthly on at least 100 degraded Kazakh invoices against an agreed minimum.

**Choice.** L costs $5,400 a year against $51,241 for H, a saving of $45,841, with no proven loss in accuracy, and the personal data stay in the country. Remaining risk: about 30 % errors on degraded Kazakh scans, caught only by costly staff review; and my figures rest on 30 invoices per cell.

## Part 3: Explain your own numbers

### Q1

Number: 1.43
Why (at most 40 words): Kazakh costs 2.13x the bytes of English, so UTF-8 encoding explains part of the premium. Tokens are 3.05x, so the tokenizer adds a further 43 %: it splits Kazakh worse than the encoding alone requires.

### Q2

Number: 473
Why (at most 40 words): The chunk is about 526 e5 tokens and e5 reads 512. The sentence starts at 473 and is under 20 tokens, so it ends before 493 and is read. Retrieval can work; only the last 14 tokens are lost silently.

### Q3

Number: 71.2
Why (at most 40 words): Temperature does not change the ranking. Lower T sharpens the distribution (top token 49.8 % at T=1, 71.2 % at T=0.5). Every token keeps non-zero probability, so the sampleable set is unchanged without top-p or top-k.

### Q4

Number: 34.1
Why (at most 40 words): I kept account numbers and birthdays, arbitrary facts that cannot be derived. Multiplication and arithmetic sequences follow rules, so the singleton bound does not apply. A "never invent" prompt adds no knowledge and does not change the bound.

### Q5

Number: 39192
Why (at most 40 words): The first message is paid for 23 times by the last turn. Prompt caching lowers the price of the repeated prefix, but the tokens are still resent every turn.
