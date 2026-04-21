<h1 align="center">⚡ FAL-Aligner + MRE-T1 Pipeline</h1>

<p align="center">
  <strong>Reinforcement Learning-Driven Intent Alignment: Overcoming Surface Semantic Traps in Retrieval</strong>
</p>

<p align="center">
  <a href="https://brightbenchmark.github.io/"><img src="https://img.shields.io/badge/BRIGHT_Benchmark-Rank_1st-8A2BE2" alt="Rank"></a>
  <a href="#"><img src="https://img.shields.io/badge/Pipeline-YAML_Driven-blue" alt="Pipeline"></a>
  <a href="#"><img src="https://img.shields.io/badge/Training-GRPO_Aligned-red" alt="Training"></a>
</p>

## 📖 Overview

**FAL-Aligner** represents a paradigm shift in reasoning-intensive retrieval tasks. Conventional models and agentic pipelines rely heavily on query expansion and are prone to the superficial semantic trap—a scenario where negative samples exhibit high surface-level textual similarity to queries yet are logically mismatched.
* Core mechanism: A multi-dimensional GRPO (Group Relative Policy Optimization) training framework.
* Technical objective: To distill core retrieval intents, filter misleading noise, and achieve high-precision alignment with the MRE-T1 retriever.
* Industrial standing: Currently ranks first on the BRIGHT Benchmark across both long-document and short-document tracks, outperforming sophisticated rerankers and existing alignment models.

---

## 🚀 Highlights

The **FAL-Aligner** shifts the retrieval paradigm from simple additive expansion to reinforcement-learning-driven intent distillation:

* **Discriminative Feature Extraction**
  * Dynamically identifies and preserves the core intents within queries that yield high retrieval gains.
  * Strongly filters out redundant or misleading superficial semantic noise, endowing the model with immunity to "semantic traps" in complex reasoning tasks.

* **Environment-Aware Semantic Alignment**
  * Goes beyond static supervision and enables interactive learning via retrieval feedback within the document corpus environment.
  * Leverages the GRPO framework to autonomously explore discriminative features, achieving the leap from "text matching" to "deep intent alignment."

* **Semantic Distillation & SNR Enhancement**
  * Significantly improves the signal-to-noise ratio (SNR) of critical retrieval signals by eliminating information redundancy.
  * Effectively mitigates the common "feature dilution" effect of embedding models under long-text inputs, ensuring that vector representations remain focused on the core task.
  
---

## 🛠️ Methodology & Architecture

To overcome the limitations of standard LLM rewriting and conventional SFT (Supervised Fine-Tuning), we have constructed a highly robust engineering pipeline.

### 1. Candidate Rewrite Mining
To prevent the model from converging on rigid, template-based shortcuts during SFT, we implemented a heterogeneous candidate rewrite mining strategy. For every query, we dynamically synthesized a diverse set of natural-language-structured rewrites. This approach forces the model to prioritize underlying retrieval intent over superficial syntactic patterns, significantly enhancing the robustness of the resulting policy distribution.

### 2. Cold Start
Traditional cross-entropy loss is inherently misaligned with retrieval objectives. By injecting natural language structural features derived from the mining stage, we equip the base model with strong discriminative feature extraction capabilities, thereby laying a stable initialization foundation for the subsequent GRPO phase.

### 3. GRPO-Driven Intent Alignment
Unlike DPO, which relies on static preference data, GRPO enables the model to engage in interactive learning via retrieval feedback within the actual document corpus environment. This allows the model to autonomously explore and extract highly discriminative features, facilitating a fundamental leap from superficial "textual matching" to deep "intent alignment". By reshaping the policy distribution, GRPO creates steep probability peaks over optimal reasoning paths, ensuring consistent and high-quality retrieval outcomes.

Our Multi-Dimensional Reward Function includes:
* **Primary Reward:** Dense retrieval NDCG scores.
* **Constraint:** Length penalty to prevent feature dilution.
* **Reward Shaping:** Cosine similarity with positive examples to maintain semantic grounding.

---

## 🏆 Performance

**FAL-Aligner + MRE-T1** achieves state-of-the-art results on the rigorous BRIGHT Benchmark. The system demonstrates categorical dominance by securing the No. 1 position across all evaluated dimensions, including both short and long document tracks. Notably, our framework outperforms both existing query alignment models and complex, resource-heavy agentic pipelines, establishing a new performance frontier in reasoning-intensive retrieval.

1、**Single-model Methods on Short Documents**
| Model | Bio. | Earth. | Econ. | Psy. | Rob. | Stack. | Sus. | Leet. | Pony | AoPS | TheoQ. | TheoT. | Avg | 
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| MRE-T1 | 55.3 | 56.5 | 32.9 | **48.2** | 33.1 | 34.2 | 37.3 | 35.0 | **35.5** | **16.7** | **43.3** | 46.9 | **39.6** |
| llama-nv-embed-reasoning-3b | **63.4** | **60.2** | **39.5** | 45.5 | 32.6 | 34.0 | **43.3** | 37.5 | 15.0 | 10.5 | 39.5 | 38.5 | 38.3 |
| ReasonEmbed-Qwen3-8B-0928 | 55.5 | 56.6 | 36.2 | 47.4 | **35.3** | **36.6** | 39.1 | 33.6 | 16.4 | 12.5 | 41.4 | **47.2** | 38.2 |
| ReasonEmbed-Qwen3-4B-0928 | 55.4 | 54.5 | 34.9 | 46.9 | 34.0 | 36.1 | 37.4 | 34.5 | 13.6 | 11.3 | 41.4 | 45.1 | 37.1 |
| Seed-1.5-Embedding | 34.8 | 46.9 | 23.4 | 31.6 | 19.1 | 25.4 | 21.0 | **43.2** | 4.9 | 12.2 | 33.3 | 30.5 | 27.2 |
| inf-retriever-v1-pro | 37.8 | 39.7 | 26.2 | 34.4 | 20.1 | 22.6 | 26.3 | 38.3 | 1.9 | 13.6 | 30.5 | 25.5 | 26.4 |
| ReasonIR-8B | 26.2 | 31.4 | 23.3 | 30.0 | 18.0 | 23.9 | 20.5 | 35.0 | 10.5 | 14.7 | 31.9 | 27.2 | 24.4 |
| Qwen3-Embedding-8B | 21.0 | 33.8 | 18.6 | 27.8 | 15.6 | 18.6 | 17.1 | 33.5 | 1.2 | 9.5 | 40.6 | 39.6 | 23.1 |
| bm25 | 18.9 | 27.2 | 14.9 | 12.5 | 13.6 | 18.4 | 15.0 | 24.4 | 7.9 | 6.2 | 10.4 | 4.9 | 14.5 |
| bge-large-en-v1.5 | 1.7 | 24.6 | 16.6 | 17.5 | 11.7 | 10.8 | 13.3 | 26.7 | 5.7 | 6.0 | 13.0 | 6.9 | 13.7 |

2、**Retrieval Pipelines on Short Documents**
| Model | Bio. | Earth. | Econ. | Psy. | Rob. | Stack. | Sus. | Leet. | Pony | AoPS | TheoQ. | TheoT. | Avg | 
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **MRE-T1-Pipeline (FAL + T1)** | **86.7** | **78.5** | 69.7 | **78.2** | **58.4** | **67.0** | **65.9** | 46.8 | **73.4** | 45.2 | **60.6** | **72.3** | **66.9** |
| INF-X-Retriever (inf+retrieve) | 79.8 | 70.9 | **69.9** | 73.3 | 57.7 | 64.3 | 61.9 | **56.1** | 54.5 | **51.9** | 53.1 | 67.9 | 63.4 |
| RakanEmb4B (inf+retrieve) | 65.9 | 56.9 | 59.0 | 60.7 | 49.0 | 52.8 | 53.3 | 35.6 | 55.4 | 22.0 | 53.9 | 64.5 | 52.4 |
| Nemo Retriever's Agentic Retrieval | 72.8 | 66.0 | 48.7 | 59.6 | 52.5 | 47.1 | 50.2 | 49.3 | 42.1 | 21.0 | 53.3 | 48.0 | 50.9 |
| DIVER-v3-GroupRank | 66.0 | 63.7 | 42.4 | 55.0 | 40.6 | 44.7 | 50.4 | 32.5 | 47.3 | 17.2 | 46.4 | 55.6 | 46.8 |
| BGE-Reasoner-0928 | 68.5 | 66.4 | 40.6 | 53.1 | 43.2 | 44.1 | 47.8 | 29.0 | 41.6 | 17.2 | 46.5 | 58.4 | 46.4 |
| Lattice Hierarchical Retrieval | 64.4 | 62.4 | 45.4 | 57.4 | 47.6 | 37.6 | 46.4 | 19.9 | 34.0 | 12.0 | 30.1 | 47.8 | 42.1 |
| ReasonRank (rerank RaDer) | 62.7 | 55.5 | 36.7 | 54.6 | 35.7 | 38.0 | 44.8 | 29.5 | 25.6 | 14.4 | 42.0 | 50.1 | 40.8 |
| XRR2 | 63.1 | 55.4 | 38.5 | 52.9 | 37.1 | 38.2 | 44.6 | 21.9 | 35.0 | 15.7 | 34.4 | 46.2 | 40.3 |
| RaDeR with Qwen reranking | 58.0 | 59.2 | 33.0 | 49.4 | 31.8 | 39.0 | 36.4 | 33.5 | 33.3 | 10.8 | 34.2 | 51.6 | 39.2 |
| ReasonIR with Rank-R1 | 59.5 | 55.1 | 37.9 | 52.7 | 30.0 | 39.3 | 45.1 | 32.1 | 17.1 | 10.7 | 40.4 | 45.6 | 38.8 |

3、**Single-model Methods on Long Documents**
| Model | Bio. | Earth. | Econ. | Psy. | Rob. | Stack. | Sus. | Pony | Avg | 
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| MRE-T1 | **46.5** | **46** | **34.5** | **52.7** | **27.7** | **22.2** | **45.2** | **6.3** | **35.1** |
| Google-Gecko-Text_Embedding-004 | - | - | - | - | - | - | - | - | 33.2 |
| inf-retriever-v1-pro | 44.1 | 42.2 | 31.4 | 43.1 | 20.8 | 21.4 | 41.0 | 0.4 | 30.5 |
| gte-Qwen1.5-7B-instruct | 39.2 | 36.1 | 25.7 | 42.3 | 21.3 | 23.5 | 33.1 | 1.3 | 27.8 |
| SFR-Embedding-Mistral | 30.3 | 37.0 | 24.3 | 47.7 | 17.3 | 14.5 | 35.0 | 2.0 | 26.0 |
| GritLM-7B | 37.5 | 40.3 | 25.7 | 34.4 | 17.8 | 20.1 | 32.4 | 0.0 | 26.0 |
| e5-mistral-7b-instruct | 29.9 | 36.3 | 26.2 | 46.7 | 17.3 | 14.5 | 32.2 | 1.1 | 25.5 |
| voyage-large-2-instruct | 34.4 | 35.4 | 26.7 | 41.6 | 12.9 | 12.8 | 31.1 | 1.3 | 24.5 |
| (Google) text-embedding-preview0409 | 30.9 | 38.0 | 21.9 | 30.7 | 12.9 | 19.2 | 25.7 | 0.3 | 22.4 |
| (OpenAI) text-embedding-3-large | 32.1 | 31.4 | 23.8 | 34.2 | 11.9 | 10.7 | 26.3 | 0.0 | 21.3 |
| Cohere-embed-english-v3.0 | 31.5 | 34.5 | 18.9 | 20.5 | 9.9 | 15.8 | 15.2 | 0.8 | 18.4 |
| SBERT | 25.6 | 34.1 | 18.9 | 15.8 | 10.9 | 15 | 18 | 1.2 | 17.4 |
| bge-large-en-v1.5 | 16.4 | 27.7 | 20.9 | 11.6 | 10.9 | 13.3 | 16.9 | 0.4 | 14.8 |
| BM25 | 10.7 | 15.4 | 10.7 | 8.4 | 7.4 | 22.2 | 10.7 | 5.4 | 11.4 |

4、**Retrieval Pipelines on Long Documents**
| Model | Bio. | Earth. | Econ. | Psy. | Rob. | Stack. | Sus. | Pony | Avg | 
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **MRE-T1-Pipeline (FAL + T1)** | **77.1** | 59.0 | **71.2** | 73.8 | 46.0 | **35.5** | **70.6** | **14.6** | **56.0** |
| INF-X-Retriever (inf+retrieve) | 73.2 | **59.6** | 69.3 | **74.3** | **55.9** | 27.8 | 64.8 | 12.0 | 54.6 |
