
# TrendPulse – Part 1: Python Foundations & Data Structures

## 1. Project Overview

TrendPulse analyses YouTube Trending Video Statistics from three countries using core Python concepts and data structures.

The objective is to understand trending video patterns, clean raw data, organize records and classify videos using rule-based logic.

## 2. Dataset Information

Dataset: Trending YouTube Video Statistics

Countries used:
- United States (US)
- Great Britain (GB)
- India (IN)

Initial dataset records:

| Country | Records |
|---|---:|
| US | 16,080 |
| GB | 15,075 |
| IN | 15,075 |
| Total | 46,230 |

Dataset files:
- USvideos.csv
- GBvideos.csv
- INvideos.csv
- categories.csv

## 3. Tools and Dependencies

- Python
- Google Colab
- Pandas
- NumPy
- Python datetime module

Install dependencies:

pip install -r requirements.txt

## 4. Project Structure

part1_python_foundations/
- data/
  - USvideos.csv
  - GBvideos.csv
  - INvideos.csv
  - categories.csv
- trendpulse_part1.ipynb
- requirements.txt
- README.md

## 5. Data Loading

A reusable load_country() function was implemented.

It accepts:
- Dataset file path
- Country code
- Optional maximum number of rows

The function verifies required columns, loads CSV data and returns a list of dictionaries.

A category_names dictionary maps category IDs to category names.

## 6. Data Cleaning Decisions

The following cleaning rules were applied:

| Field | Conversion | Invalid values |
|---|---|---|
| views | Integer | Replace with 0 |
| likes | Integer | Replace with 0 |
| dislikes | Integer | Replace with 0 |
| comment_count | Integer | Replace with 0 |
| Boolean flags | Boolean | Replace with False |
| trending_date | Date | Replace with None |
| publish_time | Datetime | Replace with None |

Try/except blocks were used to prevent individual parsing failures from stopping the program.

Invalid values were replaced rather than dropping entire records, preserving useful video information.

Parsing failures were counted and reported separately for each field.

## 7. Python Data Structures

The project demonstrates:

- Lists: Store cleaned video records.
- Tuples: Store fixed video information.
- Sets: Identify unique video IDs and compare countries.
- Dictionaries: Count channel appearances.
- Nested dictionaries: Summarize records by country and category.
- Slicing: Display the 15 most frequent trending channels.

Lists, dictionaries and sets are mutable, while tuples are immutable.

## 8. Variable Scope

A global variable was declared outside a function and accessed inside it.

A local variable was declared inside a function and demonstrated to be inaccessible directly outside that function.

## 9. Loops and String Processing

The title_features() function extracts:

- Uppercase word count
- Exclamation mark count
- Question mark count
- Total word count

The project demonstrates for loops, while loops, range(), break and continue.

## 10. Rule-Based Classification

Videos are classified using views, like ratios and feedback availability.

Classification thresholds:

| Parameter | Threshold |
|---|---:|
| Viral views | 1,000,000 |
| Steady views | 100,000 |
| High like ratio | 80% |
| Low like ratio | 50% |

Classification labels:
- Viral Hit
- Steady Performer
- Divisive
- Limited Feedback
- Low Engagement
- Unrated

Zero reactions, disabled ratings, missing values and zero views are handled explicitly to prevent division-by-zero errors.

The notebook reports the number and percentage of records in each classification.

## 11. Refactoring

Repeated dictionary-counting logic was refactored into a reusable count_by_field() function.

This reduced duplicated code and improved maintainability.

## 12. How to Run

1. Open trendpulse_part1.ipynb in Google Colab.
2. Make sure the four CSV files are available in the configured data directory.
3. Install dependencies using requirements.txt if required.
4. Run all notebook cells from beginning to end.
5. Review the output and classification summaries.

## 13. Data Source

Trending YouTube Video Statistics dataset.

Original dataset source: Kaggle.

The dataset is publicly available and is used for educational analysis.
