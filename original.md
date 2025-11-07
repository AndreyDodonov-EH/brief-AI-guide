## A guide to those who slept (on AI) last two years

### 1. General understanding

LLMs are just predictors of the most probable answer based on provided input,
and previously generated outputs.
They do not have logical frameworks or guidelines like (some) humans do.
You can try to _imitate_ those by clearly describing the logic/patterns/guidelines/approach for an LLM,
and/or providing an example.
You can’t expect logic or truth — you can expect contextual plausibility. The rest is up to you.
Basic LLMs (without add-ons described below) give you a rough equivalent of "what first pops to mind of a person with encyclopedic knowledge without much thinking or understanding".
It's not an answer to your question.
It is a text which with some luck also happens to be answer.

### 2. Notes on prompt engineering

- be **meaningfully verbose**
  If you want just generic information, e.g. data of sodium in a periodic table,
  you can (and should) say just that - "sodium periodic table". Not only you avoid unnecessary token burn, you save yourself time.
  If you need something more specific, give only _meaningful_ and _relevant_ details. Make sure they are structured.
  Avoid negatives, especially personified ones, such as "I do not care about". If it is a clear instruction,
  like "Exclude X from results", it may be still fine.
  But if you already have seen that LLM gives too much, prefer "only" in the instructive style,
  e.g. "List only the titles of all songs from album X of a band Y"
- avoid **unclear or combined instructions**
  Do not use "maybes", "you decides", "whatever you prefers", "ideally-s" etc.
  _Do not ask for multiple things in one prompt._ Even though modern tools (see below) might handle it well, quality is better if you first ask on possible ways to do something, then select one yourself,
  then instruct AI in the chat to do that in the defined way, with specifics, then export the result,
  and then ask to refine it.
- **ask model how to prompt**
  If you are unsure how to articulate your question, ask AI on tips for prompting or to create prompt for itself.
  Can be useful at the beginning.
- consider **context window**
  context window is certain maximum amount of tokens (words or word fragments) which model can "see" at once.
  And context itself is all the tokens in a current interaction.
  Even before hitting this maximum amount, quality starts to decline (the more you see, the less attention you pay to details.
  Same with models). It's also known as context rotting.
  Additionally, under the hood each next message feeds in all previous ones. So it happens quicker than you'd expect.
  Ask about one thing at a time. Ensure that output won't be too big.
  E.g. you can enquire about ways to achieve X. Once you have selected one (maybe after several extra questions), move onto the new chat.
  So the same rule as with *meaningful verbosity* applies - dialogue should have enough relevant info so that next answer is good, but not more.
  If conversation is already big, and contains relevant info, you may want to export it, ask AI to sum it up telling what's most relevant, and then feed it to the new chat.
  It's also known as task decomposition.
  Some modern wrappers and models can compile history into digests under the hood, but do not count on that.
- **be explorative**
  Feel free to ask what's possible in certain area, what are common approaches to do X,
  what exists etc. It might seem like it contradicts the previous topics, and partly it does:
  that's why it's better to broadly enquire in one chat about what's possible in one chat,
  and once you come up with a plan/option/approach, go into details or execution in another chat.
- avoid **topic hopping**
  Do not ask about things from different areas in the same chat.
  You reduce quality and burn more tokens.
- use **proper style**
  When prompting, use same style as the style of the source you would like to consult.
  Obviously LLM is not a database, but indirectly you increase the likelihood of getting digested information from a scientific paper if you have scientifc tone, information from IT documentation if you use IT jargon and calm and matter-of-factly tone, etc.
  It is especially relevant because many models try to mimic your tone, and it leads to injecting "ohs", "ahs", "maybes" etc. into your answer, and these are unlikely to be found in a scientific paper,
  so you get less from it's digest and more from the chats of armchair experts and keyboard warriors.
  There is a temptation to chat with it like with a buddy. Don't.
  The most common human bug is to treat anything that communicates with us as alive and to ascribe to it a set of functions typical to humans.
- use **system prompting**: you can add some rules which apply to all dialogues or at least
  to one complete dialogue. It can be found usually in settings menus of chat bots. If not, you can specify them just at the beginning of the chat.
  Some built-in rules in e.g. chat gpt include changing the personality it mimics, things it should or should not do etc.
  General rules not related to the task itself, but rather to the way it should operate.
  You can provide arbitrary ones. They have to be provided extremely matter-off-factly.
  Look online for good system prompts. There are even [collections of those](https://github.com/x1xhlol/system-prompts-and-models-of-ai-tools?tab=readme-ov-file). Paired with proper tone/style (see previous point), setting proper personality in a system prompt can improve result a lot.
  Good practice is also to include in system prompts things like "if you need ask further questions; if you do not know, tell so". Sometimes it helps.
- **using relevant context and examples**: make sure to provide examples of documents, code etc. if you want the result to be closer to it. And make sure to omit/exclude unnecessary data (e.g. when working with a big document or codebase)

Even outside of LLMs prompt engineering is a useful skill - it helps you collect your thoughts and articulate your requests or ideas much more efficiently.
In addition, while formulating your thoughts you will probably come up with the answer yourself. Or at least with a plan. Rubber duck effect.

### 3. Tools which can extend LLM features

There is a number of ways to _extend_ or _tweak_ LLM capabilities.
Please note - not _improve_ capabilities (of LLM itself).
However, you can _improve_ usefulness by extending or tweaking capabilities.
There are numerous tools which stay in motion.
So here more the principles they built upon are described, not exact tools or models.

- **choose the right model**: it seems obvious, but it is true.
  Some models are better at fixing small pieces of code, some at generic suggestions etc. etc.
  These things stay in motion, so look online for fresh benchmarks and leaderboards.
- **reasoning models**: these are models which include talking to itself,
  simulating exchange between LLM and you. This can extend how far LLM goes,
  i.e. the answer is not the first directly associated thought (not directly what "comes to mind" for a human), but rather a chain of associations. But keep in mind - it is still a chain of associations via the probabilities of how likely are words to be in the same context, not via some logical frameworks.

- **multi-modals**: there are models (or, rather, wrappers on top of several models) which can generate and/or understand not just text, but other formats, such as images or audio. This can often be combined with reasoning (or manual multiple prompts, chain-prompting) so that e.g. a model can generate a webpage, then reflect on the visual result (if it is provided or generated, see tool access below)

- **tool access**: it is a core idea of nowadays hyped **"agents"**. In addition to just chat, you give it access to tools, such as command line, search engines etc. These have to be either well-known tools with formalized interfaces, or you have to provide formalized description of their usage along with some examples. There are emerging ways to formalize tool access, such as MCP (Model Context Protocol). It's just a protocol, more of a convention really, such as e.g. REST. Also keep in mind, quality of a tool obviously matters. Default chat wrappers such as chatgpt website now already have access to some tools, such as search engine or python interpreter, but they may be not the best or most suitable tools.
  E.g. if you want AI to go throw a lot of webpages, i.e. act it as a powerful crawler, you either need to give it access to a conventional powerful crawler with direct instructions, or at least to a program which covers access issues, rate limit issues etc. etc.  
  But of course you want to make sure that model has access only to sandboxed tools. One deletion command can destroy all the usefulness.
  Chaining is also useful - you can verify each command it is about to execute, one by one.

- **using relevant context or even RAG**
  Yes, again (s. above). In the sense of tooling, often AI can be sitting on top of certain data source right away, such as browser extensions which prepare context and then talk to API.
  Use it. Make sure that you understand the data flow.
  But beware of both context rotting and hard context limits.
  20 MB text file can not be processed in one go.
  If you want AI to be meaningfully useful on big amounts of data, you need to set up
  RAG (Retrieval-Augmented Generation). It is an approach where you setup tools to retrieve and prepare information from your data base, documents, websites with rate limiting, or any other custom sources
  with classical tools,
  and then feed it to AI to generate the answer, if needed also with iterations under the hood (retrieve-analyze-retrieve gain-analyze-generate answer).
  Like most chat bots are augmented with google search engine,
  you might need to add classical tooling that is good at finding at retrieving relevant information in your data base,
  collection of documents, website with rate limiting or other custom source.

- **use proper wrappers, apis and context preparers**: web chats are most used, but are not necessarily best LLM wrappers for certain tasks. There are wrappers which serve different purposes and already include access to certain tools, mechanisms to invoke LLX (LLM or other model(s), such as image-based) multiple times under the hood. Call it reasoning on steroids - where instead of just talking to itself, model googles, curls, calls your scripts, defines what files in your code repo are relevant for your original request (often with a simpler model first, like you would do it as described in "using relevant context" above), etc. etc. and injects the results into reinvoking itself, so that it has not just text-based associations, but search results, script results, page renders etc.
  There are numerous business built around it, starting from simple browser extensions which scan e-mails or sum up youtube videos, to powerful coding assistance in the form of editors, terminals, extensions etc. etc. (Cursor, Codex, Windsurf, Perplexity just to name a few). Ideally google (or ask AI!) to know what AI-powered tools may be relevant for your task.
  Keep in mind that model might not know newest state of things, so make sure it can also search online on your behalf.
  And sometimes you need direct API usage, custom MCP servers and multiple agents.
  You can also schedule recurring automations — for example, have your agent run weekly research on a specific topic and deliver a short, personalized digest (your own "news feed").
  Treat this like any other toolchain: define the sources, rate limits, and output format; log results; and keep a human approval step if the automation can take actions.

- **Multi-agents systems**. Building on all of the above, you can use several agent and define communication between them. You can do it in any way, but there are also emerging protocols, such as A2A - protocol for interaction between agents.
  But beware - with a custom multi-agent system you will probably burn through your tokens rather quickly.

Surely newer patterns, principles and tools will emerge.

All-in-all:
do task decompositions and looping.
Interleave AI communication with tool calls, data provisioning, self-iteration, and,
most-importantly, your own input. Stay in the loop yourself. Do not expect magic.
As of now no all-purpose wrapper exists.
To achieve best results, you have to find or build most suitable toolchain.
Often, if task involves processing a lot of specific data, it's not a prompt problem, it's an infrastructure problem,
e.g. you need to prepare proper data and introduce proper iterations.
For that, you might get interested in RAGs or even running specific models locally,
but it is beyond the scope of this article.

### 4. What can be done efficiently

Knowing the above, you can easily come up with dos and dont's with AI.
In general - the closer something is to common knowledge, the better the results.
The closer the task to association-based text processing, the better the results.

- **Information search&aggregation**
  AI is extremely good as a replacement for (multiple) google searches.
  If you want to get lists, overviews etc. a lot of time can be saved compared to browsing multiple resources.
  Especially now that AI can do a lot of googling for you.

- **Summaries & making sense of**
  If you need a summary of an e-mail with a lot jargon/bs,
  a shorter version of verbose technical documentation, AI is perfect.
  My most powerful use-case was summing up a documentation of a certain industrial protocol.
  Documentation was extremely verbose and not practical - it was describing both relevant parts and strange niche or even unused cases equally detailed.
  Based on the document and google searches AI could provide summary which was much clearer and more actionable.

- **Style changes**
  Not only summaries are possible, but also other changes.
  If you want something to be explained in simple terms,
  or sound more polite, or more bookish, do ask AI.

- **Data transformation**
  You can do same with technical data, e.g. transform between CSV, C arrays, Markdown etc.

- **Broadening your views**
  If you want to start with a new topic, e.g. learn a new language,
  ask about possible exercises for certain muscle groups or about newest trends in AI usage:
  chats with LLMs are good place to start.
  It can be more efficient "exploring" than just googling.
  Often we don't think things which are outside of our bubble.
  AI can give you some decent "Wait, it exists?" and "Wait, it was possible all the time?" moments

- **Inspirations**
  You don't have to use what AI gives you.
  But it can be a good start. Ask about what's possible.
  Ask about common approaches to do something. Ask away.

- **Quick references**
  If something is fairly common (periodic table, popular band,
  ways to do something in Python) AI can provider better and more consolidated results
  than just googling or even manuals

- **Be careful about too domain-specific information**
  If something is extremely domain-specific, AI may easily hallucinate.
  Cross-check and verify.

- **Do not assume that something niche went into training data**
  If there is a software packet with 20 downloads or a youtube channel with just a thousand
  subscribers, do not assume that AI knows it, or even knows of it.
  It's more likely to provide some data which are more generic, but may be plain wrong
  in regard of this packet or channel. If you need to work with such information,
  make sure to prepare it well (feed relevant source files from this software packet,
  download data via apis or crawlers etc.)

- **Beware of hallucinations**
  The more niche or personal you go - the bigger the chance of hallucination.
  AI is good at generics and bad at specifics.
  Additionally, models will rarely refuse to answer or tell that they do not know.
  They'd rather generate whatever information regardless of its quality.

- **Beware of a sycophancy**
  Even though AI is excellent first partner to verify your idea -
  to ask wether it makes sense or to ask if you are right - it may still support you
  just to please you. AI reward models during fine tuning include likes from testers and users,
  so the content is aiming to please most people.
  You can counter it with system prompts (especially in regard of style, personality, etc.)
  and by asking for critic explicitly.
  AI aims to please and lets bad ideas shine.

- **Learning**
  Anything.
  If you want explanation of a certain grammatical structure or a little piece of code,
  if you want to create exercises for learning (words in a foreign language, certain computer language feature, whatever), AI is very good at it.
  Even defining timelines to learn something, refining learning programs and finding materials on certain topics can be good.
  It's a patient teacher and can rephrase very well until you understand.

- **Reviews**
  Be it code review or grammar review of an e-mail,
  basically any text-based material, AI is very good at spotting outliers.
  It's not likely to find deep conceptual errors, but if something does not match
  a dictionary, library usage, or widely adopted patterns - it will be found.
  It helped me found countless stupid bugs before they went into production.

- **Translations**
  Not just changing style, but translation with AI are insanely better
  than with any conventional tool.
  But keep context rotting in mind. It won't translate you a big book in one go,
  or a huge codebase.

- **Throw-away prototypes**
  If you want to show a demo of a webpage, if you need a local dashboard,
  if you need a script to transform X and Y,

- **Supplementary materials and placeholders**
  If you need one-pages, nicer visuals for the presentations (such as better rendered charts), any kind of small sites, temporary icons etc. - AI can help.
  Remember that it handles text formats better, so e.g. it can generate a decent A4-formatted HTML for printing which will make a nice leaflet.

Remember - with just an LLM you'll get what comes first to mind of a person knowledgeable in the area.
Rule of thumb:
if the task is non-trivial to a person knowledgeable in the area,
you need not just AI to solve it.
(and by trivial I mean not that they know it can be done or conceptually know how,
but rather that they can do it without a plan, without reiterating even once etc.)
If you yourself would need to come up with more than a one-step plan - better split prompts, split tasks, and you might need not just AI. But it can still help you a lot when used properly.

### 5. Important warning for future you

Let's imagine in some time you use AI regularly.
Be careful.

The danger is not that AI will become too smart.
The danger is that it will help us become more stupid.

You need to be disciplined. Use AI for learning, not to solve homework or tasks at work
which you are unsure how to approach yourself. Let AI be your coach and tool. Not your substitute.

If you consciously decide that doing X
is not relevant for your education, self-improvement, professional skills etc - go ahead.
If you a physics PhD, you probably won't benefit much from learning how to center div in your web-based presentation.

Ideally, we need not just parent-control in AI, we need self-control.
E.g. system prompts which won't let AI prevent you from becoming better you.

Remember - learning means struggle. Offloading tasks too often for a short-term benefit will make you broke long-term.
