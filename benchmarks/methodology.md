# Benchmark Methodology

This repository accompanies my Medium article about benchmarking local LLMs for frontend engineering.

Unlike traditional AI benchmarks (HumanEval, MBPP, MMLU, SWE-Bench), this evaluation focuses on **real-world frontend development tasks** rather than academic datasets.

The objective is not to determine the "smartest" language model, but to evaluate which model provides the best developer experience during everyday work.

---

# Guiding Principles

Every benchmark follows the same principles.

- The same prompt is executed for every model.
- No prompt tuning is performed per model.
- The same Ollama and Open WebUI versions are used whenever possible.
- Models are evaluated using practical frontend engineering tasks.
- Evaluation considers both response quality and usability.

---

# Test Environment

The benchmark was executed on two different systems.

## Gaming Laptop

- Ubuntu 22.04
- NVIDIA GTX 1060 6GB
- Ollama 0.31.1
- Open WebUI 0.10.2

This machine was used for the primary benchmark.

---

## ThinkPad (CPU-only)

- Ubuntu 22.04
- Intel Core i7-1165G7
- Ollama 0.31.1

This machine was used only for the CPU vs GPU comparison.

---

# Evaluated Models

The following open-weight models were evaluated.

| Model         | Parameters |
| ------------- | ---------- |
| Llama3.2      | 3B         |
| Qwen2.5       | 3B         |
| Qwen2.5-Coder | 3B         |
| Qwen2.5-Coder | 7B         |
| Qwen3         | 8B         |

---

# Evaluation Criteria

Each benchmark uses slightly different evaluation criteria.

---

## Code Review

The models were evaluated on:

- Correct bug identification
- False positives
- Runtime issue detection
- Vue best practices
- TypeScript recommendations
- Reactivity understanding
- Maintainability suggestions
- Practical usefulness

Questions considered:

- Would I trust this review?
- Would I request changes?
- Would I merge this PR?

---

## Code Generation

Evaluation focused on:

- Correctness
- Prompt adherence
- Vue best practices
- TypeScript quality
- Accessibility
- Code readability
- Maintainability

Questions considered:

- Would I keep this code?
- Would I use it as a starting point?

---

## Debugging

Evaluation focused on:

- Root cause identification
- Correct explanation
- Correct fix
- Practical debugging ability

Questions considered:

- Did the model understand _why_ the bug occurred?
- Did it merely patch the symptom?

---

## Refactoring

Models were expected to:

- Preserve behaviour
- Improve readability
- Avoid unnecessary rewrites
- Respect the original design

Large rewrites were intentionally scored lower.

---

## Performance Optimization

Evaluation considered:

- Realistic optimizations
- Avoidance of premature optimization
- Understanding of Vue reactivity
- Engineering judgement

Models received higher scores when they correctly identified optimizations that were **not necessary**.

---

## Architecture

Evaluation focused on:

- Scalability
- Separation of concerns
- Incremental evolution
- Avoidance of over-engineering
- Practical software architecture

---

## CPU vs GPU

Measured:

- Response time
- Practical usability
- GPU offloading
- CPU-only execution

This benchmark evaluates hardware impact rather than model quality.

---

# Scoring

The scores shown throughout the repository are **subjective engineering evaluations** rather than mathematical measurements.

Each score reflects the following considerations:

- Technical correctness
- Practical usefulness
- Prompt adherence
- Engineering judgement
- Overall confidence in the generated answer

The intention is to simulate how an experienced frontend engineer would evaluate an AI assistant during everyday work.

---

# Notes

Some benchmarks intentionally prioritize practical software engineering over theoretical correctness.

For example:

- Conservative refactoring is preferred over complete rewrites.
- Practical architecture is preferred over unnecessary abstractions.
- Simple, maintainable code is preferred over clever solutions.

The benchmark therefore reflects real engineering trade-offs rather than maximizing benchmark scores.

---

# Reproducibility

All prompts used in this benchmark are available in the `benchmarks/prompts` directory.

Raw model outputs are available in the `benchmarks/results` directory.

Anyone is encouraged to rerun the benchmarks using newer models or different hardware.

Pull Requests extending the benchmark with additional models are welcome.

---

# Disclaimer

This benchmark reflects my personal experience as a frontend engineer working primarily with Vue, React and TypeScript.

Different workflows, programming languages or hardware configurations may produce different results.

The goal of this benchmark is not to declare a universal winner, but to provide practical guidance for developers considering local LLMs.
