EthioSwitch-Bench


Overview

EthioSwitch-Bench is the first large-scale, human-validated benchmark for evaluating Large Language Model (LLM) performance on English–Amharic code-switching in multi-turn conversational dialogue.

Code-switching (CS) — the practice of alternating between two or more languages within a conversation — is a natural phenomenon among bilingual speakers, yet it remains a critical blind spot for most current LLMs. EthioSwitch-Bench addresses this gap by providing:

- Richly annotated multi-turn dialogues with explicit language switch tags, switch-point tokens, coherence scores, fluency ratings, and inter-annotator agreement.
- Two complementary benchmark tiers: a gold-standard hand-crafted set (v2.0) and a large-scale expanded corpus (v3.0).
- Full reproducibility: every dialogue is deterministically generated or annotated, with pinned random seeds and a complete evaluation pipeline.

Dataset Versions

| Version | Dialogues | Topics | Turns | Source |
|---------|-----------|--------|-------|--------|
| v2.0 (gold) | 30 | 7 | 150 | Hand-crafted by expert annotators |
| v3.0 (full benchmark) | 2,000 | 15 | 10,000 | 30 gold + 970 synthetic + 1,000 adapted from public corpora |



 1. Clone the Repository

git clone https://github.com/<Kindie Alebachew>/ethioswitch-bench.git
cd ethioswitch-bench

 2. Install Dependencies

pip install -r code/requirements.txt

 3. Load and Explore the Dataset

Load the full benchmark (v3.0)
with open("data/ethioswitch_bench_v3.json", "r", encoding="utf-8") as f:
    dataset = json.load(f)

print(f"Dataset: {dataset['dataset_name']} v{dataset['version']}")
print(f"Total dialogues: {dataset['total_dialogues']}")
print(f"Splits: {dataset['split']}")

 Iterate over dialogues
for dialogue in dataset["dialogues"][:3]:
    print(f"\nDialogue {dialogue['dialogue_id']} | Topic: {dialogue['topic']}")
    print(f"  CS Frequency: {dialogue['code_switch_frequency']}")
    print(f"  Dominance: {dialogue['language_dominance']}")
    for turn in dialogue["turns"]:
        print(f"  [{turn['speaker']}]: {turn['utterance'][:60]}...")

Dataset Schema

Each dialogue follows this structure:

{
  "dialogue_id": 1,
  "topic": "daily_life",
  "code_switch_frequency": "high",
  "language_dominance": "balanced",
  "split": "train",
  "source": "gold",
  "source_dataset": "original",
  "generation_method": "hand_crafted",
  "turns": [
    {
      "turn_id": 1,
      "speaker": "User",
      "utterance": "ሰላም! How was your day today?",
      "language_tags": ["[AMH]", "[ENG]"],
      "switch_type": "[AMH-ENG]",
      "switch_point_token": "ሰላም!"
    }
  ],
  "annotations": {
    "coherence_score": 4.6,
    "semantic_consistency": 0.87,
    "switch_point_naturalness": 4.4,
    "fluency_rating": 4.3,
    "annotator_agreement_kappa": 0.78
  }
}

 Field Reference

| Field | Type | Description |
|-------|------|-------------|
| `dialogue_id` | int | Unique dialogue identifier |
| `topic` | string | Domain topic (see topics list below) |
| `code_switch_frequency` | string | `low` / `medium` / `high` |
| `language_dominance` | string | `english_dominant` / `amharic_dominant` / `balanced` |
| `split` | string | `train` / `validation` / `test` |
| `source` | string | `gold` / `synthetic` / `adapted` |
| `turns[].utterance` | string | Raw mixed-language text |
| `turns[].language_tags` | list[str] | Language segment labels per utterance segment |
| `turns[].switch_type` | string | Inter-segment switch pattern (e.g., `[ENG-AMH-ENG]`) |
| `turns[].switch_point_token` | string | Token at which the first language switch occurs |
| `annotations.coherence_score` | float | Human-rated coherence: 1–5 |
| `annotations.semantic_consistency` | float | Cosine similarity across switch point: 0–1 |
| `annotations.switch_point_naturalness` | float | Human-rated naturalness: 1–5 |
| `annotations.fluency_rating` | float | Human-rated fluency: 1–5 |
| `annotations.annotator_agreement_kappa` | float | Cohen's κ inter-annotator agreement: 0–1 |

 Switch Tags

| Tag | Meaning |
|-----|---------|
| `[ENG]` | Monolingual English segment |
| `[AMH]` | Monolingual Amharic segment |
| `[ENG-AMH]` | English → Amharic switch |
| `[AMH-ENG]` | Amharic → English switch |
| `[ENG-AMH-ENG]` | English → Amharic → English (intra-sentential) |
| `[AMH-ENG-AMH]` | Amharic → English → Amharic (intra-sentential) |

 Topics

v2.0 (7 topics): `daily_life`, `technology`, `health`, `education`, `culture`, `sports`, `politics`

v3.0 (15 topics): All v2 topics plus `family_relationships`, `mental_health`, `entertainment`, `travel`, `environment`, `food_nutrition`, `finance`, `science`

Corpus Statistics (v3.0)

| Metric | Value |
|--------|-------|
| Total dialogues | 2,000 |
| Total turns | 10,000 |
| Avg turns/dialogue | 5.0 |
| Avg switch density | 10.29 / 100 tokens |
| Mean coherence score | 4.159 |
| Mean semantic consistency | 0.836 |
| Mean fluency rating | 3.951 |
| Mean switch-point naturalness | 4.013 |
| Mean IAA κ | 0.724 |

 Split Distribution

| Split | v2.0 | v3.0 |
|-------|------|------|
| Train | 21 | 1,400 |
| Validation | 4 | 298 |
| Test | 5 | 302 |

License

| Component | License |
|-----------|---------|
| Dataset (v2.0 and v3.0 gold + synthetic content) | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) |
| Adapted content (derived from DailyDialog, PersonaChat, EmpatheticDialogues, BlendedSkillTalk) | CC BY 4.0 (per source license terms) |
| Code | [MIT License](LICENSE) |

See [`LICENSE`](LICENSE) for full license text.

Acknowledgements

The v3.0 dataset includes dialogues adapted from the following publicly available corpora:

- DailyDialog — Li et al. (2017). [arXiv:1710.03957](https://arxiv.org/abs/1710.03957)
- PersonaChat — Zhang et al. (2018). [arXiv:1801.07243](https://arxiv.org/abs/1801.07243)
- EmpatheticDialogues — Rashkin et al. (2019). [arXiv:1811.00207](https://arxiv.org/abs/1811.00207)
- BlendedSkillTalk — Smith et al. (2020). [arXiv:2004.08449](https://arxiv.org/abs/2004.08449)
