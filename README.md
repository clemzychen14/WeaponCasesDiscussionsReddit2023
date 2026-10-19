# Weapon case discussions on Reddit in 2023

This repository provides the collected discussion texts and the selected Correlated Topic Model (CTM) associated with the following publication:

Chen, Z., & Zaman, B. (2025). In case players were wondering: A topic modelling and sentiment analysis study of community discussions on weapon cases in the CS:GO game. *Entertainment Computing, 54*, 100936. [https://doi.org/10.1016/j.entcom.2025.100936](https://doi.org/10.1016/j.entcom.2025.100936)

The study examines how players discuss and make sense of weapon cases in Counter-Strike: Global Offensive (CS:GO). It combines correlated topic modelling and lexicon-based sentiment analysis to investigate the topics, their associations, and the sentiments expressed in two Reddit communities.

## Repository contents

| File | Description |
| :--- | :--- |
| [filtered_comments1.json](filtered_comments1.json) | Collected discussion texts after filtering to the study period and before linguistic preprocessing. The file contains 22,353 strings comprising submission titles and comments. |
| [optimal_ctm.bin](optimal_ctm.bin) | Saved four-topic CTM selected for the study, produced with `tomotopy`. |

The repository contains the data and saved model only. Manuscripts, the original Jupyter notebook, and other submission materials are not included.

## Data and sampling

The corpus covers discussions from 1 January to 31 December 2023 in **r/csgo** and **r/GlobalOffensive**. Posts were identified using the Reddit API through PRAW with the search terms “loot”, “loot box”, “loot boxes”, “box”, “boxes”, “case”, and “cases”; their associated comments were also collected.

The JSON file is a UTF-8 encoded array of text strings. Despite its filename, it includes both submission titles and comments. It contains no separate fields for usernames, timestamps, subreddit identifiers, or thread relationships. It is a purposively collected corpus and should not be treated as an exhaustive archive of either subreddit or as a representative sample of all players.

Linguistic preprocessing included lowercasing, tokenization, part-of-speech-informed lemmatization, and removal of stop words and selected terms. Removing 665 empty entries left **21,688 documents** for modelling. The JSON preserves the texts before these linguistic transformations.

## Saved model

The selected model has four topics, interpreted in the study as Monetary Elements, Gaming and Gambling Concerns, Gameplay Issues, and Affective Appreciation. Topic numbers in the Python API are indexed from zero.

The original notebook records the following settings for the selected model:

| Setting | Value |
| :--- | :--- |
| Implementation | `tomotopy.CTModel` |
| Recorded tomotopy version | 0.12.5 |
| Number of topics (`k`) | 4 |
| Random seed | 144 |
| Topic-word prior (`eta`) | 0.009 |
| Most frequent vocabulary terms removed (`rm_top`) | 3: get, open, like |
| Training iterations | 100 |
| Workers for final training | 1 |

The notebook records 177,589 modelled tokens and 8,011 retained vocabulary terms. The model does not include the NRC sentiment lexicon or a separately trained sentiment classifier.

## Loading the files

The texts can be read with the Python standard library:

```python
import json

with open("filtered_comments1.json", encoding="utf-8") as source:
    texts = json.load(source)

print(len(texts))  # 22353
```

The saved model can be inspected with tomotopy:

```python
import tomotopy as tp

model = tp.CTModel.load("optimal_ctm.bin")

for topic_id in range(model.k):
    print(f"Topic {topic_id + 1}")
    print(model.get_topic_words(topic_id, top_n=20))

print(model.get_correlations())
```

The recorded training version is supplied for reference; compatibility with other versions has not been verified. These files support inspection and further analysis, but do not constitute a complete executable replication package. Consult the article for the full methodological account and interpretation of the results.

## Research use and citation

If these materials inform your research, please cite the associated article:

```bibtex
@article{ChenZaman2025WeaponCases,
  author  = {Chen, Zhiyu and Zaman, Bieke},
  title   = {In case players were wondering: A topic modelling and sentiment analysis study of community discussions on weapon cases in the {CS:GO} game},
  journal = {Entertainment Computing},
  volume  = {54},
  pages   = {100936},
  year    = {2025},
  doi     = {10.1016/j.entcom.2025.100936}
}
```

The texts originate from public Reddit discussions. Although account identifiers are not stored as separate fields, verbatim text may remain searchable. Please avoid attempts to identify contributors and consider this when quoting or redistributing excerpts. No additional reuse licence is specified in this repository.

## Author

**Zhiyu Chen**

[Homepage](https://www.zhiyu-chen.com/) · [ORCiD](https://orcid.org/0009-0003-1946-2280) · [LinkedIn](https://www.linkedin.com/in/zhiyu-chen-a34b3324b)
