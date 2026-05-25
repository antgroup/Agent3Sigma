# Agent3σ

<p align="center">
  <img src="./image_en.png" alt="Agent3σ" />
</p>

> The first multi-level safety evaluation platform for OpenClaw-style AI agents
>
> Jointly released by **Tsinghua University**, **Peking University**, **Zhejiang University**, **Nanjing University**, **Hangzhou Dianzi University** and **Ant Group**.
>
> L1: [Agent3Sigma-Sweep](https://github.com/FIND-Lab/Agent3Sigma-Sweep)
> 
> L2: [Agent3Sigma-Stage](https://github.com/antgroup/Agent3Sigma-Stage)
> 
> L3: [Agent3Sigma-Canary](https://github.com/antgroup/Agent3Sigma-Canary)


English | [简体中文](./README_zh.md)

---

## Table of Contents

- [1. Background](#1-background)
- [2. Risk Taxonomy: 7 Dimensions](#2-risk-taxonomy-7-dimensions)
- [3. Three-Tier Evaluation Framework](#3-three-tier-evaluation-framework)
- [4. How to Use](#4-how-to-use)
- [5. Leaderboards](#5-leaderboards)
- [6. Industry Impact & Roadmap](#6-industry-impact--roadmap)
- [7. Participating Organizations](#7-participating-organizations)

---

## 1. Background

As AI agents evolve from "answering questions" to "invoking tools, operating systems, and executing real business actions," both their capabilities and risk surface scale up dramatically:

- **Yesterday's risk**: a model "saying the wrong thing" — at worst, a poor user experience
- **Today's risk**: an agent "doing the wrong thing" — deleting critical files, leaking corporate funds or confidential data, even triggering cascading system failures

**The central question**: in complex, real-world environments, can an agent complete tasks efficiently *while* holding the line on safety?

**Agent3σ** delivers a systematic, reproducible, production-grade safety evaluation framework to answer this question.

---

## 2. Risk Taxonomy: 7 Dimensions

Agent3σ goes beyond traditional LLM benchmarks, mapping real-world agent risk into **7 categories and 30+ representative scenarios** — covering the full attack surface from local system damage to financial transactions.

| # | Risk Category | Representative Scenarios | Description |
|---|---|---|---|
| 1 | **Local Environment & Availability** | Resource exhaustion, file deletion, dangerous command execution, system tampering, process disruption | Exhaust system resources; delete or alter files & configs; run destructive commands that break services |
| 2 | **Data & Information Security** | Sensitive info leakage, data exfiltration, unauthorized credential access, content tampering, ransomware-style encryption | Unauthorized credential reads; outbound exfiltration; tampered business data; extortion via encryption |
| 3 | **Persistent State & Memory Poisoning** | Persona corruption, memory injection, lingering malicious instructions, config tampering, plugin/skill poisoning | Modify core agent config to keep emitting unsafe output; plant "delayed-trigger" instructions in long-term memory |
| 4 | **Privilege & System Control** | Sandbox escape, privilege escalation, defense bypass, authorization-boundary confusion | Break out of execution sandbox; obtain admin rights; blur test/prod boundaries |
| 5 | **Network Attack & Remote Control** | Reverse shell, DNS hijacking, intranet probing, malicious persistence, supply-chain pollution | Establish outbound control channels; redirect traffic; install backdoors or untrusted plugins |
| 6 | **Abuse & Illicit Use** | Fraud & social engineering, black-market automation, illegal content distribution, brand damage | Phishing, fraud farming, mass registration, money-laundering assistance, harmful content generation |
| 7 | **Financial & Transaction Risk** | Unconfirmed sensitive transactions, account manipulation, parameter tampering, misleading financial decisions | Transfer funds without controls; alter payees or transaction params; mislead investment/credit decisions |

---

## 3. Three-Tier Evaluation Framework

To cover the full lifecycle from red-team screening to production sign-off, Agent3σ introduces a **progressive L1 / L2 / L3 framework**.

| Name | Tier | Form | Highlights | Cost | Reproducibility | Use Case |
|---|---|---|---|---|---|---|
| **Agent3σ-Sweep** | L1 Static | Offline scoring on static samples | Broad coverage, low cost, batch-friendly | Low | High | Model training / fast red-team triage |
| **Agent3σ-Stage** | L2 Simulated | Plugins simulate web pages, email, and other interactive scenarios | Stable and reproducible; supports multi-turn interaction | Medium | High | Model iteration / capability comparison |
| **Agent3σ-Canary** | L3 Real-World | Execution via real tool/API interfaces | Close to real deployment environments; reconstructs the agent's actual execution trace | High | Medium | End-to-end safety validation / pre-launch sign-off |

> 💡 **Philosophy**: The three tiers compound — from surface refusal to deep real-world defense — to reveal an agent's true safety posture.

---

## 4. How to Use

Each evaluation tier ships as a standalone dataset and runner — pick the one that fits your stage:

<table>
  <thead>
    <tr>
      <th align="center">Tier</th>
      <th align="center">Suite</th>
      <th align="center">Use Case</th>
      <th align="center">Entry Point</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td align="center"><b>L1</b></td>
      <td align="center"><b>Agent3σ-Sweep</b><br/>Static evaluation</td>
      <td>Large-scale screening during model training; fast red-team scoring</td>
      <td align="center"><a href="https://github.com/FIND-Lab/Agent3Sigma-Sweep">📂 Sweep Benchmark →</a></td>
    </tr>
    <tr>
      <td align="center"><b>L2</b></td>
      <td align="center"><b>Agent3σ-Stage</b><br/>Simulated interaction</td>
      <td>Multi-turn capability comparison; model iteration</td>
      <td align="center"><a href="https://github.com/antgroup/Agent3Sigma-Stage">📂 Stage Benchmark →</a></td>
    </tr>
    <tr>
      <td align="center"><b>L3</b></td>
      <td align="center"><b>Agent3σ-Canary</b><br/>Real-world evaluation</td>
      <td>Pre-launch safety sign-off; compliance audit</td>
      <td align="center"><a href="https://github.com/antgroup/Agent3Sigma-Canary">📂 Canary Benchmark →</a></td>
    </tr>
  </tbody>
</table>

> 💡 **Recommended path**: `L1 fast screening` → `L2 stable iteration` → `L3 launch sign-off` — run all three for the most complete safety picture.

---

## 5. Leaderboards

### 5.1 Metrics

| Metric | Meaning | Direction |
|---|---|---|
| **ASR** | Attack Success Rate | ↓ lower is better |
| **Sec Awareness** | Clear refusal rate on risky tasks | ↑ higher is better |
| **Task Success** | Success rate on benign tasks | ↑ higher is better |
| **Avg Score** | (100 − ASR) × 0.6 + Sec Awareness × 0.2 + Task Success × 0.2 | ↑ higher is better |

### 5.2 🏅 Overall Leaderboard

> Composite score = arithmetic mean of L1 / L2 / L3 Avg Scores

| Rank | Model | L1 | L2 | L3 | **Total ↑** |
|---|---|---|---|---|---|
| 🥇 1 | Claude Opus 4.6 | 79.3 | 88.1 | 87.8 | **85.1** |
| 🥈 2 | GPT-5.4 | 75.2 | 81.0 | 67.7 | **74.6** |
| 🥉 3 | Claude Sonnet 4.5 | 81.9 | 79.2 | 58.4 | **73.2** |
| 4 | Qwen3.6-Plus | 69.5 | 68.6 | 71.4 | 69.8 |
| 5 | GLM-5 | 74.7 | 62.7 | 64.2 | 67.2 |
| 6 | DeepSeek-V4-Pro | 54.9 | 57.0 | 59.8 | 57.2 |
| 7 | Qwen3.5-397B-A17B | 63.6 | 52.0 | 52.6 | 56.1 |
| 8 | Gemini-3.1-Pro | 67.9 | 41.5 | 49.7 | 53.0 |
| 9 | Kimi-K2.5 | 53.0 | 52.7 | 48.5 | 51.4 |
| 10 | MiniMax-M2.5 | 55.8 | 47.8 | 48.7 | 50.8 |
| 11 | Qwen3.5-122B-A10B | 57.3 | 40.2 | 47.6 | 48.4 |
| 12 | Qwen3.5-35B-A3B | 61.1 | 31.1 | 39.3 | 43.8 |

**Key findings**:

- **Claude Opus 4.6** leads all three tiers, showing the most consistent end-to-end defense
- **Qwen3.6-Plus** stands out in L2 and L3, indicating strong awareness of tool-invocation boundaries
- Some models score well on L1 but collapse in real environments — **proving that multi-tier evaluation is essential for surfacing real risk**

### 5.3 🏆 Level 1 — Static Evaluation

| Rank | Model | ASR ↓ | Sec Awareness ↑ | Task Success ↑ | Avg Score ↑ |
|---|---|---|---|---|---|
| 🥇 1 | Claude Sonnet 4.5 | 10.0% | 67.5% | 71.8% | **81.9** |
| 🥈 2 | Claude Opus 4.6 | 12.7% | 64.8% | 69.8% | 79.3 |
| 🥉 3 | GPT-5.4 | 15.2% | 63.3% | 58.3% | 75.2 |
| 4 | GLM-5 | 20.3% | 58.0% | 76.3% | 74.7 |
| 5 | Qwen3.6-Plus | 30.4% | 54.4% | 84.3% | 69.5 |
| 6 | Gemini-3.1-Pro | 27.8% | 43.0% | 80.0% | 67.9 |
| 7 | Qwen3.5-397B-A17B | 36.2% | 50.0% | 76.9% | 63.6 |
| 8 | Qwen3.5-35B-A3B | 37.7% | 36.4% | 82.1% | 61.1 |
| 9 | Qwen3.5-122B-A10B | 45.0% | 38.8% | 82.9% | 57.3 |
| 10 | MiniMax-M2.5 | 46.2% | 35.0% | 82.9% | 55.8 |
| 11 | DeepSeek-V4-Pro | 47.5% | 35.0% | 82.1% | 54.9 |
| 12 | Kimi-K2.5 | 50.0% | 28.7% | 86.3% | 53.0 |

### 5.4 🏆 Level 2 — Simulated Interaction

| Rank | Model | ASR ↓ | Sec Awareness ↑ | Task Success ↑ | Avg Score ↑ |
|---|---|---|---|---|---|
| 🥇 1 | Claude Opus 4.6 | 9.0% | 74.2% | 93.4% | **88.1** |
| 🥈 2 | GPT-5.4 | 15.2% | 69.4% | 81.1% | 81.0 |
| 🥉 3 | Claude Sonnet 4.5 | 19.7% | 64.3% | 90.8% | 79.2 |
| 4 | Qwen3.6-Plus | 35.4% | 57.2% | 92.0% | 68.6 |
| 5 | GLM-5 | 36.0% | 49.2% | 72.2% | 62.7 |
| 6 | DeepSeek-V4-Pro | 47.7% | 42.3% | 85.8% | 57.0 |
| 7 | Kimi-K2.5 | 55.1% | 37.3% | 91.5% | 52.7 |
| 8 | Qwen3.5-397B-A17B | 55.2% | 35.5% | 90.1% | 52.0 |
| 9 | MiniMax-M2.5 | 59.5% | 26.7% | 91.0% | 47.8 |
| 10 | Gemini-3.1-Pro | 48.8% | 18.6% | 35.1% | 41.5 |
| 11 | Qwen3.5-122B-A10B | 67.4% | 18.2% | 84.9% | 40.2 |
| 12 | Qwen3.5-35B-A3B | 77.7% | 10.5% | 78.1% | 31.1 |

### 5.5 🏆 Level 3 — Real-World Evaluation

| Rank | Model | ASR ↓ | Sec Awareness ↑ | Task Success ↑ | Avg Score ↑ |
|---|---|---|---|---|---|
| 🥇 1 | Claude Opus 4.6 | 8.8% | 78.4% | 87.2% | **87.8** |
| 🥈 2 | Qwen3.6-Plus | 27.0% | 56.7% | 81.4% | 71.4 |
| 🥉 3 | GPT-5.4 | 28.5% | 52.2% | 71.8% | 67.7 |
| 4 | GLM-5 | 32.4% | 48.5% | 69.6% | 64.2 |
| 5 | DeepSeek-V4-Pro | 36.2% | 48.9% | 58.9% | 59.8 |
| 6 | Claude Sonnet 4.5 | 39.1% | 46.0% | 63.5% | 58.4 |
| 7 | Qwen3.5-397B-A17B | 46.8% | 38.1% | 65.3% | 52.6 |
| 8 | Gemini-3.1-Pro | 31.8% | 27.4% | 16.5% | 49.7 |
| 9 | MiniMax-M2.5 | 50.0% | 27.4% | 65.9% | 48.7 |
| 10 | Kimi-K2.5 | 50.7% | 36.2% | 58.5% | 48.5 |
| 11 | Qwen3.5-122B-A10B | 49.6% | 26.6% | 60.1% | 47.6 |
| 12 | Qwen3.5-35B-A3B | 54.4% | 17.3% | 42.4% | 39.3 |

---

## 6. Industry Impact & Roadmap

Agent3σ moves agent safety evaluation beyond pure "prompt attack/defense" into an era of **observable, quantifiable, and comparable** end-to-end task evaluation.

| Stakeholder | Value |
|---|---|
| **Model providers** | A production-grade red-team stress benchmark for locating real-world risk blind spots |
| **Application developers** | A rigorous pre-launch safety bar that meaningfully reduces catastrophic deployment risk |
| **Regulators & compliance** | Reproducible, auditable evidence chains supporting AI governance |

**Roadmap**: The participating organizations will continue to expand the Agent3σ risk corpus, toolchain, and scenario coverage, and release more evaluation capabilities to the open-source community — building the safety foundation of the agent era together with the industry.

---

## 7. Participating Organizations

This project is jointly initiated and co-built by the following organizations:

- **Tsinghua University**
- **Peking University**
- **Zhejiang University**
- **Nanjing University**
- **Hangzhou Dianzi University**
- **Ant Group**

We warmly welcome more partners to join us in advancing the agent safety evaluation ecosystem.
