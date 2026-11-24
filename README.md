# IR-TP1: Document Preprocessing, Representation and Indexing

## Overview
This repository contains the implementation and generated artifacts for Work Session 1 (Information Retrieval), covering text preprocessing, vocabulary and matrix representations, positional inverted indexing, preprocessing comparisons, and verification tests on a 10-document corpus (D1 to D10).

## Repository Structure
```text
Document-Preprocessing-and-Indexing/
├── README.md
├── requirements.txt
├── tp1_preprocessing_indexing.ipynb
└── results/
    ├── tokens_A.json
    ├── tokens_B.json
    ├── tokens_C.json
    ├── vocabulary_A.txt
    ├── vocabulary_B.txt
    ├── vocabulary_C.txt
    ├── binary_matrix_A.csv
    ├── binary_matrix_B.csv
    ├── binary_matrix_C.csv
    ├── tf_matrix_A.csv
    ├── tf_matrix_B.csv
    ├── tf_matrix_C.csv
    ├── inverted_index_A.json
    ├── inverted_index_B.json
    ├── inverted_index_C.json
    └── preprocessing_comparison.csv
```
## How to Run in Google Colab
1. Upload tpl_preprocessing_indexing.ipynb to Google Colab (File -> Upload notebook).
2. Run all cells sequentially from top to bottom.
3. The notebook ensures the NLTK English stop-word list is available, creates results/, runs all preprocessing, indexing, and verification steps, and exports all required files to results/.

## Implementation Choices
- Separate Preprocessing Versions (A, B, C):
  - Version A: Converts text to lowercase, replaces punctuation/non-word-non-space characters with whitespace, and splits on whitespace while leaving the raw corpus dictionary untouched.
  - Version B: Filters out English stop words using NLTK's stopwords.words('english') on top of Version A.
  - Version C: Applies NLTK's PorterStemmer to each token in Version B.
- Custom Data Structures: Vocabularies (sorted alphabetically for reproducibility), binary term-document matrices, term-frequency (TF) matrices, positional inverted indexes (term -> {doc_id: [zero_based_positions]}), and search_term() are implemented directly in Python (pandas is used only for tabular display and CSV export).

## Main Results & Verification
- Representations: Binary matrices record term presence or absence (0 or 1), whereas TF matrices record exact within-document occurrence counts.
- Comparison Metrics: results/preprocessing_comparison.csv reports vocabulary size, average tokens per document, total postings, and total stored token positions across Versions A, B, and C.
- Verification: Part 5 validates search_term() on single-document, multi-document, pre- and post-stemming (retrieval vs. retriev), and unknown query terms; checks zero-based positional accuracy against processed token lists; compares the manual D1-D3 index against the generated index; and verifies matrix values.

## Interpretation of Preprocessing Effects
Lowercasing and punctuation removal unify case variants and separate words cleanly, while stop-word removal drops high-frequency function words (and, are, the, to, etc.), reducing vocabulary size, average document length, total postings, and stored token positions. 
Applying Porter stemming further merges morphological variants (such as document/documents into document, index/indexed/indexing into index, and retrieve/retrieves/retrieval into retriev), shrinking the vocabulary and posting counts even more. 
However, preprocessing does not always improve retrieval: stemming can conflate distinct concepts or produce non-words, and removing stop words can affect phrase matching or positional proximity queries.
