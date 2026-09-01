# Day 01/100: Prompt Engineering Patterns: System, user, assistant roles, and few-shot formatting

**Date:** 2026-08-31
**Tags:** #ai #prompengineering

## Summary
Prompt engineering is the practice of designing inputs to a language model so it produces the most accurate, useful, and consistent output possible. It matters because the same underlying model can perform wildly differently depending on how a task is framed, structured, or constrained. At its core, prompting has two channels: the system prompt, which sets persistent behavior like persona, tone, and rules, and the user prompt, which carries the specific task for that turn. A basic technique is zero-shot prompting, where you simply describe the task, versus few-shot prompting, where you include a handful of examples to show the model the pattern you want.

For tasks involving multi-step logic, chain-of-thought prompting — asking the model to reason step-by-step before answering — often improves accuracy, though it adds cost and isn't needed for simple questions. Assigning a role ("act as a...") can prime the model's tone and expertise, while format constraints (JSON, tables, word limits) make outputs easy to parse downstream. When prompts mix fixed instructions with variable data — like user text or retrieved documents — delimiters such as XML tags or quotes are used to clearly separate the two, which reduces ambiguity and helps prevent prompt injection.

As prompting scales beyond one-off use, templates with variables let the same prompt "shape" be reused across many inputs. Because prompts drift and get tweaked constantly, prompt versioning treats prompt text like code — tracked, tested, and reversible if a change regresses quality. For complex tasks, prompt chaining breaks the work into a sequence of smaller, focused calls rather than one giant prompt, which tends to improve reliability since each step has a narrower job. Grounding — feeding the model real source material and instructing it to answer from that rather than its own memory — is one of the most effective ways to reduce hallucination, and is the foundation of retrieval-augmented generation (RAG). Finally, prompt caching reuses the processed version of repeated static content (long system prompts, reference documents) across calls to cut latency and cost. Together, these techniques form the practical toolkit for getting reliable, production-grade behavior out of an LLM rather than treating it as a black box you just talk to.
## References
[A Prompt Pattern Catalog to Enhance Prompt Engineering with ChatGPT]Prompt Engineering Guide — DAIR.AI (https://github.com/dair-ai/Prompt-Engineering-Guide)
Anthropic Prompt Engineering Docs (https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview)
Anthropic Interactive Prompting Tutorial (https://github.com/anthropics/prompt-eng-interactive-tutorial)
Claude Cookbooks (formerly anthropic-cookbook) (https://github.com/anthropics/claude-cookbooks)

---

