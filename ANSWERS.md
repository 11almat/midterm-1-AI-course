# Midterm 1: answers

GitHub username: 11almat
Session code: ember-92
Fingerprint: 169e80cb

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration

I used ChatGPT (GPT-6) to help run the provided deterministic Python data generator, calculate retrieval metrics, Wilson confidence intervals, annual costs, and numerical answers, and draft and check the English explanations. I used Python standard-library calculations and the formulas in the assignment. The generated data are synthetic and belong to my username and session code. AI helped with calculations and wording; it did not provide real bank data or legal compliance verification. I will review the decision logic, including budget limits, weaker language groups, and personal-data risks, before submission. I understand that I must be able to explain the formulas, assumptions, choices, and limitations in the oral check.

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| Config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---:|---:|---:|---:|
| A | 0.6167 (37/60) | 0.8500 (51/60) | 0.9333 (56/60) | 0.7351 |
| B | 0.2833 (17/60) | 0.4167 (25/60) | 0.6000 (36/60) | 0.3839 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)



### 1c. Annual model cost in USD

| Candidate | Hits/60 | recall@k | 95% Wilson interval | Annual model cost | Budget |
|---|---:|---:|---:|---:|---|
| (A, 3) | 51/60 | 85.00% | [73.89%, 91.90%] | $19,376.78 | Within |
| (A, 5) | 56/60 | 93.33% | [84.07%, 97.38%] | $25,228.05 | Over |
| (B, 3) | 25/60 | 41.67% | [30.06%, 54.27%] | $12,638.83 | Within |
| (B, 5) | 36/60 | 60.00% | [47.37%, 71.43%] | $13,998.14 | Within |

### 1d. Decision (at most 150 words)

Chosen candidate: (A, 3)
I recommend A with k=3. It retrieves the correct chunk for 51/60 questions (85.0%; 95% Wilson interval 73.9%–91.9%) and costs $19,376.78 annually, below the $22,300 budget. A with k=5 reaches 93.3% but costs $25,228.05 and is unaffordable. B with k=5 reaches only 60.0% (47.4%–71.4%). Its interval does not overlap A,k=3 (73.9%–91.9%), supporting A,k=3 as the better retrieval choice among affordable candidates. A,k=3 recall by language is EN 18/20 = 90%, RU 17/20 = 85%, and KK 16/20 = 80%. Kazakh users are served worst, although these small per-language samples do not establish a statistically certain language difference. A,k=3 leaves about $2,923.22 of the annual model budget unused.

(Decision, using your intervals and the budget. Give the recall of your choice for EN, RU and KK separately.)

### 1e. Next measurement (at most 60 words)
Run a larger, stratified test with at least 200 new questions per language, especially Kazakh, and compare end-to-end answer correctness for A,k=3 and B,k=5. Record retrieval success, harmful wrong answers, and latency. This could change the choice if cheaper B performs comparably on actual answers.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| Pipeline | Overall exact accuracy | Weakest (language, scan condition) | Weak-cell accuracy | Weak-cell 95% Wilson interval | Annual cost |
|---|---:|---|---:|---:|---:|
| H | 155/180 = 86.11% | RU, degraded | 21/30 = 70.00% | [52.12%, 83.34%] | $8,802.89 |
| L | 143/180 = 79.44% | RU, degraded **and** KK, degraded (tie) | 21/30 = 70.00% each | [52.12%, 83.34%] each | $14,400.00 |

Personal-data pages per year sent abroad under H:
Annual volume = 5,500 × 365 = 2,007,500 pages. Pages containing names and ИИН = 2,007,500 × 19% = 381,425 pages/year. Under H, these pages would be transmitted abroad unless the design changes.
H image tokens/page = ceil(1240/28) × ceil(1754/28) = 45 × 63 = 2,835. H annual cost = 2,007,500 × (2,835 × $1 + 310 × $5)/1,000,000 = $8,802.89. L needs ceil(5,500/4,000) = 2 servers, costing 2 × $600 × 12 = $14,400/year.

### 2b. Memo (at most 300 words)
To: Head of Operations
Subject: Invoice extraction pipeline choice
I recommend pipeline L on the bank’s servers in Kazakhstan, with a human review process for uncertain results. H is more accurate overall: 155/180 = 86.1%, compared with 143/180 = 79.4% for L. H is also cheaper ($8,802.89 versus $14,400 annually). However, the overall scores hide weaker groups. H performs worst on degraded Russian scans (21/30 = 70.0%; Wilson 95% CI 52.1%–83.3%). L is weakest on degraded Russian and Kazakh scans (both 21/30 = 70.0%, same interval). These intervals are wide, so we need more testing before claiming a reliable difference between weak groups.
H sends scanned pages to a foreign data centre. At the given volume, 381,425 pages annually contain names and ИИН. Kazakhstan’s personal-data law No. 94-V covers ИИН. We must establish where the vendor sends data, who stores images, logs and backups, and for how long; the scenario does not specify storage, retention or subprocessors. A vendor assurance alone is not a compliance review.
For L, scans stay on the bank’s Kazakhstan server by design, but we must verify backup locations, operator access, logs and deletion periods. Require access controls, encryption, documented retention and deletion, audit logs, monthly sampling by language and scan quality, and manual review of low-confidence or degraded scans. Verify these controls with tests and audit records.
L reduces international transfer risk but has lower extraction accuracy and higher cost. The remaining risk is incorrect invoice fields, especially degraded Russian and Kazakh scans, so human verification and continued measurement are necessary.

## Part 3: Explain your own numbers

Q1. Language premium
Number: 2.02 (bill ratio KK/EN). Total-token ratio = (2,894 + 200)/(1,279 + 130) = 3,094/1,409 = 2.20. EN bill = (1,279×2 + 130×10)/1,000,000 = $0.003858; KK bill = (2,894×2 + 200×10)/1,000,000 = $0.007788; ratio = 2.0187.
Why: Input and output tokens have different prices. Kazakh uses more input tokens, but output tokens are five times more expensive. Therefore, the weighted bill ratio differs from the simple total-token ratio.
Q2. Chunk size and embedding window
Number: 13.5%. English e5 length = 576 × 263/256 = 591.75 tokens; invisible share = (591.75 − 512)/591.75 × 100 = 13.5%.
Why: Only English is cut: EN ≈592 e5 tokens, RU =288, KK =171. The embedding sees only the first 512 English e5 tokens. The remaining text cannot influence its vector or retrieval similarity.
Q3. Temperature and sampling
Number: 44.9%. With T=2, P(Monday) = exp(4.1/2)/[exp(4.1/2)+exp(3.0/2)+exp(1.8/2)+exp(0.7/2)+exp(0.3/2)] = 0.4493.
Why: Higher temperature makes probabilities more even but keeps the same ranking. All five tokens remain possible under softmax; lower-ranked tokens become more likely. Temperature alone does not remove tokens from the sampling set.
Q4. Hallucinations and rare facts
Number: 20.9% (weighted singleton-fact share for the two arbitrary-fact sets). (146 × 23% + 362 × 20%)/(146 + 362) × 100 = 20.9%. This is the intended rare-fact risk proxy; strictly, singleton occurrence alone does not mathematically guarantee a wrong answer without an additional assumption.
Why: I excluded multiplication and arithmetic sequences because rules allow generalisation. Account numbers and birthdays are arbitrary facts. A prompt saying “never invent” cannot supply missing facts or guarantee accuracy; external verification is needed.
Q5. Training compute
Number: 15.70. Tokens per parameter = 21.98 trillion / 70 billion = 314; Chinchilla = 1.4 trillion / 70 billion = 20; ratio = 314/20 = 15.70.
Why: This model uses 15.7 times more tokens per parameter than Chinchilla’s compute-optimal reference, so it is over-trained by that criterion. Chinchilla argued that large models such as GPT-3 were under-trained relative to their parameter counts.
