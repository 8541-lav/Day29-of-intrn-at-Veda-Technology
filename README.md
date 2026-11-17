# Product Basket Analysis

**Task 29 · Data Analytics Track · Python + Pandas**

Find products that are frequently purchased together using association-style (market-basket) analysis on the **Online Retail II** dataset.

## Overview

Each order is treated as a basket. Every pair of different products in the same basket is counted, then ranked by count, support, confidence and lift. The findings are turned into practical recommendations such as bundles, "frequently bought together" prompts and product placement.

## Objectives

- Introduce association-style analysis
- Produce a list of top product pairs
- Turn the findings into recommendations

## Project Structure

```
.
├── product_basket_analysis.py              # Main analysis script
├── product_pairs.csv                       # All pairs seen in 50+ orders, with metrics
├── Product_Basket_Analysis_Report.docx     # Full report
├── online_retail_II.xlsx                   # Input dataset (not included, see below)
└── README.md
```

## Requirements

- Python 3.9+
- pandas
- openpyxl

```bash
pip install pandas openpyxl
```

## Dataset

[Online Retail II (UCI)](https://archive.ics.uci.edu/dataset/502/online+retail+ii): UK online retailer transactions, Dec 2009 – Dec 2011, in two sheets (2009-2010 and 2010-2011).

Download `online_retail_II.xlsx` and place it in the project folder. If needed, edit the file path on the `f=` line in the script.

## How to Run

```bash
python product_basket_analysis.py
```

The script prints summary statistics and the top pairs, and writes `product_pairs.csv`.

## Method

1. **Load** both sheets and combine them.
2. **Clean**: remove missing descriptions, cancelled invoices (starting with `C`), non-positive quantity or price, and non-product codes (postage, bank charges, fees).
3. **Build baskets** by grouping on `Invoice` (order ID), keeping unique products per order.
4. **Count pairs** with `itertools.combinations(items, 2)`, which generates each unordered pair once, so self-pairs `(A, A)` are excluded. Orders with more than 100 distinct items are skipped for pairing.
5. **Compute metrics** and keep pairs that appear in at least 50 orders.

### Metrics

| Metric | Meaning |
|---|---|
| Support | Share of all orders containing both A and B |
| Confidence (A→B) | Of orders containing A, the share that also contain B |
| Lift | How much more often A and B occur together than if independent (above 1 = positive association) |

## Key Results

- **39,516** valid orders and **4,732** distinct products analysed
- **Most frequent pair:** Jumbo Bag Pink Polkadot + Jumbo Bag Red Retrospot (1,191 orders)
- **Strongest association:** Cast Iron Hook Garden Fork + Trowel (lift ≈ 165)

### Top 5 pairs by frequency

| # | Product A | Product B | Orders | Support | Lift |
|---|---|---|---|---|---|
| 1 | Jumbo Bag Pink Polkadot | Jumbo Bag Red Retrospot | 1,191 | 3.0% | 5.2 |
| 2 | Red Hanging Heart T-Light Holder | White Hanging Heart T-Light Holder | 1,082 | 2.7% | 4.4 |
| 3 | Jumbo Storage Bag Suki | Jumbo Bag Red Retrospot | 1,020 | 2.6% | 4.3 |
| 4 | Lunch Bag Red Retrospot | Lunch Bag Pink Polkadot | 994 | 2.5% | 6.1 |
| 5 | Lunch Bag Red Retrospot | Lunch Bag Black Skull | 994 | 2.5% | 5.5 |

### Top 5 pairs by lift (100+ orders)

| # | Product A | Product B | Orders | Lift |
|---|---|---|---|---|
| 1 | Cast Iron Hook Garden Fork | Cast Iron Hook Garden Trowel | 124 | 165 |
| 2 | Kids Rain Mac Blue | Kids Rain Mac Pink | 140 | 123 |
| 3 | Dolly Girl Childrens Bowl | Dolly Girl Childrens Cup | 113 | 120 |
| 4 | Wooden Owls Light Garland | Light Garland Butterflies Pink | 100 | 114 |
| 5 | French Blue Metal Door Sign 4 | French Blue Metal Door Sign 0 | 101 | 108 |

## Recommendations

- **Bundles and multi-buy deals** for high-lift sets (garden fork + trowel, kids rain macs, herb markers)
- **"Frequently bought together" prompts** on product pages, chosen by confidence A→B
- **Variety packs** for the Lunch Bag and Jumbo Bag ranges
- **Place related items together** on the website, in the catalogue and on warehouse pick lists
- **Align stock levels** for items in strong pairs
- **A/B test** bundles and prompts before a full rollout

## Interview Questions

**What is market-basket analysis?**
A data-mining technique that finds which items customers tend to buy together by examining many transactions. It is used for cross-selling, bundling, store layout and recommendations.

**Why use order ID?**
The order ID defines a basket, the set of items bought in one transaction. Co-purchase only makes sense within one basket. Grouping by customer or date would pair items bought on different occasions and create false associations. It also gives the correct denominator for support.

## Limitations

- Association is not causation.
- Many top pairs are colour or design variants of the same product line.
- The data comes from one UK-based retailer with mostly wholesale customers, so results may not generalise.
- Only pairs are analysed.

## Next Steps

- Run Apriori or FP-Growth (`mlxtend`) for rules with 3+ items
- Analyse by country or season
- Merge variants into product families to find cross-family associations
