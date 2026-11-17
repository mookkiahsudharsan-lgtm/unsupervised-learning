# Unsupervised Learning - Part 1: Market Basket Analysis

Preparing grocery transaction data for market basket analysis and setting up itemset candidate generation for association rule mining.

## Overview

Unsupervised learning finds hidden structure in data without training labels. It is used to learn a better representation of the features, reduce the number of features for visualization and more effective supervised learning, and identify subgroups.

This notebook is the first part of a series on unsupervised learning. The planned topics are market basket analysis, dimensionality reduction and clustering. This part covers **market basket analysis (MBA)**: finding which combinations of products are often bought together, as an *antecedent* (items already in the basket) and a *consequent* (items bought with them).

## Key Concepts

Association rules are evaluated with three metrics:

| Metric | Meaning |
|---|---|
| **Support** (prevalence) | How often an itemset (antecedent and consequent together) appears in the transactions |
| **Confidence** (predictability) | Given the antecedent was bought, how likely the consequent is to be bought |
| **Lift** (interest) | How much more likely the association is than would be expected by chance |

To check that a rule will generalize to new data rather than reflect random sampling, the notebook introduces **Fisher's exact test** on a contingency table. Small samples can produce unreliable rules.

## Dataset

`grocery_transactions.csv` - one row per shopping basket, with item names across columns and no header row (not included in this repository; place it in a `data/` folder).

| Fact | Value |
|---|---|
| Transactions (baskets) | 9,835 |
| Item purchases in total | 43,367 |
| Distinct items | 169 |
| Maximum basket size | 32 items |

Most frequent items: whole milk (2,513), other vegetables (1,903), rolls/buns (1,809), soda (1,715) and yogurt (1,372). The rarest items appear only once or twice (for example baby food and sound storage medium).

## What the Notebook Does

1. **Load the data** with pandas and inspect the first rows, length and column counts.
2. **Plot basket size** - number of transactions by number of items in the basket.
3. **Reshape to a transaction-product matrix** - stack the item columns into one long series, one-hot encode with `pd.get_dummies`, and sum by transaction ID (9,835 x 169).
4. **Plot item support** - the share of transactions containing each item.
5. **Wrap the preparation in a function** (`retrieve_transaction_data`) for reuse.
6. **Set up candidate generation** with a minimum support of 0.01 and list all possible itemsets of length 1 to 4.

The number of possible itemsets grows very quickly, which is why itemsets with low support must be pruned:

| Itemset length | Possible itemsets |
|---|---|
| 1 | 169 |
| 2 | 14,196 |
| 3 | 790,244 |
| 4 | 32,795,126 |

## Getting Started

Requirements: Python 3, pandas, numpy, matplotlib, seaborn, jupyterthemes, Jupyter.

```bash
pip install pandas numpy matplotlib seaborn jupyterthemes jupyter
jupyter notebook unsupervised_learning-Part_1.ipynb
```

## Project Structure

```
unsupervised_learning-Part_1.ipynb
data/
  grocery_transactions.csv
README.md
```

## Status and Notes

- This part stops after candidate generation. Computing frequent itemsets, building association rules (support, confidence, lift), Fisher's exact test, dimensionality reduction and clustering are not yet implemented.
- The notebook reads the file with a Windows-style path (`.\data\grocery_transactions.csv`); change it to `./data/grocery_transactions.csv` on macOS or Linux.
- `jupyterthemes` only styles the plots; remove the `jtplot.style(...)` line if you do not want to install it.
