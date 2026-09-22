# Vehicle Listing NER

A small NLP service for turning noisy, free-text vehicle listings into a canonical vehicle record.

The pipeline first extracts structured entities with a custom spaCy NER model, then resolves the extracted make/model/trim against a known vehicle catalog. It was built for listings where abbreviations, spelling variants, punctuation, and incomplete trims make exact matching unreliable.

## What it extracts

- make, model, trim, and year
- VIN
- phone number

## How it works

1. **Custom NER** — `main/model.py` trains a blank English spaCy pipeline with a custom tokenizer that preserves hyphenated vehicle names.
2. **Normalization** — common aliases such as Chevy/Chevrolet and VW/Volkswagen are normalized before matching.
3. **Candidate narrowing** — catalog candidates are filtered by year, make, and model.
4. **Entity resolution** — fuzzy string matching and spaCy vector similarity rank trim candidates; confidence thresholds decide whether there is enough information to return a canonical match.
5. **API** — `main/server.py` exposes the pipeline through a Flask `/NER` endpoint.

## Repository map

| Path | Purpose |
| --- | --- |
| `main/model.py` | NER training, inference, scoring, and model persistence |
| `main/similarity.py` | catalog narrowing, fuzzy matching, vector similarity, and confidence policy |
| `main/server.py` | Flask API and end-to-end normalization flow |
| `main/ner.ipynb` | training and experimentation |
| `main/ner_validation.ipynb` | validation workflow |
| `Information Extraction on vehicle postings.pdf` | project write-up |

## Context

This is an early-career applied-NLP project rather than a maintained library. The public repository contains the implementation and write-up; production data and trained model artifacts are not included.
