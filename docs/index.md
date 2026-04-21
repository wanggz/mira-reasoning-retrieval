<h1 align="center">⚡ Mira-Reasoning-Retrieval</h1>

<p align="center">
  <strong>A Reasoning-Intensive Retrieval Pipeline: GRPO-Aligned Query Rewriting (MQR-A1) Meets Reasoning-Intensive Retrieval Model (MRE-T1)</strong>
</p>

<p align="center">
  <a href="https://brightbenchmark.github.io/"><img src="https://img.shields.io/badge/BRIGHT_Benchmark-Rank_1st-8A2BE2" alt="Rank"></a>
  <a href="https://huggingface.co/ForwardAILabs/MRE-T1"><img src="https://img.shields.io/badge/🤗_HuggingFace-MRE--T1-yellow" alt="Retriever"></a>
  <a href="https://huggingface.co/ForwardAILabs/MQR-A1"><img src="https://img.shields.io/badge/🤗_HuggingFace-MQR--A1-orange" alt="Rewriter"></a>
  <a href="https://www.mira.day/"><img src="https://img.shields.io/badge/Forward_AI_Labs-mira.day-green" alt="Company"></a>
  <a href="#"><img src="https://img.shields.io/badge/Pipeline-YAML_Driven-blue" alt="Pipeline"></a>
  <a href="#"><img src="https://img.shields.io/badge/Training-GRPO_Aligned-red" alt="Training"></a>
</p>

## 📖 Overview

**MQR-A1** represents a paradigm shift in reasoning-intensive retrieval tasks. Conventional models and agentic pipelines rely heavily on query expansion and are prone to the superficial semantic trap—a scenario where negative samples exhibit high surface-level textual similarity to queries yet are logically mismatched.
* Core mechanism: A multi-dimensional GRPO (Group Relative Policy Optimization) training framework.
* Technical objective: To distill core retrieval intents, filter misleading noise, and achieve high-precision alignment with the MRE-T1 retriever.
* Industrial standing: Currently ranks first on the BRIGHT Benchmark across both long-document and short-document tracks, outperforming sophisticated rerankers and existing alignment models.

---

## 🚀 Highlights

The **MQR-A1** shifts the retrieval paradigm from simple additive expansion to reinforcement-learning-driven intent distillation:

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

**MQR-A1 + MRE-T1** achieves state-of-the-art results on the rigorous BRIGHT Benchmark. The system demonstrates categorical dominance by securing the No. 1 position across all evaluated dimensions, including both short and long document tracks. Notably, our framework outperforms both existing query alignment models and complex, resource-heavy agentic pipelines, establishing a new performance frontier in reasoning-intensive retrieval.

1、**Single-model Methods on Short Documents**

<table>
<thead>
<tr>
  <th align="left">Model</th>
  <th align="center">Bio.</th>
  <th align="center">Earth.</th>
  <th align="center">Econ.</th>
  <th align="center">Psy.</th>
  <th align="center">Rob.</th>
  <th align="center">Stack.</th>
  <th align="center">Sus.</th>
  <th align="center">Leet.</th>
  <th align="center">Pony</th>
  <th align="center">AoPS</th>
  <th align="center">TheoQ.</th>
  <th align="center">TheoT.</th>
  <th align="center">Avg</th>
</tr>
</thead>
<tbody>
<tr>
  <td align="left">MRE-T1</td>
  <td align="center">55.3</td>
  <td align="center">56.5</td>
  <td align="center">32.9</td>
  <td align="center"><b>48.2</b></td>
  <td align="center">33.1</td>
  <td align="center">34.2</td>
  <td align="center">37.3</td>
  <td align="center">35.0</td>
  <td align="center"><b>35.5</b></td>
  <td align="center"><b>16.7</b></td>
  <td align="center"><b>43.3</b></td>
  <td align="center">46.9</td>
  <td align="center"><b>39.6</b></td>
</tr>
<tr>
  <td align="left">llama-nv-embed-reasoning-3b</td>
  <td align="center"><b>63.4</b></td>
  <td align="center"><b>60.2</b></td>
  <td align="center"><b>39.5</b></td>
  <td align="center">45.5</td>
  <td align="center">32.6</td>
  <td align="center">34.0</td>
  <td align="center"><b>43.3</b></td>
  <td align="center">37.5</td>
  <td align="center">15.0</td>
  <td align="center">10.5</td>
  <td align="center">39.5</td>
  <td align="center">38.5</td>
  <td align="center">38.3</td>
</tr>
<tr>
  <td align="left">ReasonEmbed-Qwen3-8B-0928</td>
  <td align="center">55.5</td>
  <td align="center">56.6</td>
  <td align="center">36.2</td>
  <td align="center">47.4</td>
  <td align="center"><b>35.3</b></td>
  <td align="center"><b>36.6</b></td>
  <td align="center">39.1</td>
  <td align="center">33.6</td>
  <td align="center">16.4</td>
  <td align="center">12.5</td>
  <td align="center">41.4</td>
  <td align="center"><b>47.2</b></td>
  <td align="center">38.2</td>
</tr>
<tr>
  <td align="left">ReasonEmbed-Qwen3-4B-0928</td>
  <td align="center">55.4</td>
  <td align="center">54.5</td>
  <td align="center">34.9</td>
  <td align="center">46.9</td>
  <td align="center">34.0</td>
  <td align="center">36.1</td>
  <td align="center">37.4</td>
  <td align="center">34.5</td>
  <td align="center">13.6</td>
  <td align="center">11.3</td>
  <td align="center">41.4</td>
  <td align="center">45.1</td>
  <td align="center">37.1</td>
</tr>
<tr>
  <td align="left">Seed-1.5-Embedding</td>
  <td align="center">34.8</td>
  <td align="center">46.9</td>
  <td align="center">23.4</td>
  <td align="center">31.6</td>
  <td align="center">19.1</td>
  <td align="center">25.4</td>
  <td align="center">21.0</td>
  <td align="center"><b>43.2</b></td>
  <td align="center">4.9</td>
  <td align="center">12.2</td>
  <td align="center">33.3</td>
  <td align="center">30.5</td>
  <td align="center">27.2</td>
</tr>
<tr>
  <td align="left">inf-retriever-v1-pro</td>
  <td align="center">37.8</td>
  <td align="center">39.7</td>
  <td align="center">26.2</td>
  <td align="center">34.4</td>
  <td align="center">20.1</td>
  <td align="center">22.6</td>
  <td align="center">26.3</td>
  <td align="center">38.3</td>
  <td align="center">1.9</td>
  <td align="center">13.6</td>
  <td align="center">30.5</td>
  <td align="center">25.5</td>
  <td align="center">26.4</td>
</tr>
<tr>
  <td align="left">ReasonIR-8B</td>
  <td align="center">26.2</td>
  <td align="center">31.4</td>
  <td align="center">23.3</td>
  <td align="center">30.0</td>
  <td align="center">18.0</td>
  <td align="center">23.9</td>
  <td align="center">20.5</td>
  <td align="center">35.0</td>
  <td align="center">10.5</td>
  <td align="center">14.7</td>
  <td align="center">31.9</td>
  <td align="center">27.2</td>
  <td align="center">24.4</td>
</tr>
<tr>
  <td align="left">Qwen3-Embedding-8B</td>
  <td align="center">21.0</td>
  <td align="center">33.8</td>
  <td align="center">18.6</td>
  <td align="center">27.8</td>
  <td align="center">15.6</td>
  <td align="center">18.6</td>
  <td align="center">17.1</td>
  <td align="center">33.5</td>
  <td align="center">1.2</td>
  <td align="center">9.5</td>
  <td align="center">40.6</td>
  <td align="center">39.6</td>
  <td align="center">23.1</td>
</tr>
<tr>
  <td align="left">bm25</td>
  <td align="center">18.9</td>
  <td align="center">27.2</td>
  <td align="center">14.9</td>
  <td align="center">12.5</td>
  <td align="center">13.6</td>
  <td align="center">18.4</td>
  <td align="center">15.0</td>
  <td align="center">24.4</td>
  <td align="center">7.9</td>
  <td align="center">6.2</td>
  <td align="center">10.4</td>
  <td align="center">4.9</td>
  <td align="center">14.5</td>
</tr>
<tr>
  <td align="left">bge-large-en-v1.5</td>
  <td align="center">1.7</td>
  <td align="center">24.6</td>
  <td align="center">16.6</td>
  <td align="center">17.5</td>
  <td align="center">11.7</td>
  <td align="center">10.8</td>
  <td align="center">13.3</td>
  <td align="center">26.7</td>
  <td align="center">5.7</td>
  <td align="center">6.0</td>
  <td align="center">13.0</td>
  <td align="center">6.9</td>
  <td align="center">13.7</td>
</tr>
</tbody>
</table>

2、**Retrieval Pipelines on Short Documents**

<table>
<thead>
<tr>
  <th align="left">Model</th>
  <th align="center">Bio.</th>
  <th align="center">Earth.</th>
  <th align="center">Econ.</th>
  <th align="center">Psy.</th>
  <th align="center">Rob.</th>
  <th align="center">Stack.</th>
  <th align="center">Sus.</th>
  <th align="center">Leet.</th>
  <th align="center">Pony</th>
  <th align="center">AoPS</th>
  <th align="center">TheoQ.</th>
  <th align="center">TheoT.</th>
  <th align="center">Avg</th>
</tr>
</thead>
<tbody>
<tr>
  <td align="left"><b>MRE-T1-Pipeline (MQR-A1 + T1)</b></td>
  <td align="center"><b>86.7</b></td>
  <td align="center"><b>78.5</b></td>
  <td align="center">69.7</td>
  <td align="center"><b>78.2</b></td>
  <td align="center"><b>58.4</b></td>
  <td align="center"><b>67.0</b></td>
  <td align="center"><b>65.9</b></td>
  <td align="center">46.8</td>
  <td align="center"><b>73.4</b></td>
  <td align="center">45.2</td>
  <td align="center"><b>60.6</b></td>
  <td align="center"><b>72.3</b></td>
  <td align="center"><b>66.9</b></td>
</tr>
<tr>
  <td align="left">INF-X-Retriever (inf+retrieve)</td>
  <td align="center">79.8</td>
  <td align="center">70.9</td>
  <td align="center"><b>69.9</b></td>
  <td align="center">73.3</td>
  <td align="center">57.7</td>
  <td align="center">64.3</td>
  <td align="center">61.9</td>
  <td align="center"><b>56.1</b></td>
  <td align="center">54.5</td>
  <td align="center"><b>51.9</b></td>
  <td align="center">53.1</td>
  <td align="center">67.9</td>
  <td align="center">63.4</td>
</tr>
<tr>
  <td align="left">RakanEmb4B (inf+retrieve)</td>
  <td align="center">65.9</td>
  <td align="center">56.9</td>
  <td align="center">59.0</td>
  <td align="center">60.7</td>
  <td align="center">49.0</td>
  <td align="center">52.8</td>
  <td align="center">53.3</td>
  <td align="center">35.6</td>
  <td align="center">55.4</td>
  <td align="center">22.0</td>
  <td align="center">53.9</td>
  <td align="center">64.5</td>
  <td align="center">52.4</td>
</tr>
<tr>
  <td align="left">Nemo Retriever's Agentic Retrieval</td>
  <td align="center">72.8</td>
  <td align="center">66.0</td>
  <td align="center">48.7</td>
  <td align="center">59.6</td>
  <td align="center">52.5</td>
  <td align="center">47.1</td>
  <td align="center">50.2</td>
  <td align="center">49.3</td>
  <td align="center">42.1</td>
  <td align="center">21.0</td>
  <td align="center">53.3</td>
  <td align="center">48.0</td>
  <td align="center">50.9</td>
</tr>
<tr>
  <td align="left">DIVER-v3-GroupRank</td>
  <td align="center">66.0</td>
  <td align="center">63.7</td>
  <td align="center">42.4</td>
  <td align="center">55.0</td>
  <td align="center">40.6</td>
  <td align="center">44.7</td>
  <td align="center">50.4</td>
  <td align="center">32.5</td>
  <td align="center">47.3</td>
  <td align="center">17.2</td>
  <td align="center">46.4</td>
  <td align="center">55.6</td>
  <td align="center">46.8</td>
</tr>
<tr>
  <td align="left">BGE-Reasoner-0928</td>
  <td align="center">68.5</td>
  <td align="center">66.4</td>
  <td align="center">40.6</td>
  <td align="center">53.1</td>
  <td align="center">43.2</td>
  <td align="center">44.1</td>
  <td align="center">47.8</td>
  <td align="center">29.0</td>
  <td align="center">41.6</td>
  <td align="center">17.2</td>
  <td align="center">46.5</td>
  <td align="center">58.4</td>
  <td align="center">46.4</td>
</tr>
<tr>
  <td align="left">Lattice Hierarchical Retrieval</td>
  <td align="center">64.4</td>
  <td align="center">62.4</td>
  <td align="center">45.4</td>
  <td align="center">57.4</td>
  <td align="center">47.6</td>
  <td align="center">37.6</td>
  <td align="center">46.4</td>
  <td align="center">19.9</td>
  <td align="center">34.0</td>
  <td align="center">12.0</td>
  <td align="center">30.1</td>
  <td align="center">47.8</td>
  <td align="center">42.1</td>
</tr>
<tr>
  <td align="left">ReasonRank (rerank RaDer)</td>
  <td align="center">62.7</td>
  <td align="center">55.5</td>
  <td align="center">36.7</td>
  <td align="center">54.6</td>
  <td align="center">35.7</td>
  <td align="center">38.0</td>
  <td align="center">44.8</td>
  <td align="center">29.5</td>
  <td align="center">25.6</td>
  <td align="center">14.4</td>
  <td align="center">42.0</td>
  <td align="center">50.1</td>
  <td align="center">40.8</td>
</tr>
<tr>
  <td align="left">XRR2</td>
  <td align="center">63.1</td>
  <td align="center">55.4</td>
  <td align="center">38.5</td>
  <td align="center">52.9</td>
  <td align="center">37.1</td>
  <td align="center">38.2</td>
  <td align="center">44.6</td>
  <td align="center">21.9</td>
  <td align="center">35.0</td>
  <td align="center">15.7</td>
  <td align="center">34.4</td>
  <td align="center">46.2</td>
  <td align="center">40.3</td>
</tr>
<tr>
  <td align="left">RaDeR with Qwen reranking</td>
  <td align="center">58.0</td>
  <td align="center">59.2</td>
  <td align="center">33.0</td>
  <td align="center">49.4</td>
  <td align="center">31.8</td>
  <td align="center">39.0</td>
  <td align="center">36.4</td>
  <td align="center">33.5</td>
  <td align="center">33.3</td>
  <td align="center">10.8</td>
  <td align="center">34.2</td>
  <td align="center">51.6</td>
  <td align="center">39.2</td>
</tr>
<tr>
  <td align="left">ReasonIR with Rank-R1</td>
  <td align="center">59.5</td>
  <td align="center">55.1</td>
  <td align="center">37.9</td>
  <td align="center">52.7</td>
  <td align="center">30.0</td>
  <td align="center">39.3</td>
  <td align="center">45.1</td>
  <td align="center">32.1</td>
  <td align="center">17.1</td>
  <td align="center">10.7</td>
  <td align="center">40.4</td>
  <td align="center">45.6</td>
  <td align="center">38.8</td>
</tr>
</tbody>
</table>

3、**Single-model Methods on Long Documents**

<table>
<thead>
<tr>
  <th align="left">Model</th>
  <th align="center">Bio.</th>
  <th align="center">Earth.</th>
  <th align="center">Econ.</th>
  <th align="center">Psy.</th>
  <th align="center">Rob.</th>
  <th align="center">Stack.</th>
  <th align="center">Sus.</th>
  <th align="center">Pony</th>
  <th align="center">Avg</th>
</tr>
</thead>
<tbody>
<tr>
  <td align="left">MRE-T1</td>
  <td align="center"><b>46.5</b></td>
  <td align="center"><b>46</b></td>
  <td align="center"><b>34.5</b></td>
  <td align="center"><b>52.7</b></td>
  <td align="center"><b>27.7</b></td>
  <td align="center"><b>22.2</b></td>
  <td align="center"><b>45.2</b></td>
  <td align="center"><b>6.3</b></td>
  <td align="center"><b>35.1</b></td>
</tr>
<tr>
  <td align="left">Google-Gecko-Text_Embedding-004</td>
  <td align="center">-</td>
  <td align="center">-</td>
  <td align="center">-</td>
  <td align="center">-</td>
  <td align="center">-</td>
  <td align="center">-</td>
  <td align="center">-</td>
  <td align="center">-</td>
  <td align="center">33.2</td>
</tr>
<tr>
  <td align="left">inf-retriever-v1-pro</td>
  <td align="center">44.1</td>
  <td align="center">42.2</td>
  <td align="center">31.4</td>
  <td align="center">43.1</td>
  <td align="center">20.8</td>
  <td align="center">21.4</td>
  <td align="center">41.0</td>
  <td align="center">0.4</td>
  <td align="center">30.5</td>
</tr>
<tr>
  <td align="left">gte-Qwen1.5-7B-instruct</td>
  <td align="center">39.2</td>
  <td align="center">36.1</td>
  <td align="center">25.7</td>
  <td align="center">42.3</td>
  <td align="center">21.3</td>
  <td align="center">23.5</td>
  <td align="center">33.1</td>
  <td align="center">1.3</td>
  <td align="center">27.8</td>
</tr>
<tr>
  <td align="left">SFR-Embedding-Mistral</td>
  <td align="center">30.3</td>
  <td align="center">37.0</td>
  <td align="center">24.3</td>
  <td align="center">47.7</td>
  <td align="center">17.3</td>
  <td align="center">14.5</td>
  <td align="center">35.0</td>
  <td align="center">2.0</td>
  <td align="center">26.0</td>
</tr>
<tr>
  <td align="left">GritLM-7B</td>
  <td align="center">37.5</td>
  <td align="center">40.3</td>
  <td align="center">25.7</td>
  <td align="center">34.4</td>
  <td align="center">17.8</td>
  <td align="center">20.1</td>
  <td align="center">32.4</td>
  <td align="center">0.0</td>
  <td align="center">26.0</td>
</tr>
<tr>
  <td align="left">e5-mistral-7b-instruct</td>
  <td align="center">29.9</td>
  <td align="center">36.3</td>
  <td align="center">26.2</td>
  <td align="center">46.7</td>
  <td align="center">17.3</td>
  <td align="center">14.5</td>
  <td align="center">32.2</td>
  <td align="center">1.1</td>
  <td align="center">25.5</td>
</tr>
<tr>
  <td align="left">voyage-large-2-instruct</td>
  <td align="center">34.4</td>
  <td align="center">35.4</td>
  <td align="center">26.7</td>
  <td align="center">41.6</td>
  <td align="center">12.9</td>
  <td align="center">12.8</td>
  <td align="center">31.1</td>
  <td align="center">1.3</td>
  <td align="center">24.5</td>
</tr>
<tr>
  <td align="left">(Google) text-embedding-preview0409</td>
  <td align="center">30.9</td>
  <td align="center">38.0</td>
  <td align="center">21.9</td>
  <td align="center">30.7</td>
  <td align="center">12.9</td>
  <td align="center">19.2</td>
  <td align="center">25.7</td>
  <td align="center">0.3</td>
  <td align="center">22.4</td>
</tr>
<tr>
  <td align="left">(OpenAI) text-embedding-3-large</td>
  <td align="center">32.1</td>
  <td align="center">31.4</td>
  <td align="center">23.8</td>
  <td align="center">34.2</td>
  <td align="center">11.9</td>
  <td align="center">10.7</td>
  <td align="center">26.3</td>
  <td align="center">0.0</td>
  <td align="center">21.3</td>
</tr>
<tr>
  <td align="left">Cohere-embed-english-v3.0</td>
  <td align="center">31.5</td>
  <td align="center">34.5</td>
  <td align="center">18.9</td>
  <td align="center">20.5</td>
  <td align="center">9.9</td>
  <td align="center">15.8</td>
  <td align="center">15.2</td>
  <td align="center">0.8</td>
  <td align="center">18.4</td>
</tr>
<tr>
  <td align="left">SBERT</td>
  <td align="center">25.6</td>
  <td align="center">34.1</td>
  <td align="center">18.9</td>
  <td align="center">15.8</td>
  <td align="center">10.9</td>
  <td align="center">15</td>
  <td align="center">18</td>
  <td align="center">1.2</td>
  <td align="center">17.4</td>
</tr>
<tr>
  <td align="left">bge-large-en-v1.5</td>
  <td align="center">16.4</td>
  <td align="center">27.7</td>
  <td align="center">20.9</td>
  <td align="center">11.6</td>
  <td align="center">10.9</td>
  <td align="center">13.3</td>
  <td align="center">16.9</td>
  <td align="center">0.4</td>
  <td align="center">14.8</td>
</tr>
<tr>
  <td align="left">BM25</td>
  <td align="center">10.7</td>
  <td align="center">15.4</td>
  <td align="center">10.7</td>
  <td align="center">8.4</td>
  <td align="center">7.4</td>
  <td align="center">22.2</td>
  <td align="center">10.7</td>
  <td align="center">5.4</td>
  <td align="center">11.4</td>
</tr>
</tbody>
</table>

4、**Retrieval Pipelines on Long Documents**

<table>
<thead>
<tr>
  <th align="left">Model</th>
  <th align="center">Bio.</th>
  <th align="center">Earth.</th>
  <th align="center">Econ.</th>
  <th align="center">Psy.</th>
  <th align="center">Rob.</th>
  <th align="center">Stack.</th>
  <th align="center">Sus.</th>
  <th align="center">Pony</th>
  <th align="center">Avg</th>
</tr>
</thead>
<tbody>
<tr>
  <td align="left"><b>MRE-T1-Pipeline (MQR-A1 + T1)</b></td>
  <td align="center"><b>77.1</b></td>
  <td align="center">59.0</td>
  <td align="center"><b>71.2</b></td>
  <td align="center">73.8</td>
  <td align="center">46.0</td>
  <td align="center"><b>35.5</b></td>
  <td align="center"><b>70.6</b></td>
  <td align="center"><b>14.6</b></td>
  <td align="center"><b>56.0</b></td>
</tr>
<tr>
  <td align="left">INF-X-Retriever (inf+retrieve)</td>
  <td align="center">73.2</td>
  <td align="center"><b>59.6</b></td>
  <td align="center">69.3</td>
  <td align="center"><b>74.3</b></td>
  <td align="center"><b>55.9</b></td>
  <td align="center">27.8</td>
  <td align="center">64.8</td>
  <td align="center">12.0</td>
  <td align="center">54.6</td>
</tr>
</tbody>
</table>
