# ChatGPT Custom Instructions

Evidence-informed Custom Instructions for ChatGPT, updated for the current GPT-5 generation and the 2026 Personalization limits.

Version 4 keeps the strongest idea from v3 — **task-specific self-reflection** — while removing rigid presentation rituals and adding explicit stopping and verification rules.

> [!IMPORTANT]
> OpenAI currently gives **Free and Go** users up to **1,500 characters**, while **Plus, Pro, Business, Enterprise, and Education** users get up to **5,000 characters**. Choose the version that matches your actual limit, not simply “free vs paid.” See the [official Custom Instructions FAQ](https://help.openai.com/en/articles/8096356-custom-instructions-for-chatgpt) and [July 15, 2026 release note](https://help.openai.com/en/articles/6825453-chatgpt-release-notes).

## Choose your v4

### ⚡ Free & Go — 1,500-character limit

**[→ v4 Compact: 1,455 characters](v4-free.md)**

Keeps the core behaviors that fit comfortably inside the smaller limit: task-specific rubric, material-weakness review, stopping rule, evidence preference, adaptive depth, and no mandatory answer ritual.

### 🧠 Plus / Pro / Business / Enterprise / Education — 5,000-character limit

**[→ v4 Extended: 3,581 characters](v4-5000.md)**

Adds stronger verification rules, context reuse, ambiguity handling, rewriting preservation, validation guidance, and task-adaptive formatting while leaving room for future edits.

---

## Why v4?

v3 introduced a useful pattern: ask the model to silently build a task-specific rubric and use it to improve the answer. That idea is not just prompt folklore. OpenAI's GPT-5 prompting material explicitly describes using a private **5–7 category rubric** for complex tasks. See the [OpenAI GPT-5 Prompting Guide](https://cookbook.openai.com/examples/gpt-5/gpt-5_prompting_guide) and its source in the public [openai/openai-cookbook repository](https://github.com/openai/openai-cookbook/blob/main/examples/gpt-5/gpt-5_prompting_guide.ipynb).

The newer GPT-5.6 guidance, however, pushes in another direction at the same time: **simplify prompts, keep success criteria and stopping conditions, remove redundant scaffolding, and validate with tools when possible**. See OpenAI's live [GPT-5.6 prompting best practices](https://developers.openai.com/api/docs/guides/model-guidance?model=gpt-5.6#prompting-best-practices).

v4 tries to combine both lessons.

### 1. Keep the rubric, remove the arbitrary `98/100`

The rubric stays because it gives reflection a concrete target. The exact `≥98/100` threshold from v3 is replaced with a practical criterion: **fix material weaknesses, then stop when further iteration is unlikely to materially improve the result**.

Why: self-refinement can help, but repeated self-correction without new evidence is not guaranteed to improve an already-correct answer.

- [Self-Refine: Iterative Refinement with Self-Feedback](https://huggingface.co/papers/2303.17651)
- [Large Language Models Cannot Self-Correct Reasoning Yet](https://huggingface.co/papers/2310.01798)

### 2. Replace infinite iteration with a stopping rule

`Keep going until solved with a best score` sounds ambitious but has no natural stopping condition. v4 explicitly stops when the important rubric dimensions are satisfied and more rewriting is unlikely to help.

That matches current GPT-5.6 guidance, which recommends clear **success criteria** and **stop rules** rather than unnecessary process scaffolding.

### 3. Remove the mandatory “world-famous PhD” announcement

v4 still allows the model to silently apply the standards of the relevant expert domain, but it no longer forces a visible persona, fictional prestige, or award into every new chat.

Expert personas can affect behavior and style, but evidence that they reliably increase factual accuracy is mixed; irrelevant persona detail can also hurt performance. See [When “A Helpful Assistant” Is Not Really Helpful: Personas in System Prompts](https://huggingface.co/papers/2508.19764).

### 4. Make TL;DR and step-by-step conditional

A TL;DR is useful for a long research answer and pointless for a two-sentence factual reply. Step-by-step decomposition is valuable for some reasoning and troubleshooting tasks but should not be mandatory presentation syntax for every answer.

v4 tells the model to use these structures **when they improve the result**, rather than because the template always demands them.

### 5. Prefer verification over more self-talk

When current sources, files, tools, calculations, tests, or other validation are available, v4 tells the model to use them. A second internal opinion from the same model is weaker evidence than an actual check.

The extended version makes this explicit for research, calculations, code, data analysis, and tool-based work.

### 6. Keep this universal

The v4 prompts contain **no contributor-specific profession, language preference, personality preset, memory contents, connected services, subscription-only tools, or personal writing style**. They are intended as general-purpose defaults. Features are referenced only conditionally — “when available.”

---

## Plan limits

OpenAI's current documentation says:

- **Free and Go:** up to **1,500 characters**
- **Plus, Pro, Business, Enterprise, Education:** up to **5,000 characters**

Source: [ChatGPT Custom Instructions — OpenAI Help Center](https://help.openai.com/en/articles/8096356-custom-instructions-for-chatgpt).

The 5,000-character expansion for the higher tiers was announced on July 15, 2026: [ChatGPT Release Notes](https://help.openai.com/en/articles/6825453-chatgpt-release-notes).

---

## Evaluation status

**v4 is evidence-informed, not yet benchmark-proven.**

The previous v3 MMLU-PRO run is preserved in the [v3 archive](v3.md), including its reported **70.20%** overall score and the author's note that a TL;DR-related grader bug caused some answers to be misclassified.

A fair next test should compare **baseline vs v3 vs v4 Compact vs v4 Extended** using:

- the same model and snapshot;
- the same reasoning effort;
- the same benchmark sample;
- the same decoding/settings;
- the same grader and parsing logic;
- raw outputs retained for inspection.

Until that test is run, v4 should be treated as a reasoned proposal rather than a claimed numerical improvement.

---

## How to apply

1. Open **ChatGPT → Settings → Personalization**.
2. Open **Custom Instructions**.
3. Choose [v4 Compact](v4-free.md) or [v4 Extended](v4-5000.md) based on your plan's character limit.
4. Copy only the prompt inside the code block and save it.
5. Start a new chat; Custom Instruction changes apply to future conversations.

---

## Acknowledgement

This repository and the original v1–v3 approach were created by **[Denis Shiryaev (@DenisSergeevitch)](https://github.com/DenisSergeevitch)**; the original project is [chatgpt-custom-instructions](https://github.com/DenisSergeevitch/chatgpt-custom-instructions).

I used the v3-style instructions for a long time and found the core self-reflection/rubric idea useful. Rather than replacing it wholesale, I eventually went back through the current OpenAI guidance and the research around self-refinement, self-correction, and persona prompting to see which parts still made sense. v4 is the result: preserve the strong core, remove arbitrary or overly rigid pieces, and make the prompt fit today's ChatGPT limits.

Thanks to [Denis](https://github.com/DenisSergeevitch) for publishing the original prompt, benchmarks, and iterations openly — they are what made this review possible.

---

## References

### OpenAI

- [GPT-5.6 prompting best practices](https://developers.openai.com/api/docs/guides/model-guidance?model=gpt-5.6#prompting-best-practices)
- [GPT-5 Prompting Guide — OpenAI Cookbook](https://cookbook.openai.com/examples/gpt-5/gpt-5_prompting_guide)
- [GPT-5 Prompting Guide — source on GitHub](https://github.com/openai/openai-cookbook/blob/main/examples/gpt-5/gpt-5_prompting_guide.ipynb)
- [ChatGPT Custom Instructions FAQ](https://help.openai.com/en/articles/8096356-custom-instructions-for-chatgpt)
- [ChatGPT Release Notes](https://help.openai.com/en/articles/6825453/chatgpt-release-notes)

### Research

- [Self-Refine: Iterative Refinement with Self-Feedback](https://huggingface.co/papers/2303.17651)
- [Large Language Models Cannot Self-Correct Reasoning Yet](https://huggingface.co/papers/2310.01798)
- [When “A Helpful Assistant” Is Not Really Helpful: Personas in System Prompts](https://huggingface.co/papers/2508.19764)

---

## Previous versions

- [v1](v1.md)
- [v2](v2.md)
- [v3 archive + MMLU-PRO results](v3.md)

## License

Feel free to use and modify these instructions for your own use.
