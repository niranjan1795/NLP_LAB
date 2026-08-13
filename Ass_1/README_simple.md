# NLP Assignment 1: Hindi Text Preprocessing & Tokenization

## What this does
This project takes raw Hindi text, splits it into sentences, splits each sentence into words, and then reports some basic stats about the text (like word count and average sentence length). Everything is done with custom Python code (regex) — no ready-made tokenizer libraries.

## Data used
- **IndicCorpV2 (Hindi)** — from AI4Bharat
- **OSCAR-2301 (Hindi)** — a large web-crawled corpus (requires Hugging Face login/access approval)

## How the tokenizer works
1. **Sentence splitting**: Text is split at `.`, `?`, `!`, and the Hindi sentence-end marks `।` / `॥`. Before splitting, things like URLs, emails, dates, and decimal numbers are protected so they don't get cut in the middle (e.g. `3.14` or `12/05/2025` won't be split at the `.` or `/`).
2. **Word splitting**: Each sentence is broken into tokens — Hindi words, English words, numbers, and punctuation are each kept as separate tokens. Nothing gets dropped.

## Folder contents
```
Assignment_1/
├── Assignment1.ipynb        # Main notebook with all the code
├── README.md
├── requirements.txt
├── indic_tokenized.parquet  # Tokenized IndicCorpV2 sentences
├── indic_stats.txt          # Stats for IndicCorpV2
└── venv/
```

## How to run
```bash
# Set up environment
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python -m ipykernel install --user --name=nlp-assignment1 --display-name "Python (NLP Assignment 1)"
```
Then either open `Assignment1.ipynb` and run the cells, or run it headlessly:
```bash
jupyter nbconvert --to notebook --execute --inplace Assignment1.ipynb
```

## Output files
- `indic_tokenized.parquet` — one tokenized sentence per row
- `indic_stats.txt` — stats summary for IndicCorpV2
- `oscar_tokenized.parquet` / `oscar_stats.txt` — same, for OSCAR-2301 (only produced if you have access)

## Results (IndicCorpV2, 50,000 sentences)
| Metric | Value |
|---|---|
| Total sentences | 50,000 |
| Total words | 919,334 |
| Total characters | 3,403,766 |
| Avg. sentence length | 18.39 words |
| Avg. word length | 3.70 characters |
| Type/Token Ratio | 0.0535 |

## Note on OSCAR-2301 access
OSCAR-2301 is gated. To use it: request access on its [Hugging Face page](https://huggingface.co/datasets/oscar-corpus/OSCAR-2301), accept the license, then run `hf auth login` with your access token. If access isn't set up, the notebook just prints a message instead of crashing.
