# Assignment 17: Text Cleaning, Preprocessing & NLP Pipeline

A complete NLP preprocessing pipeline that turns messy raw text into clean, model-ready text. This project covers cleaning, stopword removal, tokenization, stemming, lemmatization, and one final reusable function.

> No vectorization and no ML models are used here. This assignment is only about **text preparation**.

## Dataset

**Source:** Self-created dataset of 36 product reviews, written manually for this assignment.

**File:** `product_reviews.csv` (1 column: `text`)

The reviews were written with noise on purpose, so every cleaning step has something to clean:

| Noise type | Example |
|---|---|
| Uppercase and lowercase mix | `LOVED`, `NOT` |
| Punctuation | `!!!`, `...`, `:)` |
| Numbers | `2024`, `10/10`, `9876543210` |
| Extra spaces | `"   Battery   is   bad  "` |
| URLs | `https://shop.com`, `www.audiodeals.in` |
| Email addresses | `john@gmail.com` |
| HTML tags | `<p>`, `<b>`, `<div>` |
| Emojis and symbols | `😍`, `₹`, `#`, `@` |

## Tools Used

- Python
- Pandas
- NLTK (stopwords, tokenizer, Porter stemmer, WordNet lemmatizer)
- spaCy (tokenization in Task 6)
- `re` (regular expressions)

## Project Structure

```
TuteDudeAssignments/
├── product_reviews.csv      # raw dataset
├── task1.ipynb              # Task 1: explore raw text
├── task2.ipynb              # Task 2: basic cleaning
├── task3.ipynb              # Task 3: advanced cleaning
├── task3_output.csv         # saved output of Task 3
├── task4.ipynb              # Task 4: stopwords
├── task4_output.csv         # saved output of Task 4
├── task5.ipynb              # Task 5: repeated characters and slang
├── task6.ipynb              # Task 6: tokenization
├── task7.ipynb              # Task 7: stemming
├── task8.ipynb              # Task 8: lemmatization
├── task9.ipynb              # Task 9: final pipeline
├── final_output.csv         # final cleaned data
└── README.md
```

Each notebook saves its output to a CSV, and the next notebook loads it. This avoids repeating the same cleaning steps in every file.

## Tasks

### Part 1: NLP Pipeline and Basic Text Cleaning

**Task 1: Understanding Raw Text Data** (`task1.ipynb`)
- Loaded the dataset with Pandas.
- Printed the first 5 samples and the length of each text.
- Checked for common issues: uppercase/lowercase mix, punctuation, numbers and extra spaces.

**Task 2: Basic Text Cleaning** (`task2.ipynb`)
- Lowercase, remove punctuation, remove numbers, remove extra whitespace.
- Result stored in the column `clean_text_basic`.
- Observation: HTML tags became broken words like `pgreat productp`, and URLs and emails became `httpsshopcom` and `johngmailcom`. This is why Task 3 is needed.

### Part 2: Advanced Text Cleaning

**Task 3: Removing Noise** (`task3.ipynb`)
- Removed URLs, emails and HTML tags first, then special characters and emojis, then lowercase, numbers and extra spaces.
- Result stored in the column `clean_text_advanced`.
- Key learning: **the order matters.** URLs, emails and tags need symbols like `://`, `@` and `<>` to be found, so they must be removed before punctuation.

**Task 4: Handling Stopwords** (`task4.ipynb`)
- Loaded English stopwords with NLTK.
- Removed stopwords from `clean_text_advanced`.
- Result stored in the column `text_no_stopwords`.
- Observation: `not` is a stopword, so `not good would not recommend` became `good would recommend`, which flips the meaning. For sentiment tasks, negation words should be kept.

**Task 5: Repeated Characters and Slang (Optional)** (`task5.ipynb`)
- Normalized repeated characters with regex: `soooo` becomes `so`.
- Built a small slang dictionary (`u`, `gr8`, `thx`, `plz`, `asap`) and replaced slang words.
- Used a small separate demo dataset, because the main reviews had no slang.
- Limitations: `goooood` becomes `god`, and replacing `its` with `it is` can be wrong.

### Part 3: Basic Text Preprocessing in NLP

**Task 6: Tokenization** (`task6.ipynb`)
- Word tokenization and sentence tokenization (spaCy and NLTK).
- Displayed tokens for 3 samples.
- Sentence tokenization uses the original `text`, because sentence boundaries need punctuation.

**Task 7: Stemming** (`task7.ipynb`)
- Applied the Porter Stemmer to `text_no_stopwords`.
- Compared original words with stemmed words.
- Observation: stems are not always real words (`battery` becomes `batteri`, `quality` becomes `qualiti`).

**Task 8: Lemmatization** (`task8.ipynb`)
- Applied the WordNet Lemmatizer.
- Compared stemming and lemmatization.
- Observation: lemmatization gives real words. By default WordNet treats every word as a noun, so `loved` stays `loved` unless `pos="v"` is given.

**Task 9: Final NLP Pipeline** (`task9.ipynb`)

One function, `nlp_preprocess(text)`, that runs all stages in order:

1. Lowercasing
2. Noise removal (URLs, emails, HTML tags, special characters, numbers, extra spaces)
3. Stopword removal
4. Tokenization
5. Lemmatization

```python
df["final_clean_text"] = df["text"].apply(nlp_preprocess)
```

Applied to the full dataset, with the output stored in `final_clean_text`.

**Task 10: Observations and Insights**
- Basic vs advanced cleaning.
- Why lemmatization is preferred over stemming.
- Why preprocessing matters in NLP models.

## Example

| Raw text | Final clean text |
|---|---|
| `LOVED this phone!!! Best purchase of 2024 😍😍 visit https://shop.com for more` | `loved phone best purchase visit` |
| `<p>Great product</p> Worth ₹15,000!!! #happy @amazon` | `great product worth happy amazon` |
| `Amazing sound quality 🎧🔥 check www.audiodeals.in now` | `amazing sound quality check` |

## Key Learnings

- Cleaning steps must run in the right order. Remove URLs, emails and HTML first, and punctuation after.
- Spaces should be cleaned last, because earlier removals leave gaps.
- Stemming is fast but crude. Lemmatization is slower but gives real words.
- Preprocessing should match the task. Removing stopwords like `not` can change the meaning in sentiment analysis.

## How to Run

1. Install the libraries:
   ```
   pip install pandas nltk spacy
   python -m spacy download en_core_web_sm
   ```
2. Download the NLTK data (once):
   ```python
   import nltk
   nltk.download("stopwords")
   nltk.download("punkt_tab")
   nltk.download("wordnet")
   ```
3. Run the notebooks in order, from `task1.ipynb` to `task9.ipynb`.

## Author

Sagar · GitHub: [sagardubeyokk](https://github.com/sagardubeyokk)
