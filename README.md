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

# Day 02/100: Structured Outputs: Pydantic ⇄ TypeScript

**Date:** 2026-09-01

## Summary
What it is: Instead of letting an LLM return free-form text, you constrain its output to match a JSON Schema — guaranteeing a parseable, type-correct response every time. Pydantic (Python) and Zod (TypeScript) are the two main tools for defining that schema and validating data against it.

Pydantic (Python): Define a schema as a class; Pydantic auto-generates the JSON Schema and validates/parses raw LLM output into a typed object, raising an error on mismatch. Used directly as an Anthropic/OpenAI tool-call schema via model_json_schema().

Zod (TypeScript): TS types vanish at runtime, so they can't validate incoming data alone — Zod adds runtime validation, and TS types are inferred from the Zod schema (z.infer<...>), keeping one source of truth.

Key takeaway: enforce the schema at the boundary (right after the LLM call), not deep in application logic — catch malformed data at the source.

## References
Pydantic JSON Schema docs: https://docs.pydantic.dev/latest/concepts/json_schema/
Zod docs : [Zod docs](https://zod.dev/)
LangChain structured output guide : https://docs.langchain.com/oss/python/langchain/structured-output


----

# Day 03/100: Typesafe AI Pipelines: Using libraries like Instructor or BAML for type guarantees.

**Date:** 2026-09-02

## Summary
Typesafe AI pipelines = making LLM outputs behave like normal typed function returns instead of raw, unpredictable strings — so a bad/malformed response gets caught and retried instead of silently breaking your app.

Instructor (Python + Pydantic): You define a Pydantic model as the expected output shape. It patches your LLM client so calls return a real Pydantic object, and auto-retries with the validation error if the model gets it wrong. Low effort, drops right into an existing Python/LangChain stack.
BAML (a full DSL from Boundary): You write .baml files declaring the function's input, output type, and prompt. It compiles to type-safe client code in Python/TS/Ruby/etc. Calling an LLM feels like calling a normal typed function. More setup, but better parsing resilience and treats prompts as versioned, testable artifacts.

Bottom line: Instructor = quick, lightweight structured outputs for Python projects. BAML = heavier but more robust/production-grade approach when prompts and types need to be first-class, multi-language artifacts.



## References
https://docs.boundaryml.com/
https://github.com/instructor-ai/instructor

---------




# Day 04/100: Tokenomics & Cost Optimization — understanding how LLM usage translates into cost and latency, and the techniques to control both.

## Summary
Context window management: The context window is the max number of tokens (input + output) a model can process in one call. Bigger context = more cost and slower responses, so the skill is trimming what you send — summarizing prior conversation turns, retrieving only the most relevant chunks (this is where RAG comes in), and truncating/compressing long documents instead of dumping everything in.
Tokenizer math: Tokens aren't words — roughly 4 characters ≈ 1 token in English (varies by model/language). Since pricing is per-token (input and output priced differently, output usually costs more), estimating token counts before sending a request lets you predict cost and avoid hitting context limits. Libraries like tiktoken (OpenAI) let you count tokens programmatically before a call.
Caching strategies: Avoid re-processing/re-paying for repeated content.
Prompt caching (offered by OpenAI, Anthropic, etc.): if the same long system prompt or document prefix is reused across calls, the provider caches it server-side and charges a fraction of the price for the cached portion.
Semantic caching: store past query→response pairs (often in a vector DB) and reuse the response if a new query is semantically similar, skipping the LLM call entirely.
Response caching: simple key-value caching for identical repeated requests.

Bottom line: cost optimization = send only the tokens you need, know how to count them in advance, and reuse computation (via caching) instead of re-paying for the same context repeatedly.

## References
https://github.com/openai/tiktoken
https://platform.openai.com/tokenizer
https://platform.openai.com/docs/guides/prompt-caching

-------------
