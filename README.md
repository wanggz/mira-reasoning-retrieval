<h1 align="center">⚡ FAL-Aligner + MRE-T1 Pipeline</h1>

<p align="center">
  <strong>Reinforcement Learning-Driven Intent Alignment: Overcoming Surface Semantic Traps in Retrieval</strong>
</p>

<p align="center">
  <a href="https://brightbenchmark.github.io/"><img src="https://img.shields.io/badge/BRIGHT_Benchmark-Rank_1st-8A2BE2" alt="Rank"></a>
  <a href="https://huggingface.co/ForwardAILabs/MRE-T1"><img src="https://img.shields.io/badge/🤗_HuggingFace-MRE--T1-yellow" alt="Model"></a>
  <a href="https://www.mira.day/"><img src="https://img.shields.io/badge/Forward_AI_Labs-mira.day-green" alt="Company"></a>
  <a href="#"><img src="https://img.shields.io/badge/Pipeline-YAML_Driven-blue" alt="Pipeline"></a>
  <a href="#"><img src="https://img.shields.io/badge/Training-GRPO_Aligned-red" alt="Training"></a>
</p>

---

**MRE-T1** (Mira Recruitment Embedding, Thought v1) is a reasoning-enhanced retrieval model developed by [Forward AI Labs](https://www.mira.day/), a company specializing in recruitment AI agents. Combined with **FAL-Aligner**, our pipeline achieves **No. 1** on the [BRIGHT Benchmark](https://brightbenchmark.github.io/) across all evaluated dimensions — including both short and long document tracks — and **No. 1** among single-model methods.

## Key Results

| Track | Metric | MRE-T1 Pipeline | Runner-up |
| :--- | :---: | :---: | :---: |
| Pipeline — Short Docs | Avg NDCG | **66.9** | 63.4 |
| Pipeline — Long Docs | Avg NDCG | **56.0** | 54.6 |
| Single Model — Short Docs | Avg NDCG | **39.6** | 38.3 |
| Single Model — Long Docs | Avg NDCG | **35.1** | 33.2 |

## Links

- **Model**: [ForwardAILabs/MRE-T1 on HuggingFace](https://huggingface.co/ForwardAILabs/MRE-T1)
- **Benchmark**: [BRIGHT Benchmark](https://brightbenchmark.github.io/)
- **Company**: [Forward AI Labs (mira.day)](https://www.mira.day/)
- **Full Technical Details**: [docs/index.md](docs/index.md)
