# Midterm 1: Retrieval, Documents, and Your Own Numbers

CSS4007 Artificial Intelligence · Narxoz University · Fall 2026-27 · Midterm 1 (35 points of ISC 1)

An in-class, 80-minute, open-AI midterm. You get **your own data set** (generated from your GitHub
username and a code announced in class), decide what a small bank should do with it, and explain
your numbers. Every company, document and measurement in this exam is invented.

| Part | Points | Suggested time | What it assesses |
|---|---|---|---|
| 1. Retrieval and cost decision | 15 | 35 min | ABET SO1, indicator 1.3: apply computing principles with justification |
| 2. A document pipeline you can defend | 10 | 25 min | ABET SO4, indicator 4.2: ethical and legal considerations in a design decision |
| 3. Explain your own numbers | 10 | 20 min | Course outcomes LO-01 to LO-04 (lectures 1.1 to 4.1, labs 1 to 2) |

After the written part, every student has a **3-minute oral check** (see below). It can lower a part's
score; it cannot raise it.

## Rules

- **AI is allowed**, any assistant, any tool (Python, spreadsheet, notebook). Computing is not the
  point; deciding and explaining is.
- **Declare it.** `ANSWERS.md` starts with an AI-use declaration (about half a page: what you used,
  and for what). A missing or knowingly incomplete declaration is a violation of the course AI policy.
- **Work alone.** Your numbers are yours alone, so a neighbour's table will not help you. Copying
  another student's text or sharing answers is an academic-honesty violation.
- **Everything you submit you must be able to explain.** The oral check is about your own answers.
- Time is limited: 80 minutes from the moment the code is announced. Your commit time is checked.

## Before the exam (rehearse, 15 minutes)

1. **Fork** this repository on GitHub (top right, *Fork*).
2. Run the generator once with the practice code, to check that your setup works:

   ```bash
   git clone https://github.com/YOUR_USERNAME/midterm-1-AI-course.git
   cd midterm-1-AI-course
   python make_my_data.py --github YOUR_USERNAME --code PRACTICE
   ```

   You need Python 3.9 or newer and nothing else (standard library only). Open `my_data/DATA.md`.
   The practice data has the **same structure** as the exam data but different numbers.
3. No git or Python on your machine? Use the Colab notebook instead:
   [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/unreal-kz/midterm-1-AI-course/blob/main/midterm1.ipynb)

## During the exam

1. The **session code** is written on the board at the start. Run:

   ```bash
   python make_my_data.py --github YOUR_USERNAME --code THE_CODE
   ```

   `YOUR_USERNAME` is the GitHub account that owns your fork (upper or lower case does not matter).
   You get `my_data/DATA.md`, `my_data/retrieval_ranks.csv` and `my_data/doc_results.csv`.
2. Copy the **fingerprint** printed in `DATA.md` (and the username and code) into the first lines of
   `ANSWERS.md`. If the fingerprint does not match, your data is not the exam data.
3. Work through Parts 1, 2, 3. Write **only in `ANSWERS.md`**; it is the only file that is graded.
4. Commit and push `ANSWERS.md` to your fork:

   ```bash
   git add ANSWERS.md
   git commit -m "Midterm 1 answers"
   git push
   ```

   *Without git:* open `ANSWERS.md` in your fork on github.com, click the pencil icon, edit, then
   *Commit changes*.
5. **Submit the commit URL in Canvas**, not the repository URL. On your fork, click the latest commit;
   the address looks like `https://github.com/YOUR_USERNAME/midterm-1-AI-course/commit/3fa9c1e...`.
   The commit fixes exactly what you wrote; later pushes do not change what is graded.

## Part 1: Retrieval and cost decision (15 points)

A bank's support assistant looks up chunks of its knowledge base and pastes them into the prompt before
answering (Lab 03). Two chunkings are being compared: **config A** (paragraph chunks, about 256
cl100k tokens each) and **config B** (sentence-window chunks, about 64). The assistant retrieves the
top **k** chunks, with k = 3 or k = 5, so there are four candidates: (A, 3), (A, 5), (B, 3), (B, 5).
`retrieval_ranks.csv` gives, for 60 test questions, the rank at which each config retrieved the correct
chunk. `DATA.md` gives the traffic, token counts, prices and annual budget.

- **1a.** recall@1, recall@3, recall@5 and MRR@10 for config A and for config B.
- **1b.** For each candidate, its recall@k and the 95 % Wilson interval.
- **1c.** The annual model cost of each candidate.
- **1d.** Your recommended candidate (also on the `Chosen candidate:` line) and a decision of at most 150 words.
  It has to respect the budget, to use your intervals, and to report the recall of your choice **for
  each language separately**.
- **1e.** One concrete next measurement that would settle what your data cannot (at most 60 words).

| Criterion | Points |
|---|---|
| Metrics (1a) correct | 4 |
| Intervals (1b) correct | 2 |
| Reading of the intervals: no winner claimed where intervals overlap, a winner where they do not | 1 |
| Cost model (1c) correct | 3 |
| Decision (1d): within budget, consistent with the numbers, per-language recall of the choice, who is served worst | 4 |
| Next measurement (1e): concrete and able to change the decision | 1 |

## Part 2: A document pipeline you can defend (10 points)

The bank also wants to read scanned invoices automatically (Lab 04). Pipeline H is a hosted API in a
data centre abroad; pipeline L is an open model on the bank's own server in Kazakhstan.
`doc_results.csv` gives each pipeline's results by language and scan condition. In part of the scans an
individual's name and **ИИН** are printed. `DATA.md` gives volumes, prices and server costs.

- **2a.** Fill in the table in `ANSWERS.md`: overall accuracy of each pipeline, its weakest
  (language, condition) cell with accuracy and 95 % Wilson interval, and annual cost. Add the number
  of scanned pages per year that carry an individual's data and would leave the country under H.
- **2b.** A **memo of at most 300 words** to the head of operations, who must pick one pipeline. It has to
  cover: who each pipeline serves worst and how sure you are; what the personal-data situation means
  for the design (use the three questions of Lecture 5.1: where does the scan go, who stores it, for how
  long; say what you know and what you would have to find out); safeguards that someone could verify;
  your choice and the risk that remains.

This is not legal advice and the exam asks for no legal facts beyond Lecture 5.1: Kazakhstan's law on
personal data (No. 94-V) covers the ИИН, and a vendor's statement is not a compliance review.

| Criterion | Points |
|---|---|
| Impact with evidence: names the worst-served group with its interval, and notes where an overall figure hides it | 3 |
| The legal constraint is built into the design: the three questions are applied to H and to L | 3 |
| Safeguards that are specific and checkable | 2 |
| A weighed choice (accuracy, cost, risk) with the residual risk named | 2 |

## Part 3: Explain your own numbers (10 points)

`DATA.md` contains five short questions, one from each of: the language premium, chunk size and the
embedding window, temperature and top-p, why models hallucinate, and the cost of conversations or
training. Each uses **your** numbers. For each: a **Number** line (1 point) and a **Why** line of at
most 40 words (1 point).

## The oral check (3 minutes, every student)

Two questions about your own answers, from any part. If you cannot explain something you wrote, that part
is re-scored to what you could explain, down to zero for that part. The ABET level of an outcome is
recorded from the score that survives the oral check.

## Formulas

- **recall@k** = (number of questions whose correct chunk has rank ≤ k) ÷ 60.
  **MRR@10** = (1 ÷ 60) × the sum of 1 ÷ rank over questions with rank ≤ 10 (rank 11 adds nothing).
- **95 % Wilson interval** for hits out of n, with p = hits ÷ n and z = 1.96:
  centre = (p + z² ÷ 2n) ÷ (1 + z² ÷ n),
  half-width = z × √(p(1 − p) ÷ n + z² ÷ 4n²) ÷ (1 + z² ÷ n); interval = centre ± half-width.
  As in the course, two proportions whose intervals overlap are **not shown to differ**.
- **Cost of one request in language ℓ** = (input tokens × price_in + output tokens × price_out) ÷ 1,000,000,
  where input tokens = system prompt + question_ℓ + k × chunk_ℓ(config), and output tokens = answer_ℓ.
  **Annual cost** = Σ over languages of (traffic share_ℓ × cost_ℓ) × requests per day × 365.
- **Pipeline H, one page**: image tokens = ⌈width ÷ 28⌉ × ⌈height ÷ 28⌉.
  Annual cost = pages per day × 365 × (image tokens × price_in + output tokens × price_out) ÷ 1,000,000.
  **Pipeline L**: annual cost = ⌈pages per day ÷ server capacity⌉ × monthly cost × 12.
- **Pages with personal data per year** = pages per day × 365 × share of scans with personal data.

## Files

- `make_my_data.py`: your data generator (standard library only).
- `ANSWERS.md`: the only graded file.
- `midterm1.ipynb`: Colab notebook for running the generator without installing anything.
- `my_data/`: created by the generator, not committed.
