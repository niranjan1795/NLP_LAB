# NLP Lab 2 — Text Segmentation

This project segments unspaced English text (e.g. `"itthatthecitytakestepstothisproblem"`)
back into individual words, using two different approaches, and compares how well each
one does.

## Files

- `text_segmentation.py` — the full solution (loads data, runs both algorithms, evaluates them)
- `text_segmentation_dataset.json` — dataset: word frequency counts + 1000 test cases with ground truth

## Dataset

The dataset (a snapshot of the Brown corpus) contains:
- `word_counts` — a vocabulary of 1500 words with how often each appears in the corpus
- `test_cases` — 1000 examples, each with:
  - `input`: unspaced text
  - `ground_truth`: the correctly spaced version
  - `word_count`: number of words in the ground truth

## Approach 1: Greedy (Longest Match)

Starting from the left, repeatedly cut off the **longest** substring that exists in the
vocabulary. If no known word matches at the current position, fall back to taking a
single character so segmentation can continue.

**Weakness:** it's short-sighted. Picking the longest word at each step can paint the
algorithm into a corner, leading to garbage later in the sentence (e.g. matching `"top"`
early on and leaving `"l a c e"` behind instead of `"place"`).

## Approach 2: Dynamic Programming (Maximum Log-Probability)

Builds the segmentation left to right, keeping track of the highest total
log-probability path to every position in the string. A word's probability is
`frequency / total_corpus_words`; unseen "words" get a small penalty (scaled by length)
so the algorithm can still handle text it hasn't seen before.

Because it considers the whole sentence rather than committing greedily, it consistently
finds the globally best segmentation.

## Evaluation Metrics

- **Accuracy** — percentage of test sentences segmented *exactly* like the ground truth
- **Edit Distance** — average Levenshtein (character-level) distance between the
  predicted sentence and the ground truth, across all test cases

## Results

| Method                                     | Accuracy | Edit Distance |
|---------------------------------------------|:--------:|:--------------:|
| Greedy (Longest Match)                     | 69.10%   | 1.290          |
| Dynamic Programming (Max Log-Probability)   | 98.20%   | 0.028          |

**Dynamic Programming clearly outperforms Greedy.** Greedy's local, word-by-word
decisions often go wrong once a "longer but incorrect" word gets picked, breaking the
rest of the sentence. DP avoids this by scoring entire segmentation paths and choosing
the one with the highest overall probability.

## How to Run

1. Place `text_segmentation_dataset.json` in the same folder as `text_segmentation.py`.
2. Run:
   ```
   python3 text_segmentation.py
   ```
3. The script will print:
   - Dataset stats
   - A few example segmentations
   - Accuracy and Edit Distance for both methods
   - A summary comparison table
