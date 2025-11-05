## A brief guide for those who slept on AI the last two years

### Rationale (who is this guide for?)

Over the last couple of years, I’ve noticed many great engineers haven’t really used modern AI tools, and now expect miracles because of the hype around them.  
This guide is my attempt to summarise what you realistically can and can’t do today with large language models (LLMs) and the tools around them,
and how to use them to boost your productivity (and learn something new in the process).

This is a condensed version;
if you prefer more verbose version, have a look at [the original document](./original.md).


### 1. What LLMs actually are
Large Language Models (LLMs) are probability machines: they predict the most likely next token (word piece) given your input and everything they’ve generated so far. That’s it.

They **do not** have built-in logic, beliefs, or a model of the world like humans do. You can *imitate* structure and reasoning by clearly describing rules, steps, patterns, or styles, and by giving examples — but underneath it’s still just pattern matching.

A plain LLM (without tools, search, code execution, etc.) is roughly like asking:  
> “What would a very well-read person blurt out first, without thinking too hard?”

It’s not an answer to your question.  
It’s a text which, with some luck, also happens to be an answer.

Your job is to provide structure, constraints, and reality checks.

### 2. How to talk to LLMs (prompting & context)

#### Be meaningfully verbose

If you just need the sodium entry in the periodic table, write exactly that:  
`"sodium periodic table"`.

If you need something more complex, add **only relevant and structured detail**:

- Prefer “only” over vague negatives:  
  - Good: “List only the song titles from album X by band Y.”  
  - Worse: “I don’t care about anything else.”

Avoid fuzzy language like “maybe”, “whatever you prefer”, “ideally” when you actually have a preference.

#### Don’t mix requests

Don’t bundle 3 tasks into one mega-prompt. Quality is usually higher when you:

1. Ask: “What are ways to do X?”
2. Pick an approach yourself.
3. Start a new message (or new chat) with a precise instruction for that one approach.
4. Only then ask for refinements.

If you’re unsure how to phrase something, you can simply ask:  
> “Help me craft a good prompt for this goal.”

#### Understand context and its limits

“Context” is the text the model sees at once (your instructions, previous messages, and sometimes hidden system rules). There’s a **maximum context window** (a token limit).

Even before you hit the hard limit, quality tends to degrade: the more text there is, the less attention goes to each detail — “context rotting”.

Practically:

- Ask about **one thing at a time**.
- Avoid huge rambly chats on many topics.
- When a conversation gets long but contains important info, export it, ask the model to summarize what’s relevant, and start a fresh chat with that summary.
- If the output you want would be enormous, rethink the task: split it up, or use tools like RAG (see below).

#### Stay on topic

Don’t use one chat for tax advice, fitness plans, and C++ templates. Topic hopping wastes tokens and applies the wrong context to the wrong question.

#### Match the style to the source you’d like

If you want something that feels like a scientific paper, **write your prompt in a scientific tone**. For technical docs, use calm, precise, jargon-appropriate language. If you talk like a YouTube comment section, you’ll get answers influenced by that style of text.

Also, resist the urge to treat the model as a buddy.  
A very common human bug is to treat anything that communicates with us as alive and to assign it human-like qualities. Don’t.

#### Use system-level instructions

Most tools let you set “global” rules (system prompts) for a chat or account:

- Tone and personality
- What to always do (“ask clarifying questions when needed”)
- What not to do (“don’t invent sources; say you don’t know”)

These work best when written plainly and firmly. You can even reuse good system prompts from public collections.

#### Provide examples and relevant context

If you want something “like this”, show “this”: snippets of code, fragments of documents, email examples, etc. But ruthlessly strip anything irrelevant — large, noisy context hurts.

Prompt engineering is useful even outside AI: it forces you to structure your thoughts. Often, while preparing a clear request, you stumble on the answer or at least a good plan (rubber-duck effect).

### 3. Extending LLMs beyond chat

You can’t “improve” the core model, but you can dramatically boost **usefulness** with tools around it.

Key ideas:

- **Pick the right model.** Some are better for code, some for generic writing, some for reasoning. Check current benchmarks, don’t rely on old impressions.
- **Reasoning models / chains.** Some systems let the model “talk to itself” or reason in multiple steps. It’s still probabilistic associations, but over more steps instead of one quick guess.
- **Multimodal models.** These can handle text plus images, sometimes audio or other formats. They can, for example, generate a web page, then look at the rendered result and comment on it.
- **Tool access (agents).** Give the model tools: a shell, a browser, APIs, your scripts. But:
  - Tool APIs must be clearly described, with examples.
  - Access must be sandboxed. A single wrong `rm -rf` can ruin your day.
  - It’s often best to approve each action the model wants to take.
- **RAG (Retrieval-Augmented Generation).** Instead of dumping a 20 MB text file into the prompt (which won’t fit), you:
  1. Store documents in a searchable index.
  2. For each question, retrieve a small, relevant subset.
  3. Feed only that subset to the model as context.

  This is how you make AI genuinely useful over your own data (docs, code, internal knowledge bases) without hallucinating wildly.
- **Wrappers and orchestration.** Many tools sit between you and the model: editor plugins, browser extensions, automation frameworks, “research assistants” etc. They:
  - Decide when to call which model.
  - Chunk and prepare context.
  - Call search engines, APIs, crawlers, or your scripts.
  - Loop over “retrieve → analyze → generate → refine”.

- **Multi-agent systems.** You can create several agents with different roles and let them collaborate. Useful, but also an efficient way to burn through tokens if not controlled.

Bottom line: for non-trivial tasks, the difference between “just a chat” and a **well-designed toolchain** around the model is huge.

### 4. What LLMs are actually good (and bad) at

Think in terms of “What would a smart, slightly lazy human be good at if they had read the whole internet?”

They’re great at:

- **Information search & aggregation.** A faster, more conversational front-end to lots of web searches.
- **Summaries and simplification.** Shortening verbose docs, emails, specs; turning jargon into plain language.
- **Style transformations.** Making text more polite, more formal, more casual, more textbook-like, etc.
- **Data transformations.** Converting between CSV, JSON, Markdown, C arrays, etc.
- **Exploration and “what’s possible?”** Getting overviews of new areas, exercises for learning something, or lists of common approaches.
- **Inspiration & idea-storming.** You don’t need to accept its proposals, but they’re a useful starting point.
- **Quick references.** Periodic tables, Python idioms, common patterns in a language or framework.
- **Learning and explanation.** Explaining code snippets, grammar, math steps; generating practice exercises; adapting explanations to your level.
- **Reviewing.** Spotting obvious mistakes in code, grammar, style, or patterns. It’s good at catching outliers, less good at deep conceptual issues.
- **Translations.** Much better and more context-aware than classic machine translation (within reasonable size limits).
- **Throw-away prototypes and placeholders.** Demo web pages, simple dashboards, scripts to glue tools together, rough icons, draft one-pagers, etc.

They are **weak or dangerous** at:

- **Highly niche details.** Unknown open-source projects with 20 downloads, small channels, internal company tools — assume it doesn’t know them unless you provide the data.
- **Up-to-date specifics.** Versions, prices, availability, breaking changes — always cross-check.
- **Hallucination.** If it doesn’t know, it tends to *invent* plausible-sounding nonsense instead of admitting ignorance.
- **Sycophancy.** Models are trained to please users. If you ask “This idea is good, right?”, it’s biased to agree unless you explicitly demand critique.
- **Large, complex, multi-step tasks on your own data** without the right infrastructure (indexing, chunking, RAG, tools). That’s not a prompt problem; it’s a **system design** problem.

Rule of thumb:

> If a task is non-trivial for a competent human in that area, you probably need *more than just* “paste everything into a chat and pray”.

Split tasks, build the right scaffolding, and use the model as a component, not a magician.

### 5. A warning for future you

If you start using AI daily, the biggest risk is not “AI becomes too smart”.

The real risk is that it makes **you** dumber by removing all struggle.

Use AI as a coach, sparring partner, and power tool — **not** as a substitute for thinking in areas you actually want to master.

If you consciously decide:  
> “This skill is not important for me; I just need it done,”  
then offload away.

But if it’s core to your learning or professional growth, force yourself to:

1. Think first, then ask.
2. Use AI to explain, critique, and suggest alternatives.
3. Keep some friction in the loop so you still learn.

We don’t just need parental controls on AI.  
We need **self-control**: guardrails that stop us from outsourcing everything that makes us smarter.

Learning involves effort and frustration.  
Short-term comfort from offloading too much can lead to long-term fragility.

Use AI to **amplify** your thinking, not to switch it off.
