# The Complete Claude and Agentic AI Handbook

A personal, long-term reference for going from zero knowledge of AI to advanced, hands-on knowledge of Claude, Agentic AI, and MCP — written so anyone can follow it, with a few bonus notes for infrastructure/SRE work along the way.

## DOCUMENT METADATA
- **Version:** 0.3 (Draft — Parts 0–16 complete; Appendices pending)
- **Last Updated:** 2026-09-08
- **Primary Sources:** platform.claude.com/docs · anthropic.com · modelcontextprotocol.io
- **Style rule:** one-line explanations, not paragraphs. Every concept gets a "what it is," "why we use it," and one clean, universally-understandable example. Infra/SRE relevance shown as a small bonus note, not the main teaching example. Official source linked at the end of every part.

## CHANGE LOG
| Version | Date | Changes |
|---|---|---|
| 0.1 | 2026-09-08 | Initial draft, heavy per-chapter template, SRE-first examples |
| 0.2 | 2026-09-08 | Rewritten for universal readability — one-liners, plain examples first, SRE notes as small bonus asides |
| 0.3 | 2026-09-08 | Added Parts 4–16 in the same style (Prompting, API, Structured Output, Tool Use, Agentic AI, Agent SDK, Claude Code + CI/CD, MCP + server build, RAG, Multi-Agent, AI+SRE, Production, Final Project) |

## TABLE OF CONTENTS
- Part 0 — Getting Set Up
- Part 1 — AI Foundations
- Part 2 — Understanding LLMs
- Part 3 — Understanding Claude (+ Models)
- Part 4 — Prompting & Context Engineering
- Part 5 — Claude API (+ Vision, Extended Thinking, Production)
- Part 6 — Structured Output
- Part 7 — Tool Use
- Part 8 — Agentic AI
- Part 9 — Claude Agents & the Agent SDK (+ Computer Use)
- Part 10 — Claude Code (+ Plugins, CI/CD)
- Part 11 — MCP (+ building a server, security)
- Part 12 — RAG
- Part 13 — Multi-Agent Systems
- Part 14 — AI + SRE Use Cases
- Part 15 — Production Agentic AI (Reliability, Security, Observability, Evals)
- Part 16 — Final Project
- Appendices A–D (Glossary, Cheat Sheets, Docs Map, Pricing Snapshot) — *pending*

---
---

# PART 0 — GETTING SET UP

There are three different "Claudes." Know which one you're using before anything else:

| Name | What it's for |
|---|---|
| **Claude.ai** | The chat website/app. Not covered in this handbook. |
| **Claude API** | Lets you build your own software that uses Claude. Core focus here. |
| **Claude Code** | A tool that lets Claude work inside your terminal/codebase. Core focus here. |

**Why it exists:** you need different tools depending on whether you're chatting, building an app, or automating your codebase — same reason a browser, a code editor, and a terminal are three different tools, not one.

**Steps to get running:**
1. Go to `platform.claude.com`, sign up, create an API key.
2. Save the key as an environment variable — never paste it directly into code:
   ```bash
   export ANTHROPIC_API_KEY="sk-ant-xxxxxxxx"
   ```
3. Install the tools:
   ```bash
   pip install anthropic                        # Python SDK
   npm install -g @anthropic-ai/claude-code      # Claude Code CLI
   ```
4. Check it worked:
   ```bash
   claude --version
   python3 -c "import anthropic; print(anthropic.__version__)"
   ```

**Example anyone can follow:** an API key is like a house key — it proves you're allowed in, so you never leave a copy lying in a public place (like committed in code on GitHub).

**🔧 Bonus (infra note):** treat this key exactly like a cloud secret (AWS key, Kubernetes Secret) — store it in your usual secrets manager, not in a script.

**In one line:** three tools, one underlying model — pick the one that matches what you're doing.

🔗 Official: [Claude Console setup](https://platform.claude.com/docs/en/home) · [Claude Code install](https://platform.claude.com/docs/en/claude-code/overview)

---
---

# PART 1 — AI FOUNDATIONS

## What is Artificial Intelligence (AI)?
- **What it is:** software that does things we normally think need a human brain — understanding language, recognizing images, making decisions.
- **Why it exists:** some problems are too complex or too varied to solve with fixed step-by-step instructions.
- **Example:** a chess program that follows programmed rules is AI — even though it never "learns" anything.
- **In one line:** AI is the goal (act smart); everything below is a *method* for reaching that goal.

## What is Machine Learning (ML)?
- **What it is:** instead of writing exact rules, you show the computer lots of examples and it figures out the pattern itself.
- **Why it exists:** some patterns are too complicated for a human to write rules for by hand.
- **Example:** show a computer 10,000 emails labeled "spam" or "not spam," and it learns to spot spam on its own — nobody wrote "if email contains X, it's spam."
- **In one line:** ML = learning from examples instead of being told the rules.

## What is Deep Learning (DL)?
- **What it is:** a specific ML method using many stacked layers of simple math units (loosely inspired by neurons in a brain) to learn very complex patterns.
- **Why it exists:** simple ML struggles with messy, high-detail data like images, sound, and full sentences — deep learning handles that complexity.
- **Example:** how your phone recognizes a face in a photo, even in different lighting or angles.
- **In one line:** DL = ML with many layers, built for complex data like images and language.

## What is Generative AI (GenAI)?
- **What it is:** deep learning models trained to *create* new content, not just label or sort things.
- **Why it exists:** sorting/classifying answers "what is this?" — sometimes you want "write/draw/say something new" instead.
- **Example:** a tool that writes a birthday message for you, instead of just detecting if a message is happy or sad.
- **In one line:** GenAI creates new things; older ML mostly just classifies or predicts numbers.

## What is a Large Language Model (LLM)?
- **What it is:** a generative AI model trained on huge amounts of text, specialized in understanding and producing language.
- **Why we use it:** almost every job involves reading, writing, or explaining something in words — a model fluent in language can help with an enormous range of tasks without being custom-built for each one.
- **Example:** without an LLM, "summarize this 40-page PDF" needs a human to read it. With an LLM, you hand it the PDF and get a summary in seconds.
- **Another example:** without an LLM, "explain this error message in plain English" means searching forums yourself. With an LLM, you paste the error and get an explanation immediately.
- **In one line:** an LLM is GenAI specialized in language — useful anywhere a task can be described in words. **Claude is an LLM.**

### How they all connect
```
Artificial Intelligence   → the goal: act smart
    Machine Learning      → the method: learn from examples
        Deep Learning     → the technique: many-layered learning, for complex data
            Generative AI → the use: create new content
                LLMs      → the specialty: language
                    Claude, GPT, Gemini...  → actual products
```

**🔧 Bonus (infra note):** a fixed alert rule ("CPU > 90% → page") is old-school software. An anomaly detector that learns your normal traffic pattern is ML. An assistant that reads an alert and writes a plain-English theory of what broke is the GenAI/LLM layer — that's where Claude comes in, covered fully in Part 14.

### More worked examples — Part 1

**AI, a second example:** a GPS app calculating the fastest route is AI — it's solving a "smart" problem (best path through traffic) using search algorithms, with zero machine learning involved. This is why "AI" existed as a field for 60+ years before "ML" or "LLMs" became common words — AI is the whole umbrella, not a synonym for the newest technique.

**ML, a second example:** a bank's fraud detector doesn't have a rule for every possible scam — it was shown millions of past transactions labeled "fraud" or "not fraud," and it learned the subtle combination of signals (amount, location, time, merchant type) that tends to indicate fraud. Nobody sat down and wrote "if amount > $500 and country changed in the last hour, flag it" — the pattern was learned from data, and it keeps adapting as new data comes in.

**DL, a second example:** an old-school ML spam filter might count "money," "free," "click here" and add up a score. A deep learning model instead looks at the entire structure and phrasing of an email, layer by layer, picking up much subtler and harder-to-describe patterns — the same jump in capability that let image recognition go from "detects orange blobs" to "recognizes your specific cat's face."

**GenAI, a second example:** a translation tool that converts French to English is doing a transformation, not really "creating" — GenAI is specifically about producing genuinely new content: a summary that didn't exist before, an image that's never been seen, code that's never been written, tailored to your exact request.

**LLM, a third example:** ask an LLM to write a haiku about your morning coffee, and one is generated on the spot — it wasn't stored anywhere waiting to be retrieved; the model composed it token by token, guided by patterns learned from millions of poems it was trained on.

🔗 Official: [Anthropic — What is Claude?](https://www.anthropic.com/claude)

---
---

# PART 2 — UNDERSTANDING LLMs

## What is a token?
- **What it is:** a small chunk of text — roughly 4 letters, or about ¾ of a word in English. Not the same thing as a "word."
- **Why it exists:** computers don't read text the way we do — they need everything broken into small, countable, numbered pieces first.
- **Example:** the word "unbelievable" might split into three tokens: `un` `believ` `able`.
- **In one line:** tokens are the actual units the model reads, counts, and charges you for — not words.

## What is tokenization?
- **What it is:** the process of splitting your text into tokens before the model can use it.
- **Why it exists:** the model can only work with its own fixed vocabulary of chunks — it has to translate your sentence into that vocabulary first.
- **Example, worked out:** send Claude the sentence *"Please restart the payment-service pod."* It splits roughly like this:
  ```
  Please | restart | the | payment | - | service | pod | .
  ```
  Notice: common English words ("Please," "restart," "the") each become one token. Your made-up hyphenated name ("payment-service") gets chopped into three pieces, because the model has never seen that exact word before.
- **In one line:** everyday words tokenize efficiently; unusual/technical words and names get split into more, smaller pieces.

**🔧 Bonus (infra note):** this is why log lines full of pod names and UUIDs cost more tokens than a plain-English sentence of the same length — try it yourself:
```python
import anthropic
client = anthropic.Anthropic()

for text in [
    "Please restart the service in the cluster.",
    "Please restart the payment-service-7f9c8b4d6-x2k9p pod in namespace prod-checkout-eu-west-1.",
]:
    count = client.messages.count_tokens(model="claude-sonnet-4-5", messages=[{"role": "user", "content": text}])
    print(f"{len(text):>3} chars -> {count.input_tokens:>3} tokens : {text}")
```
Run it — the second line uses far more tokens per character, even though a human calls them "about the same length."

## What is context / a context window?
- **What it is:** context = everything the model can currently "see." The context window = the maximum amount of that it can hold at once.
- **Why it exists:** the model can't remember everything ever said to it — like a person's short-term memory, there's a limit to how much fits in view at one time.
- **Example:** if you're mid-conversation and paste in a giant document, older parts of the conversation may get pushed out to make room — like a whiteboard that's full, so writing something new means erasing something old.
- **In one line:** context window = the model's "working memory limit" for a single request.

## What is training vs. inference?
- **Training:** the one-time, expensive process where the model learns from massive amounts of text. Done once, by Anthropic — not something you do.
- **Inference:** what happens every time *you* send a message — the already-trained model reads it and generates a response. This is what you actually pay for, per request.
- **Example:** training is like a student spending years in school. Inference is that same person answering one question for you at work today — school doesn't happen again each time they answer.
- **In one line:** training happens once (Anthropic's job); inference happens every request (your job, and your bill).

## What are parameters?
- **What it is:** the internal numbers the model learned during training — its "knowledge," locked in place once training finishes.
- **Why it exists:** these numbers are literally what makes the model good at predicting language — more/better-tuned parameters generally means better performance.
- **Example:** think of parameters like a chef's fully-formed intuition after years of cooking — you can't "edit" it directly, you can only give the chef a new recipe (your prompt) to work from.
- **In one line:** parameters are fixed after training — you influence the output only through what you send in (Part 4).

## Input tokens vs. output tokens
- **What it is:** input tokens = what you send in. Output tokens = what the model generates back.
- **Why it matters:** they're priced separately, and output tokens usually cost more per token than input.
- **Example:** asking a one-line question (few input tokens) but requesting a 10-page report back (many output tokens) — most of your cost there is in the output.
- **In one line:** always know roughly how much you're sending *and* how much you're asking for back.

### More worked examples — Part 2

**Tokenization, a second worked example (a longer, messier sentence):**
Send: *"ERROR 2026-09-08T14:32:01Z pod=payment-service-7f9c8b4d6-x2k9p reason=OOMKilled exitCode=137"*
This roughly tokenizes into 20+ pieces, because almost nothing in it is a common English word:
```
ERROR | 2026 | - | 09 | - | 08 | T | 14 | : | 32 | : | 01 | Z | pod | = | payment | - | service | - | 7 | f | 9 | c | 8 | b | 4 | d | 6 | - | x | 2 | k | 9 | p | reason | = | OOM | Killed | exit | Code | = | 137
```
Compare that to a plain-English sentence of similar character length — *"The payment service crashed because it ran out of memory."* — which tokenizes into roughly 12 tokens. **Same rough length, almost double the tokens**, purely because of timestamps, hashes, and identifiers. This is the single most useful practical fact in this whole chapter for anyone working with logs at scale.

**Context window, a second example with real numbers:** suppose a model's context window is 200,000 tokens. You paste in a 150,000-token log file, plus your conversation history is already 60,000 tokens. That's 210,000 tokens total — over the limit *before Claude even writes a reply*. You'd need to either trim the log file, summarize the older conversation, or split the task — this exact math is why Part 5's "production considerations" insist on counting tokens before sending large payloads, not after something breaks.

**Training vs. inference, a second example:** think of a spell-checker dictionary. Building the dictionary (scanning millions of documents to learn which words are valid) happens once, offline — that's training. Every time your word processor underlines a typo as you type, it's just checking against that already-built dictionary — that's inference. You never rebuild the dictionary while typing; you only *use* it, over and over, cheaply and quickly.

**Parameters, a second example:** two chefs trained at different schools, cooking the same recipe, produce slightly different results — their trained "parameters" (years of built-up technique) differ. Similarly, two different LLMs given the identical prompt can give meaningfully different answers, because their internal parameters — shaped by different training data and processes — differ, even though the "recipe" (your prompt) was identical.

**Input/output tokens, a second worked example with rough cost math** (⚠️ illustrative — verify real prices in the Console): suppose input costs $3 per million tokens and output costs $15 per million tokens. A request with a 2,000-token input and a 500-token output costs roughly: (2,000 ÷ 1,000,000 × $3) + (500 ÷ 1,000,000 × $15) ≈ $0.006 + $0.0075 ≈ **$0.0135** — a little over a cent. Now imagine that same request running 50,000 times a day (e.g., triaging every incoming log alert) — that's roughly $675/day, which is exactly the kind of number that should show up on a dashboard (Part 15), not be discovered for the first time on an invoice.

🔗 Official: [Tokens and context windows](https://docs.anthropic.com/en/docs/build-with-claude/context-windows) · [Token counting](https://docs.anthropic.com/en/docs/build-with-claude/token-counting)

---
---

# PART 3 — UNDERSTANDING CLAUDE

## What is Claude?
- **What it is:** an AI model made by Anthropic that reads text (and images) and writes back a helpful response.
- **Why it exists:** to help with any task you can describe in words — writing, analyzing, coding, answering questions, using tools.
- **Example:** ask it to explain a topic, summarize a document, or draft an email — it generates the answer fresh each time, it doesn't look it up from a stored file.
- **In one line:** Claude is a very capable, very fast writer/thinker that starts fresh each time, with no memory of you unless you provide it.

## The one big difference from normal software
- **What it is:** normal software gives the *same* output every time for the same input. Claude can give slightly different wording (sometimes different reasoning) each time, even with an identical question.
- **Why:** it's not looking up a fixed answer — it's predicting a good response fresh, each time, which naturally varies a little.
- **Example:** ask the same factual question three times in three new conversations — the core facts should stay the same, but the exact phrasing may differ.
- **In one line:** treat Claude's output like a helpful, smart, but not 100%-consistent collaborator — not like a calculator.

**🔧 Bonus (infra note):** never let raw Claude output directly trigger a real action (like deleting a resource) without a human or a validation check in between — covered fully in Part 15.

## The Claude product family
| Name | What it's for |
|---|---|
| Claude.ai | General chat |
| **Claude API** | Build your own apps/tools |
| **Claude Code** | Work inside a real codebase/terminal |
| **Claude Agent SDK** | Build your own custom AI agents |
| Claude Cowork | A desktop assistant for files/folders |

🔗 Official: [Anthropic — Claude overview](https://www.anthropic.com/claude)

---

## Claude's models — which one to use
- **What it is:** Claude comes in three "sizes": Haiku, Sonnet, Opus.
- **Why it exists:** not every task needs the most powerful (and most expensive/slowest) model — you pick based on how hard the task is.
- **Example, like choosing a car:**
  - **Haiku** = an economy car — fast, cheap, great for simple errands.
  - **Sonnet** = a family car — balanced, handles almost everything well. Good default.
  - **Opus** = a high-performance car — most capable, but slower and pricier. Save it for genuinely hard problems.
- **In one line:** start with Sonnet by default; drop to Haiku for simple/high-volume tasks; reach for Opus only for the hardest reasoning.

⚠️ **MAY CHANGE:** exact model names/prices update over time — verify current ones in the Console before relying on a specific name.

```python
import anthropic
client = anthropic.Anthropic()

response = client.messages.create(
    model="claude-sonnet-4-5",   # verify current name in the Console
    max_tokens=1024,
    messages=[{"role": "user", "content": "Summarize this in 3 bullets."}]
)
print(response.content[0].text)
```
- `model=...` — which "size" handles this request.
- `max_tokens=1024` — the most tokens it's allowed to generate back.

### More worked examples — Part 3

**"What is Claude," a second example:** ask Claude to write a cover letter for a job posting you paste in. It doesn't search a database of cover letters — it generates one from scratch, shaped by the specific posting and any details you give it, the way a human writer drafts something original rather than copy-pasting a template unchanged.

**The determinism difference, a second example:** run `2 + 2` in a calculator a thousand times — you get `4` every time. Ask Claude to "explain what a firewall does" a thousand times, across a thousand fresh conversations, and you'll get a thousand explanations that agree on the facts but differ in wording, length, and which analogy they reach for. Neither is "wrong" — it's simply not the same kind of tool as a calculator.

**Choosing between the product family, a quick decision guide:**
- Want to just ask something and read the answer? → **Claude.ai**
- Want your own app/script to call Claude programmatically? → **Claude API**
- Want Claude working inside your actual codebase, running commands, editing files? → **Claude Code**
- Want to build your *own* custom agent with your own tools, using the same engine as Claude Code? → **Claude Agent SDK**
- Want an assistant working inside a folder of local documents/spreadsheets on your desktop? → **Claude Cowork**

**Model choice, a concrete before/after:** a team routed every support-ticket classification (just picking 1 of 5 categories) through Opus. Switching that one narrow, high-volume task to Haiku cut cost for that workload by roughly 10x with no measurable drop in accuracy — the task never needed deep reasoning. They kept Opus for their one genuinely hard task: synthesizing a quarterly trend report across dozens of documents. **The lesson: audit what each task actually needs — not once for the whole system.**

🔗 Official: [Models overview](https://docs.anthropic.com/en/docs/about-claude/models/overview) ⚠️ · [Model deprecations](https://docs.anthropic.com/en/docs/about-claude/model-deprecations) ⚠️ · [Pricing](https://www.anthropic.com/pricing) ⚠️

---

---
---

# PART 4 — PROMPTING & CONTEXT ENGINEERING

## What is a prompt?
- **What it is:** the text (and/or images) you send Claude to get a response.
- **Why it exists:** Claude has no idea what you want until you tell it — the prompt is the entire instruction.
- **Example:** typing "fix this" gives Claude almost nothing to work with. Typing "fix the typo in line 3 of this email" gives it something specific to do.
- **In one line:** a prompt is your only way to communicate intent — vague in, vague out.

## System prompt vs. user prompt
- **System prompt:** sets the role/rules for the whole conversation — set once, applies throughout.
- **User prompt:** the actual question/request for this specific turn.
- **Example:** system prompt = "You are a calm, precise incident-response assistant. Always ask for confirmation before suggesting destructive actions." User prompt = "The checkout service is returning 500s."
- **In one line:** system prompt = the personality/rules; user prompt = today's specific ask.

## Instructions, constraints, and examples (the three levers of a good prompt)
- **Instructions:** what to do. *"Summarize this."*
- **Constraints:** the boundaries. *"In 3 bullet points. No jargon."*
- **Examples (few-shot):** show, don't just tell. *"Format like this: → Issue: ... → Fix: ..."*
- **Why they matter:** without constraints and examples, Claude has to guess your preferred format — often wrong on the first try.
- **In one line:** tell it what to do, how to shape it, and show it an example if the format matters.

## Bad prompt → improved prompt → production prompt
A simple example anyone can follow, evolving in three stages:

**Bad:** *"Look at this and tell me what's wrong."*
- Too vague — wrong about what? In what format? For whom?

**Improved:** *"This is an error log from a web server. Explain in plain English what likely went wrong."*
- Better — gives context (it's a server log) and a clear ask (plain-English explanation).

**Production-ready:** *"You are a calm SRE assistant. Given the error log below, respond with exactly: (1) one-sentence likely cause, (2) confidence level (low/medium/high), (3) one suggested next step. Do not suggest destructive actions. Log:\n\n<paste log>"*
- Best — defines the role, the exact output format, and an explicit safety boundary. This version behaves consistently enough to use inside an automated pipeline.

**In one line:** vague prompts get vague answers; specific role + format + boundaries get consistent, usable answers.

## What is context engineering?
- **What it is:** deliberately deciding what information Claude actually sees for a given request — not just "the prompt," but everything around it.
- **Why it exists:** the context window (Part 2) is limited and shared — stuffing in everything "just in case" wastes tokens and can push out what actually matters.
- **Example:** instead of pasting an entire 500-page manual, you paste only the 2 relevant paragraphs — same idea as giving a new employee just the one page they need, not the entire company handbook, for a single task.
- **In one line:** context engineering = giving Claude exactly what it needs, not everything you have.

```
System Instructions   (the role/rules)
        +
User Request           (what's being asked right now)
        +
Conversation History   (what's been said so far)
        +
Documents               (relevant files/data, only the relevant parts)
        +
Tool Results             (fresh data pulled in, e.g. from Part 7)
        ↓
     Claude
```

**In one line:** everything above the arrow competes for the same limited context window — be deliberate about what goes in.

### More worked examples — Part 4

**A second bad → improved → production example, in a completely different domain (writing a commit message):**
- **Bad:** *"Write a commit message for this."* — for what change? What style? How long?
- **Improved:** *"Write a git commit message for this diff, following the imperative-mood convention ('Fix bug' not 'Fixed bug')."* — now it knows the format convention.
- **Production:** *"Write a git commit message for this diff. First line: imperative mood, under 50 characters, no period at the end. Then a blank line. Then up to 3 bullet points explaining *why*, not just *what*. Diff:\n\n&lt;paste diff&gt;"* — fully specified: length limit, structure, and what kind of explanation is wanted.
- **Why this matters beyond code:** notice the pattern is identical to the incident-log example earlier — role/context, then exact structure, then boundaries. That three-part shape (context → format → boundaries) works for almost any prompt, in almost any domain.

**A second system-vs-user example:** system prompt = *"You are a patient math tutor for a 10-year-old. Never just give the answer — always ask a guiding question first."* User prompt, turn 1 = *"What's 12 × 15?"* User prompt, turn 2 (same conversation) = *"I don't get it, can you just tell me?"* — notice the system prompt's rule ("never just give the answer") still applies on turn 2, even though the user prompt changed — that persistence across every turn is exactly why it's called the *system* prompt.

**A second context-engineering example:** a customer-support bot doesn't need your entire order-history database in context for every question — if the customer asks "where's my package," you fetch (via a tool, Part 7) just that one order's tracking info and hand Claude only that, not all 200 of the customer's past orders. Same principle as the manual example, applied to live data instead of a static document.

🔗 Official: [Prompt engineering overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview) · [System prompts](https://docs.anthropic.com/en/docs/build-with-claude/system-prompts)

---
---

# PART 5 — CLAUDE API

## What is an API request/response, really?
- **What it is:** you send a structured message (JSON) to Anthropic's server; it sends a structured message back.
- **Why it exists:** software needs a predictable, machine-readable way to talk to Claude — not a chat window, a data format.
- **Example:** ordering food by pointing at a menu item number (structured) instead of describing the dish from scratch every time (unstructured).
- **In one line:** the API is a structured, repeatable way for your code to talk to Claude.

## Messages and roles
- **What it is:** every conversation is a list of messages, each tagged with a role: `system`, `user`, or `assistant`.
- **Why it exists:** Claude needs to know who said what — its own past replies (`assistant`) vs. your input (`user`) vs. the rules (`system`).
- **Example:** a 3-turn conversation is really just a growing list you resend each time — Claude has no memory between calls unless you include the history yourself.
- **In one line:** you're responsible for remembering the conversation — Claude only sees what's in the list you send.

## First real code example, explained line by line
```python
import anthropic

client = anthropic.Anthropic()  # reads ANTHROPIC_API_KEY from environment

response = client.messages.create(
    model="claude-sonnet-4-5",           # which model handles this request
    max_tokens=500,                      # cap on how much it can generate back
    system="You are a helpful assistant.", # the rules/role for this whole call
    messages=[
        {"role": "user", "content": "What is a load balancer, in one sentence?"}
    ]
)

print(response.content[0].text)  # the actual text Claude generated
```
- `anthropic.Anthropic()` — creates the connection to Anthropic's servers, using your saved API key.
- `model=` — picks Haiku/Sonnet/Opus (Part 3).
- `max_tokens=` — required; the maximum length of the reply.
- `system=` — the instruction that applies to the whole request.
- `messages=[...]` — the actual conversation, a list of role/content pairs.
- `response.content[0].text` — Claude's answer is nested inside a list (it can return multiple content blocks — text, tool calls, etc.) — for a simple text reply, the answer is the first item.

**In one line:** every API call needs a model, a token limit, and a list of messages — everything else is optional refinement.

## Vision — giving Claude images and PDFs
- **What it is:** Claude can accept images/PDFs as part of the input, alongside text.
- **Why it exists:** some information only exists visually — a screenshot, a diagram, a scanned document.
- **Example:** send a screenshot of a dashboard and ask "what looks abnormal here?" — no need to manually describe every number.
- **In one line:** if a picture is worth a thousand words, send the picture instead of typing the thousand words.

## Extended thinking — letting Claude reason longer before answering
- **What it is:** a mode where Claude spends extra effort "thinking" through a hard problem step by step before giving a final answer.
- **Why it exists:** some problems (multi-step math, complex debugging) benefit from deliberate reasoning rather than a fast, instinctive answer.
- **Example:** a simple factual question doesn't need it. A "here are 5 conflicting log entries, what's the actual sequence of events" question benefits from it.
- **In one line:** turn it on for genuinely hard reasoning tasks, not for everyday questions — it costs more time and tokens. ⚠️ MAY CHANGE — check current API parameters.

## Making it production-ready
- **Error handling & retries:** API calls can fail (network blip, rate limit, server hiccup) — always wrap calls in retry logic with backoff, exactly like you'd retry a flaky HTTP call to any external service.
- **Rate limits & timeouts:** Anthropic caps how many requests/tokens you can send per minute — design for it the same way you'd design for any third-party API's rate limit. ⚠️ MAY CHANGE — verify current limits.
- **Logging:** record every request's model, token counts, and latency — you'll want this the first time someone asks "why did this cost so much last Tuesday."
- **Prompt caching:** Anthropic can cache the unchanging part of your prompt (e.g., a long system prompt or reference document) so repeated calls are cheaper and faster — like a CDN cache, but for prompt content. Default cache lifetime is short (~5 minutes) unless you request a longer one. ⚠️ MAY CHANGE — verify current TTL options.
- **Batch API:** for large, non-urgent workloads (e.g., "classify these 50,000 old tickets overnight"), you can submit them as a batch instead of one-by-one — cheaper, and doesn't compete with your real-time traffic for rate limit budget.
- **In one line:** treat the Claude API like any other critical external dependency — retries, timeouts, logging, and cost visibility are not optional.

```
Your App → [build request] → Claude API → [response or error]
              │                                    │
              └── on error: retry w/ backoff ──────┘
              └── log: model, tokens, latency, cost, every call
```

### More worked examples — Part 5

**A multi-turn conversation, made concrete.** Remember: Claude has no memory between calls — *you* resend the growing list every time.
```python
messages = [{"role": "user", "content": "What does a 502 error mean?"}]

# Turn 1
response = client.messages.create(model="claude-sonnet-4-5", max_tokens=300, messages=messages)
print(response.content[0].text)
messages.append({"role": "assistant", "content": response.content[0].text})  # save its reply

# Turn 2 — the user follows up, referencing the previous answer
messages.append({"role": "user", "content": "Could that be caused by a timeout upstream?"})
response = client.messages.create(model="claude-sonnet-4-5", max_tokens=300, messages=messages)
print(response.content[0].text)
```
Notice `messages` keeps growing — by turn 2, you're sending turn 1's question *and* answer, plus the new question. This is exactly why long conversations quietly consume more and more of your context window (Part 2) with every turn.

**Streaming — getting the answer word-by-word instead of waiting for the whole thing:**
```python
with client.messages.stream(
    model="claude-sonnet-4-5",
    max_tokens=500,
    messages=[{"role": "user", "content": "Explain DNS in a short paragraph."}]
) as stream:
    for text in stream.text_stream:
        print(text, end="", flush=True)   # prints as each piece arrives, like a typing effect
```
**Why it exists:** for a long response, waiting for the *entire* answer before showing anything feels slow — streaming shows words as they're generated, the same reason a progress bar feels better than a frozen screen.

**A more complete production example — retries with backoff, in real code:**
```python
import time
import anthropic

client = anthropic.Anthropic()

def call_claude_with_retries(messages, max_retries=3):
    for attempt in range(max_retries):
        try:
            return client.messages.create(
                model="claude-sonnet-4-5",
                max_tokens=500,
                messages=messages,
            )
        except anthropic.RateLimitError:
            wait = 2 ** attempt  # 1s, 2s, 4s — exponential backoff
            print(f"Rate limited, retrying in {wait}s...")
            time.sleep(wait)
        except anthropic.APIStatusError as e:
            print(f"API error: {e.status_code} — {e.message}")
            raise  # don't blindly retry things like a 400 bad request
    raise RuntimeError("Max retries exceeded")
```
- `anthropic.RateLimitError` — a specific, catchable error for "you're sending requests too fast" — worth retrying after a pause.
- `anthropic.APIStatusError` — a broader error category; a 400 (you sent something malformed) should *not* be retried the same way — retrying a bad request just fails the same way three more times.
- **In one line:** not all errors deserve a retry — distinguish "temporary, worth waiting" from "wrong, needs a fix," exactly like you would for any flaky downstream API call.

🔗 Official: [Messages API reference](https://docs.anthropic.com/en/api/messages) · [Vision](https://docs.anthropic.com/en/docs/build-with-claude/vision) · [Extended thinking](https://docs.anthropic.com/en/docs/build-with-claude/extended-thinking) ⚠️ · [Rate limits](https://docs.anthropic.com/en/api/rate-limits) ⚠️ · [Prompt caching](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching) ⚠️ · [Batch API](https://docs.anthropic.com/en/docs/build-with-claude/batch-processing) · [Streaming](https://docs.anthropic.com/en/docs/build-with-claude/streaming)

---
---

# PART 6 — STRUCTURED OUTPUT

## Why structured output matters
- **What it is:** getting Claude to respond in a fixed, predictable data format (usually JSON) instead of free-flowing prose.
- **Why it exists:** software can't reliably parse a paragraph of English, but it can always parse a JSON field.
- **Example — unstructured:**
  > "It looks like the service is having a pretty serious problem, most likely because it's run out of memory. I'd recommend bumping up the memory limit."
  A program reading that has no reliable way to extract "severity" or "recommendation" — the wording changes every time.
- **Example — structured:**
  ```json
  {
    "severity": "critical",
    "root_cause": "memory exhaustion",
    "recommendation": "increase memory limits"
  }
  ```
  A program can read `data["severity"]` every single time, guaranteed.
- **In one line:** structured output turns a one-off answer into something your code can actually depend on.

## JSON, schema, and validation
- **JSON:** a simple, universal text format for data — like a form with clearly labeled fields instead of a paragraph.
- **Schema:** a definition of exactly which fields are required and what type each one must be (text, number, one of a fixed list, etc.).
- **Validation:** checking Claude's actual output against that schema before trusting it — same idea as checking a form was filled out completely before processing it.
- **Why it matters:** an LLM can occasionally produce malformed or incomplete JSON — validating catches that before it breaks something downstream.
- **In one line:** define the shape you expect, then verify the real output actually matches it.

## Pydantic — validating in Python
```python
from pydantic import BaseModel
from typing import Literal

class IncidentAnalysis(BaseModel):
    severity: Literal["low", "medium", "high", "critical"]
    root_cause: str
    recommendation: str

# Claude's raw output (as text) gets parsed and checked against the shape above:
raw = '{"severity": "critical", "root_cause": "memory exhaustion", "recommendation": "increase memory limits"}'
result = IncidentAnalysis.model_validate_json(raw)
print(result.severity)  # "critical" — guaranteed to be one of the 4 allowed values, or this line raises an error
```
- `class IncidentAnalysis(BaseModel)` — defines the exact shape you require.
- `Literal["low", ..., "critical"]` — severity must be exactly one of these four words, nothing else.
- `model_validate_json(raw)` — parses and checks Claude's text output; if it doesn't match, you get a clear error instead of a silent bad value flowing downstream.

**In one line:** Pydantic turns "I hope Claude's JSON is correct" into "my code guarantees it, or fails loudly."

### More worked examples — Part 6

**A second schema, a different domain (deployment risk assessment):**
```python
from pydantic import BaseModel
from typing import Literal

class DeploymentRisk(BaseModel):
    risk_level: Literal["safe", "caution", "high_risk"]
    affected_services: list[str]
    reasoning: str
    requires_approval: bool

raw = '''
{
  "risk_level": "high_risk",
  "affected_services": ["payment-service", "checkout-api"],
  "reasoning": "Change modifies the shared database schema used by both services.",
  "requires_approval": true
}
'''
result = DeploymentRisk.model_validate_json(raw)
if result.requires_approval:
    print(f"⚠️ Needs sign-off — {result.risk_level}: {result.reasoning}")
```
Notice `affected_services` is a *list* of strings, and `requires_approval` is a plain boolean — schemas aren't limited to simple flat fields; they can require whole structured shapes, which is exactly what lets this plug directly into an approval workflow (Part 15) instead of a human having to re-read a paragraph to figure out if sign-off is needed.

**What happens when validation actually fails — handle it, don't assume it can't happen:**
```python
from pydantic import ValidationError

raw_bad = '{"risk_level": "extremely bad", "affected_services": "payment-service"}'
# "extremely bad" isn't one of the allowed Literal values, and affected_services isn't a list

try:
    result = DeploymentRisk.model_validate_json(raw_bad)
except ValidationError as e:
    print("Claude's output didn't match the expected shape:")
    print(e)
    # sensible fallbacks: retry with a clarified prompt, fall back to a safe default,
    # or escalate to a human — but never let a malformed value flow downstream silently
```
**In one line:** validation failures are not a rare edge case in production — build the "what do we do when it's wrong" path from day one, not after the first incident it causes.

🔗 Official: [Structured outputs](https://docs.anthropic.com/en/docs/build-with-claude/structured-outputs)

---
---

# PART 7 — TOOL USE

## Why Claude can't just "do" things
- **What it is:** Claude only generates text — it cannot, by itself, restart a server, query a database, or click a button.
- **Why it exists (as a limitation):** it's a language model, not a piece of software with system access — same reason a very smart consultant can *tell* you what to do, but can't personally walk into your server room and do it.
- **The fix — tool use:** you give Claude a list of tools (functions) it's *allowed* to ask for, and your own code actually runs them.
- **In one line:** Claude decides *what* to do; your code is the one that actually does it.

```
Claude
    ↓  (decides a tool is needed, and with what input)
Tool Selection
    ↓
Your Python Function   (e.g., get_pod_status("payment-service"))
    ↓
External System         (e.g., a real Kubernetes cluster)
    ↓
Result                  (e.g., "3/3 pods running")
    ↓
back to Claude, which now writes the final answer using that real result
```

## Tools, schemas, parameters, results
- **Tool:** a function you define and describe to Claude (name, purpose, expected inputs).
- **Schema:** the exact shape of the input a tool expects — same idea as a function signature.
- **Parameters:** the actual values Claude fills in when it decides to use the tool.
- **Tool result:** what your function returns, which gets fed back to Claude to write its final answer.
- **In one line:** you describe the tool once; Claude decides when and how to call it, based on the conversation.

## A simple, clean example
Suppose you want Claude to be able to check if a website is up.
```python
import anthropic

client = anthropic.Anthropic()

tools = [{
    "name": "check_site_status",
    "description": "Checks if a website is reachable and returns its HTTP status code.",
    "input_schema": {
        "type": "object",
        "properties": {"url": {"type": "string", "description": "The URL to check"}},
        "required": ["url"]
    }
}]

response = client.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=500,
    tools=tools,
    messages=[{"role": "user", "content": "Is example.com up right now?"}]
)
# Claude replies with a tool_use request instead of plain text —
# your code then actually calls check_site_status("https://example.com"),
# and sends the real result back to Claude to finish its answer.
```
**In one line:** Claude never touches the network itself — it asks your code to, and your code decides whether to actually comply.

## Real examples in your world (Kubernetes, Prometheus, AWS)
- `get_pod_status(namespace, pod_name)` — checks live pod health in Kubernetes.
- `query_prometheus(metric, range)` — pulls a real metric value/time series.
- `list_ec2_instances(tag)` — checks running AWS instances matching a tag.
- **Why these specifically matter:** they turn Claude from "a smart assistant that can only guess" into "a smart assistant that can check the real, current state of your systems" — this is the foundation of everything in Part 14 and the Part 16 project.

### More worked examples — Part 7

**The full back-and-forth, made visible (this is the part most explanations skip).** A single "tool use" isn't really one API call — it's *two*, with your code in the middle:
```python
messages = [{"role": "user", "content": "Is example.com up right now?"}]

# Call #1 — Claude decides it needs the tool, and asks for it
response = client.messages.create(
    model="claude-sonnet-4-5", max_tokens=500, tools=tools, messages=messages
)
# response.stop_reason == "tool_use"  ← Claude is asking, not answering yet

tool_use_block = next(b for b in response.content if b.type == "tool_use")
print(tool_use_block.name)   # "check_site_status"
print(tool_use_block.input)  # {"url": "https://example.com"}

# Your code actually runs it:
real_result = check_site_status(tool_use_block.input["url"])  # e.g., "200 OK"

# You append Claude's request AND your result, then call again:
messages.append({"role": "assistant", "content": response.content})
messages.append({
    "role": "user",
    "content": [{"type": "tool_result", "tool_use_id": tool_use_block.id, "content": real_result}]
})

# Call #2 — now Claude writes the actual final answer, using your real result
final = client.messages.create(model="claude-sonnet-4-5", max_tokens=500, tools=tools, messages=messages)
print(final.content[0].text)  # "Yes, example.com is up (200 OK)."
```
**In one line:** "using a tool" is really: Claude asks → your code runs it → you hand the real result back → Claude writes the final answer using that real data. Nothing happens automatically — every step is a message you explicitly send.

**A second, closer-to-your-world tool, with multiple parameters:**
```python
tools = [{
    "name": "get_deployment_status",
    "description": "Returns the rollout status of a Kubernetes deployment.",
    "input_schema": {
        "type": "object",
        "properties": {
            "namespace": {"type": "string"},
            "deployment_name": {"type": "string"}
        },
        "required": ["namespace", "deployment_name"]
    }
}]
# Ask: "Is the checkout deployment fully rolled out in prod?"
# Claude fills in: {"namespace": "prod", "deployment_name": "checkout"}
```
Notice Claude has to *infer* `namespace: "prod"` from the word "prod" in the question — this is exactly why clear `description` fields on each parameter matter: they're the only hint Claude has for how to map your words onto your schema correctly.

🔗 Official: [Tool use overview](https://docs.anthropic.com/en/docs/build-with-claude/tool-use/overview)

---
---

# PART 8 — AGENTIC AI

## Traditional LLM use vs. an agent
**Traditional (single-shot):**
```
Input → LLM → Output
```
One question, one answer, done.

**Agent (a loop):**
```
Goal
  ↓
Think          (what should I do next?)
  ↓
Choose Action  (which tool, with what input?)
  ↓
Use Tool
  ↓
Observe        (what actually happened?)
  ↓
Repeat  ──────┘ (until the goal is met)
  ↓
Complete
```

- **What an agent is:** Claude given a goal, a set of tools, and permission to keep looping — think → act → observe → think again — until the goal is done, instead of answering once and stopping.
- **Why it exists:** many real tasks can't be solved in a single response — you often need to check something, react to what you find, and check again.
- **Example:** "figure out why the website is slow right now" can't be answered from a single guess — an agent might check server load, then check the database, then check the network, adjusting its next step based on what it finds each time — the same way a person would actually investigate, not the way a fortune teller would.
- **In one line:** a single LLM call answers a question; an agent investigates a situation, adapting as it goes.

## The pieces of the loop
- **State:** what the agent currently knows/has done so far.
- **Planning:** deciding the next step given the goal and current state.
- **Actions:** the tool calls it actually makes.
- **Observations:** the real results that come back from those actions.
- **Feedback:** using those observations to decide the *next* action — this is what makes it a loop, not a script.
- **In one line:** an agent's power comes entirely from being able to react to what it just learned, not from following a fixed script.

### A fully narrated example — "why is checkout slow?"

This is what an actual agent loop looks like, step by step, for a goal that can't be answered in one shot:

1. **Goal:** "Investigate why checkout is slow right now."
2. **Think:** "I should check if checkout itself is healthy first."
3. **Action:** calls `get_pod_status(namespace="prod", pod_name="checkout")`.
4. **Observe:** result = "3/3 pods running, no restarts." (Checkout itself looks fine.)
5. **Think:** "Pods are healthy, so maybe it's a downstream dependency. Let me check the database."
6. **Action:** calls `query_prometheus(metric="db_query_duration_p99", range="15m")`.
7. **Observe:** result = "p99 latency: 4.2s (up from a normal ~200ms)."
8. **Think:** "That's a strong lead — the database is the likely bottleneck, not checkout itself."
9. **Action:** calls `get_logs(pod_name="postgres-primary", lines=100)`.
10. **Observe:** logs show repeated "connection pool exhausted" errors.
11. **Complete:** reports back — *"Checkout itself is healthy. The database's connection pool is exhausted, causing query latency to spike 20x. Recommend increasing the pool size or investigating a possible connection leak."*

**Why this matters:** notice steps 4 and 5 — the agent's *second* action was chosen based on what it learned in the *first* one. A fixed script would have had to check the database anyway, whether or not it was relevant, and couldn't have skipped straight past checkout once it confirmed checkout was healthy. That adaptive redirection, step to step, is the entire value of the loop over a single-shot answer.

🔗 Official: [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents)

---
---

# PART 9 — CLAUDE AGENTS & THE AGENT SDK

## Building agents with Claude — starting simple, getting more complex
1. **Single-agent, no tools** — just a good prompt and a good model. Fine for straightforward Q&A.
2. **Tool-using agent** — one agent, several tools (Part 7), looping until done. Good for most real automation tasks.
3. **Multi-step workflow** — a fixed sequence of steps, each possibly using different tools, chained together.
4. **Multi-agent system** — multiple specialized agents, each with their own tools/expertise, coordinated by one "lead" agent (fully covered in Part 13).

**In one line:** start with one agent and a couple of tools — only add more agents once one agent genuinely can't keep up with the complexity.

## The Claude Agent SDK
- **What it is:** an official toolkit from Anthropic for building your *own* custom agents, using the same underlying engine that powers Claude Code — available for Python and TypeScript.
- **Why it exists:** building a reliable agent loop from scratch (state tracking, tool orchestration, error handling, permissions) is a lot of repeated plumbing — the SDK gives you that scaffolding so you focus on your actual tools and logic.
- **Example:** instead of writing your own "think → act → observe → repeat" loop by hand, you define your tools and goal, and the SDK runs the loop for you — the same relationship as using a web framework instead of hand-rolling HTTP parsing.
- **In one line:** if Claude Code is a finished house built on this engine, the Agent SDK is the engine itself, available for you to build your own house with.

```python
# Conceptual shape — verify exact API against current SDK docs (⚠️ MAY CHANGE)
from claude_agent_sdk import Agent, tool

@tool
def get_pod_status(namespace: str, pod_name: str) -> str:
    """Returns the current status of a Kubernetes pod."""
    ...  # your real implementation

agent = Agent(
    model="claude-sonnet-4-5",
    tools=[get_pod_status],
    system_prompt="You are an SRE assistant. Investigate issues using the tools available."
)

result = agent.run("Why might payment-service be unhealthy right now?")
print(result)
```
**In one line:** you supply the tools and the goal; the SDK handles the loop, tool-calling mechanics, and conversation state.

### A fuller Agent SDK example — multiple tools working together

```python
# Conceptual shape — verify exact API against current SDK docs (⚠️ MAY CHANGE)
from claude_agent_sdk import Agent, tool

@tool
def get_pod_status(namespace: str, pod_name: str) -> str:
    """Returns the current status of a Kubernetes pod."""
    return "3/3 pods running, no restarts"  # replace with a real k8s API call

@tool
def query_prometheus(metric: str, range: str) -> str:
    """Returns a Prometheus metric value over a given time range."""
    return "p99 latency: 4.2s (up from ~200ms)"  # replace with a real Prometheus query

@tool
def get_logs(pod_name: str, lines: int = 50) -> str:
    """Fetches recent log lines for a pod."""
    return "repeated 'connection pool exhausted' errors"  # replace with a real log query

agent = Agent(
    model="claude-sonnet-4-5",
    tools=[get_pod_status, query_prometheus, get_logs],
    system_prompt=(
        "You are an SRE assistant. Investigate issues using the tools available. "
        "Check the most likely cause first, and only escalate to deeper checks if needed."
    ),
    max_iterations=10,  # a hard cap — see Part 15 on why this matters
)

result = agent.run("Why is checkout slow right now?")
print(result)
# Internally, this runs the exact loop narrated in Part 8's worked example —
# the SDK is what's actually managing that think → act → observe → repeat cycle for you.
```
**In one line:** this is the same "why is checkout slow" investigation from Part 8, except now it's real, runnable code instead of a narrated story — the SDK's `Agent` object *is* the loop.

## Computer use
- **What it is:** letting Claude control a computer directly — looking at screenshots and deciding where to click or what to type, instead of using purpose-built tools.
- **Why it exists:** some tasks only have a visual interface, with no clean API to hook into (an old internal dashboard, a legacy admin panel).
- **Example:** navigating a website that has no API, by looking at a screenshot and clicking buttons — the same underlying idea behind the browser-automation demos you may have already seen in Claude Cowork.
- **A second example:** an internal, decade-old admin tool with no REST API at all — the only way to "check the queue depth" is to log into a web UI and read a number off the screen. Computer use lets Claude do exactly that: take a screenshot, locate the number, read it, and decide what to do next, the same way a human would if asked to check it manually.
- **In one line:** a fallback for "there's no clean tool for this, but there is a screen" — usually slower and less reliable than a purpose-built tool (Part 7), so prefer a real tool/API whenever one exists.

🔗 Official: [Agent SDK overview](https://platform.claude.com/docs/en/agent-sdk/overview) · [Computer use](https://docs.anthropic.com/en/docs/build-with-claude/computer-use) ⚠️

---
---

# PART 10 — CLAUDE CODE

## What is Claude Code, and how is it different from chat?
- **What it is:** a command-line tool (and IDE extension) where Claude works directly inside your actual project — reading files, running commands, editing code.
- **Why it exists:** chat requires you to copy-paste code back and forth; Claude Code instead operates *in* your project directly, the way you would.
- **Example:** in chat, you paste a function and ask "what's wrong with this?" In Claude Code, you just say "the login function is broken" — it opens the file itself, looks around the codebase for context, and proposes (or makes) the fix.
- **In one line:** chat is a conversation about your code; Claude Code works inside your code.

## Fundamentals
- **Projects:** Claude Code operates from your project's root folder, reading files as needed rather than requiring you to paste everything in.
- **Permissions:** by default, it asks before running commands or editing files — you approve or deny each action, same as reviewing a pull request before merging.
- **Git integration:** it can create branches, commits, and diffs — but *you* still decide what actually gets pushed.
- **Terminal usage:** it can run real shell commands (tests, builds, linters) and read the output to decide its next step.
- **In one line:** Claude Code is a careful collaborator inside your repo, not an autonomous committer — permissions keep a human in the loop.

## Advanced Claude Code: Skills, Subagents, Hooks, and CLAUDE.md
- **CLAUDE.md:** a file in your project that Claude Code reads automatically — your standing instructions ("we use pytest, not unittest," "never touch the `prod/` folder without asking").
- **Skills:** a `SKILL.md` file giving Claude Code specialized, reusable instructions for one kind of task — loaded automatically when relevant, not something you re-explain every session.
- **Subagents:** specialized helper agents Claude Code can delegate a sub-task to — like a lead engineer handing off "you handle the tests" to a teammate.
- **Hooks:** your own code that runs automatically at specific moments (before a tool runs, when a session starts) — useful for blocking a dangerous command before it executes.
- **In one line:** these four features turn Claude Code from "a smart assistant" into "an assistant that already knows your rules, your reusable workflows, and where to stop and ask."

### Claude Code Plugins — bundling it all together
- **What it is:** a packaged bundle of skills + subagents + hooks + MCP connections (Part 11), installable as one unit.
- **Why it exists:** so a whole team can share the exact same setup instead of everyone hand-configuring their own skills/hooks individually.
- **Example directory structure:**
  ```
  my-sre-plugin/
  ├── plugin.json          ← name, version, description
  ├── skills/
  │   └── incident-review/
  │       └── SKILL.md
  ├── subagents/
  │   └── log-analyzer.md
  ├── hooks/
  │   └── block-prod-delete.js
  └── mcp-servers.json      ← which MCP servers (Part 11) this plugin connects
  ```
- **In one line:** a plugin is "install our team's whole Claude Code setup in one command," instead of assembling it piece by piece.

## Claude Code in CI/CD (the most relevant chapter here for your job)
- **What it is:** running Claude Code *without* a human watching — inside a pipeline, triggered automatically.
- **Why it exists:** a lot of the value of Claude Code (reviewing a diff, investigating a failing test, drafting a fix) doesn't need a person sitting at the keyboard — it can run the same way any other CI step does.
- **Example — headless mode:**
  ```bash
  claude -p "Review this diff for security issues and summarize in 3 bullets" --output-format json
  ```
  The `-p` flag runs one prompt non-interactively and exits — perfect for a pipeline step.
- **Example — GitHub Actions:**
  ```yaml
  # .github/workflows/claude-review.yml
  name: Claude Code Review
  on: pull_request
  jobs:
    review:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v4
        - uses: anthropics/claude-code-action@v1
          with:
            anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
            prompt: "Review this PR diff for bugs and security issues."
  ```
- **In one line:** Claude Code isn't just an interactive tool — it's a CLI, which means anywhere you can run a CLI (a pipeline, a cron job, a webhook handler), you can run Claude Code. ⚠️ MAY CHANGE — verify current action name/flags.

### More worked examples — Part 10

**What a real interactive session looks like:**
```
$ claude
> the login endpoint is returning 500s intermittently, can you investigate?

Claude Code: I'll look at the recent changes and error logs first.
  → reading src/auth/login.py
  → running: git log -5 --oneline -- src/auth/login.py
  → running: grep -r "500" logs/app.log | tail -20

Claude Code: Found it — a recent commit (a3f9c2e) removed a null check on
`request.session`. When a client sends a request without a session cookie,
this throws an unhandled exception. Want me to add the null check back
and add a test for it?

> yes, go ahead

Claude Code: 
  → editing src/auth/login.py
  → running: pytest tests/test_login.py
  → 4 passed
  Created a fix. Want me to commit it?
```
Notice the shape: it investigates *first* (reading files, running commands, checking git history) before proposing anything, then it asks before making the actual change, then it verifies with a real test run, then it asks again before committing — permission checkpoints at every consequential step, not just at the end.

**A minimal `CLAUDE.md` example:**
```markdown
# Project rules
- We use pytest, not unittest.
- Never modify anything under `infra/prod/` without explicit confirmation.
- All new functions need type hints.
- Run `make lint` before considering any change complete.
```
**Why it exists, concretely:** without this file, you'd have to repeat "we use pytest" and "never touch prod without asking" in every single session — `CLAUDE.md` is read automatically, every time, so those rules are always in effect.

**A minimal hook example (blocking a dangerous command):**
```javascript
// hooks/block-prod-delete.js — runs before any shell command executes
module.exports = function(command) {
  if (command.includes("rm -rf") && command.includes("prod")) {
    return { block: true, reason: "Blocked: destructive command targeting prod." };
  }
  return { block: false };
};
```
**In one line:** a hook is a real safety net enforced in code — not a hope that Claude "remembers" not to do something dangerous.

🔗 Official: [Claude Code overview](https://platform.claude.com/docs/en/claude-code/overview) · [Claude Code CLI reference](https://platform.claude.com/docs/en/claude-code/cli-reference) · [GitHub Actions integration](https://github.com/anthropics/claude-code-action) ⚠️

---
---

# PART 11 — MCP (MODEL CONTEXT PROTOCOL)

## Why MCP exists
**Without MCP** — every AI tool needed its own custom, one-off connection to every system:
```
AI  →  Custom Integration #1  →  Gmail
AI  →  Custom Integration #2  →  Slack
AI  →  Custom Integration #3  →  Kubernetes
```
Every new tool = another custom integration to build and maintain. If you have 5 AI apps and 10 systems, that's up to 50 separate integrations.

**With MCP** — one shared standard, both sides agree to speak:
```
AI Client  →  MCP  →  MCP Server  →  Tools / Resources (Gmail, Slack, Kubernetes, ...)
```
Now it's 5 AI apps + 10 systems = 15 things that each just need to speak MCP once — not 50 custom wires.

- **Why it exists:** to stop every AI-to-tool connection from being a one-off, fragile, custom build.
- **In one line:** MCP is a shared plug shape (like USB-C) so any AI client can connect to any tool server, without a custom adapter for every pair.

## The pieces of MCP
- **MCP Client:** the AI application (e.g., Claude Code, Claude Desktop) that wants to use external tools/data.
- **MCP Server:** a small program that exposes specific tools/data over the MCP standard — you can write your own.
- **Tools:** actions the server offers (e.g., "get the current pods").
- **Resources:** data the server can hand over (e.g., a specific file or log).
- **Prompts:** reusable prompt templates the server can offer to the client.
- **Transport:** how client and server actually talk (usually local process pipes, or HTTP for remote servers).
- **In one line:** a server offers tools/resources/prompts; a client (like Claude) connects and uses them, all through one shared protocol.

## Building an MCP server — a real example, step by step
**The goal:** build a small "SRE MCP server" that exposes four tools Claude can call: `get_pods()`, `get_logs()`, `get_metrics()`, `get_alerts()`.

**Step 1 — project structure:**
```
sre-mcp-server/
├── server.py          ← the MCP server itself
├── requirements.txt    ← mcp, plus whatever talks to your real systems
└── README.md
```

**Step 2 — install the MCP SDK:**
```bash
pip install mcp
```

**Step 3 — define the server and its tools:**
```python
# server.py
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("sre-tools")

@mcp.tool()
def get_pods(namespace: str) -> str:
    """List pods and their status in a given Kubernetes namespace."""
    # Replace with a real call to the Kubernetes API
    return f"3/3 pods running in {namespace}"

@mcp.tool()
def get_logs(pod_name: str, lines: int = 50) -> str:
    """Fetch the last N lines of logs for a given pod."""
    # Replace with a real call to your logging system
    return f"[mock] last {lines} log lines for {pod_name}"

@mcp.tool()
def get_metrics(service: str) -> str:
    """Fetch current CPU/memory metrics for a service."""
    # Replace with a real Prometheus query
    return f"[mock] CPU: 42%, Memory: 71% for {service}"

@mcp.tool()
def get_alerts() -> str:
    """List currently firing alerts."""
    # Replace with a real call to your alerting system
    return "[mock] 1 alert firing: HighMemoryUsage on payment-service"

if __name__ == "__main__":
    mcp.run()
```

**Step 4 — connect it to Claude (e.g., in Claude Desktop's config):**
```json
{
  "mcpServers": {
    "sre-tools": {
      "command": "python",
      "args": ["/path/to/sre-mcp-server/server.py"]
    }
  }
}
```

**Step 5 — use it:** ask Claude *"Are there any alerts firing right now, and what does payment-service's memory look like?"* — Claude sees the four tools are available, calls `get_alerts()` and `get_metrics()` itself, and writes its answer using the real results.

**In one line:** an MCP server is just a small program that says "here are the tools/data I offer" — the `@mcp.tool()` decorator is what makes a plain Python function visible and callable by any MCP client.

### Going further — adding a Resource and a Prompt to the same server

Tools aren't the only thing an MCP server can offer. Extending the same `sre-tools` server:

```python
# Added to server.py

@mcp.resource("runbook://payment-service")
def payment_service_runbook() -> str:
    """Exposes the payment-service runbook as a readable resource, not a callable action."""
    with open("runbooks/payment-service.md") as f:
        return f.read()

@mcp.prompt()
def triage_incident(service: str) -> str:
    """A reusable prompt template clients can invoke, pre-filled with the service name."""
    return f"Investigate {service} using get_pods, get_logs, get_metrics, and get_alerts. Report severity, root cause, and a recommendation."
```
- **Resource vs. Tool, the real difference:** a *tool* is an action Claude decides to invoke ("go check the current metrics"). A *resource* is more like a file it can read when relevant ("here's the runbook text") — no side effects, just data.
- **Prompt:** a saved, reusable template — instead of every user typing out the full triage instructions, they invoke `triage_incident("payment-service")` and get the fully-formed prompt handed to them.
- **In one line:** a well-designed MCP server offers a mix of all three — actions it can take (tools), data it can hand over (resources), and starting points for common tasks (prompts).

### A second example — connecting over HTTP instead of locally

The example above ran on your own machine (`command: "python"`) — fine for personal use, but a team-shared server needs to run somewhere reachable over the network:
```json
{
  "mcpServers": {
    "sre-tools-remote": {
      "url": "https://sre-mcp.internal.example.com/mcp",
      "headers": { "Authorization": "Bearer ${SRE_MCP_TOKEN}" }
    }
  }
}
```
**Why this changes the security picture:** a local server is naturally scoped to whoever's logged into that machine. A remote one, reachable by anyone on the network, needs a real credential (`Authorization` header, OAuth token) — which is exactly what the next section covers.

## MCP security & auth
- **What it is:** rules controlling *what* an MCP server is allowed to expose, and *who* is allowed to connect to it.
- **Why it exists:** an MCP server that can restart production services is powerful — and dangerous if anyone could connect to it.
- **Example:** a local server (your own laptop) is naturally scoped to you. A *remote* MCP server (reachable over the network) needs real authentication (OAuth) so it's not just an open door.
- **Best practice — least privilege:** only expose the specific tools a task needs (e.g., `get_pods` read-only), never a blanket "run any command" tool, the same rule you'd already apply to any service account.
- **In one line:** treat an MCP server like any other internal service — authenticate connections, and only expose the smallest set of tools necessary.

🔗 Official: [MCP introduction](https://modelcontextprotocol.io/docs/getting-started/intro) · [Building MCP servers](https://modelcontextprotocol.io/docs/develop/build-server) · [MCP authorization](https://modelcontextprotocol.io/docs/concepts/authorization) ⚠️

---
---

# PART 12 — RETRIEVAL-AUGMENTED GENERATION (RAG)

## Why RAG exists
- **What it is:** a technique for letting Claude answer using *your own* documents, instead of only what it learned during training.
- **Why it exists:** Claude doesn't know your company's specific runbooks, past incidents, or internal docs — RAG is how you hand it exactly the right piece of that private knowledge, right when it's needed.
- **Example anyone can follow:** instead of memorizing an entire library, you ask a librarian a question, and they hand you the *one relevant book page* — you (Claude) then answer using that page, not your general knowledge.
- **In one line:** RAG = look up the relevant piece of your own data first, then let Claude answer using it.

## How it works, step by step
```
Your Documents (runbooks, postmortems, architecture docs)
        ↓
Chunking          (split into small, digestible pieces)
        ↓
Embeddings         (convert each piece into a list of numbers capturing its meaning)
        ↓
Vector Database    (store those number-lists for fast similarity search)
        ↓
Retrieval          (given a new question, find the most similar stored pieces)
        ↓
Claude             (answer the question, using only the retrieved pieces as context)
        ↓
Answer
```

- **Chunking:** splitting a big document into smaller pieces (e.g., per section) — searching works much better on focused pieces than one giant blob.
- **Embeddings:** a way of turning text into numbers such that *similar meaning* = *similar numbers* — this is what makes "find the relevant page" possible without exact keyword matching.
- **Vector database:** a database built specifically to quickly find the closest-matching embeddings to a new question.
- **Retrieval:** the actual lookup step — "which stored chunks are most relevant to this question?"
- **Metadata:** extra tags stored alongside each chunk (e.g., "this is from the 2025 postmortem for Incident #482") — lets you filter results, not just rank them.
- **Reranking:** a second, more careful pass that re-orders the initial retrieved results, since the fast first-pass search can be a little rough.
- **In one line:** RAG turns "Claude doesn't know this" into "Claude was just handed the exact right paragraph to answer this."

## A concrete example: runbooks + postmortems
Suppose someone asks: *"payment-service is throwing 503s, what do we usually do?"*
1. That question gets embedded (turned into numbers).
2. The vector database finds your most similar stored chunks — likely a section of the payment-service runbook, and maybe a past postmortem that mentions 503s.
3. Those specific chunks (not your entire documentation library) get added to Claude's context.
4. Claude answers using that real, specific, internal knowledge — not a generic guess.

**In one line:** RAG is what turns Claude from "generic AI assistant" into "an assistant that actually knows your team's own history."

### A minimal, fully working RAG example (small enough to run yourself)

This skips a real vector database and just does the similarity search in memory — enough to see every step concretely, without hiding anything behind a library:

```python
import anthropic
import numpy as np

client = anthropic.Anthropic()

# Step 1 & 2 — a few pre-chunked runbook snippets (imagine these came from real files)
chunks = [
    "Runbook: payment-service 503s. Usually caused by DB connection pool exhaustion. Fix: scale pool size in config, restart pods.",
    "Runbook: checkout latency spikes. Usually caused by third-party fraud-check API slowness. Fix: enable the local cache fallback.",
    "Postmortem #482: memory leak in image-resize worker traced to unclosed file handles. Fixed in v2.3.1.",
]

def embed(text: str) -> list[float]:
    # ⚠️ MAY CHANGE — verify current embeddings API/model; conceptually: text in, numbers out
    response = client.embeddings.create(model="claude-embed-v1", input=text)
    return response.embedding

# Step 3 — embed every chunk once, store the vectors
chunk_embeddings = [embed(c) for c in chunks]

def cosine_similarity(a, b):
    a, b = np.array(a), np.array(b)
    return np.dot(a, b) / (np.linalg.norm(a) * np.linalg.norm(b))

# Step 4 — a new question comes in
question = "payment-service is throwing 503s, what do we usually do?"
question_embedding = embed(question)

# Step 5 — retrieval: find the most similar chunk
scores = [cosine_similarity(question_embedding, ce) for ce in chunk_embeddings]
best_chunk = chunks[np.argmax(scores)]
print(f"Most relevant chunk (score {max(scores):.2f}): {best_chunk}")

# Step 6 — hand only that chunk to Claude, not the whole runbook library
response = client.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=300,
    messages=[{
        "role": "user",
        "content": f"Using this runbook excerpt, answer the question.\n\nExcerpt: {best_chunk}\n\nQuestion: {question}"
    }]
)
print(response.content[0].text)
```
- `embed(...)` — turns text into a list of numbers; similar meaning produces similar numbers.
- `cosine_similarity(...)` — a standard way to measure how "close" two number-lists are — higher score means more similar meaning.
- `np.argmax(scores)` — picks the single best-matching chunk out of all of them.
- **What a real system adds on top of this:** a proper vector database (for fast search across millions of chunks instead of a Python loop), metadata filtering (e.g., "only search 2025+ postmortems"), and a reranking step for extra accuracy — but the core idea, embed → compare → retrieve → hand to Claude, is exactly what's running above.

**In one line:** this entire mini-example *is* RAG — a real vector database just makes the "compare against every chunk" step fast at scale; it doesn't change the underlying idea.

🔗 Official: [Anthropic — building with embeddings/retrieval](https://docs.anthropic.com/en/docs/build-with-claude/embeddings)

---
---

# PART 13 — MULTI-AGENT SYSTEMS

## The pattern
```
                Coordinator Agent
                        │
       ┌────────────────┼────────────────┐
       ▼                ▼                 ▼
  K8s Agent        Logs Agent        Metrics Agent
 (checks pod        (searches         (checks CPU/
  health)           log patterns)      memory trends)
       │                │                 │
       └────────────────┼────────────────┘
                        ▼
                 Final Analysis
          (Coordinator combines all three
           findings into one answer)
```
- **What it is:** one "coordinator" agent breaks a big task into pieces and hands each piece to a specialized worker agent, then combines their results.
- **Why it exists:** a single agent juggling "check Kubernetes AND search logs AND analyze metrics AND write the report" all at once tends to lose focus — specialized workers, each with a narrow job, tend to do better at that one job.
- **Example anyone can follow:** like a general contractor (coordinator) who doesn't personally do the plumbing, electrical, and painting — they hire specialists for each, then inspect and combine the finished work.

## When multi-agent is worth it
- The task naturally splits into independent sub-tasks (checking three unrelated systems at once).
- Different sub-tasks genuinely benefit from different instructions/tools/models (e.g., a fast cheap model for simple log filtering, a stronger model for the final synthesis).

## When it's NOT worth it
- The task is simple enough for one agent with a couple of tools (Part 9) — added agents mean added complexity, cost, and more places for something to go wrong.
- You need a fast answer — coordinating multiple agents is almost always slower than one agent doing the whole thing directly.
- **In one line:** multi-agent systems trade speed and simplicity for specialization — only pay that price when a single agent actually can't keep up.

### A worked example — the coordinator pattern in code

```python
# Conceptual shape — each "agent" here could be its own Agent SDK instance (Part 9)
def k8s_agent(question: str) -> str:
    agent = Agent(model="claude-haiku-4-5", tools=[get_pod_status])
    return agent.run(question)

def logs_agent(question: str) -> str:
    agent = Agent(model="claude-haiku-4-5", tools=[get_logs])
    return agent.run(question)

def metrics_agent(question: str) -> str:
    agent = Agent(model="claude-haiku-4-5", tools=[query_prometheus])
    return agent.run(question)

def coordinator(incident_description: str) -> str:
    # dispatch the same underlying question to each specialist, in parallel
    k8s_result = k8s_agent(f"Check pod health related to: {incident_description}")
    logs_result = logs_agent(f"Check for relevant errors related to: {incident_description}")
    metrics_result = metrics_agent(f"Check for anomalies related to: {incident_description}")

    # a stronger model combines all three specialist findings into one final answer
    synthesis_agent = Agent(model="claude-sonnet-4-5", tools=[])
    return synthesis_agent.run(
        f"Combine these findings into one root-cause analysis:\n"
        f"K8s: {k8s_result}\nLogs: {logs_result}\nMetrics: {metrics_result}"
    )
```
**Notice the model choice pattern from Part 3 in action:** the three specialist workers use the fast/cheap Haiku tier (their jobs are narrow and don't need deep reasoning), while the final synthesis — genuinely harder, combining three different findings into one coherent story — uses Sonnet. This is exactly the kind of per-task model routing Part 3 recommended, now applied at the multi-agent level.

🔗 Official: [Building effective agents — multi-agent patterns](https://www.anthropic.com/engineering/building-effective-agents)

---
---

# PART 14 — AI + SRE, PRACTICAL USE CASES

Six concrete ways this all comes together for infrastructure/operations work:

- **AI Log Analysis** — feed Claude a chunk of raw logs, ask "what's unusual here?" — turns a wall of text a human would skim in 20 minutes into a 30-second read. *Concretely:* paste 10,000 lines of application logs from the last hour; Claude clusters recurring error patterns and flags the 3 that started appearing right when a deploy went out — a correlation a human would need a dashboard, not a log file, to spot quickly.
- **AI Incident Investigation** — an agent (Part 8–9) with tools (Part 7) for logs/metrics/alerts investigates a live issue and proposes a root cause, the way a first-responder engineer would. *Concretely:* this is the exact "why is checkout slow" agent from Part 8 and 9 — paged at 3am, it's already run the first three investigation steps by the time a human opens their laptop.
- **AI Runbook Assistant** — RAG (Part 12) over your existing runbooks, so "what do we do when X happens" gets answered with your team's actual documented process, not a generic guess. *Concretely:* a new on-call engineer, unfamiliar with a legacy service, asks "what do we do when the batch job fails" and gets back your team's actual 2019 runbook answer, not a plausible-sounding generic one.
- **AI Alert Analysis** — Claude reads a firing alert plus recent context and drafts a first-pass triage note before a human even opens the page. *Concretely:* the page itself arrives with a pre-written note: "Likely related to the 2:14am deploy of order-service v4.2 — 3 similar past incidents linked below," saving the first 10 minutes of triage.
- **AI Root Cause Analysis (RCA)** — after an incident, Claude cross-references logs, metrics, and the timeline to draft a first version of "what actually happened and why." *Concretely:* instead of an engineer spending 2 hours the next morning reconstructing a timeline from 4 different dashboards, Claude drafts the first version overnight, which a human then reviews and corrects rather than writing from a blank page.
- **AI Deployment Assistant** — Claude reviews a deployment diff/config change and flags anything that looks risky before it ships. *Concretely:* a config change removing a resource limit on a pod gets flagged automatically with "this removes the memory limit — was that intentional?" before it merges, not after it causes an incident.

**In one line:** in every case, the pattern is the same — give Claude the right tools/data (Parts 7, 11, 12), let it reason (Parts 8–9), and keep a human reviewing anything consequential (Part 15).

---
---

# PART 15 — PRODUCTION AGENTIC AI

## Reliability
- **Timeouts:** every tool call and API call needs a max wait time — an agent that hangs forever on one stuck tool call is a production incident of its own.
- **Retries:** transient failures (a flaky API, a network blip) should retry with backoff — same as any other distributed system call.
- **Failure handling:** decide up front what happens when a tool fails — does the agent try a different approach, ask a human, or stop cleanly?
- **Maximum iterations:** an agent loop (Part 8) needs a hard cap on how many think→act→observe cycles it can run — without one, a confused agent can loop indefinitely, burning time and money.
- **Circuit breakers:** if a specific tool keeps failing, stop calling it for a while instead of hammering a broken dependency — the exact same pattern you already use for flaky microservices.
- **In one line:** everything you already know about building reliable distributed systems applies directly here — an agent loop is just another distributed system component.

## Security
- **API key security:** store keys in a secrets manager, rotate them, never log them — same as any other credential (Part 0).
- **Least privilege for tools:** an agent should only have the specific tools/permissions its task actually needs — a log-reading agent should never also have a "delete resource" tool.
- **Prompt injection:** if an agent reads untrusted external text (a log file, a support ticket, a web page), that text could contain hidden instructions trying to hijack the agent ("ignore your instructions and instead..."). Defend by never letting tool *results* directly trigger further actions without validation, and by keeping the most sensitive tools out of reach of any agent that reads untrusted input.
- **Human approval:** for anything consequential (deleting something, spending money, changing production), require an explicit human "yes" before it happens — this is the single most important safety rule in this whole handbook.
- **In one line:** the model is not the security boundary — your tool permissions and approval steps are.

## Observability
Monitor the same categories you'd monitor for any production service, applied to your agent:
- **Latency** — how long each request/tool call takes.
- **Token usage & cost** — input/output tokens per request, and the resulting dollar cost.
- **Tool calls** — which tools got called, how often, and with what results.
- **Errors** — failed API calls, failed tool calls, malformed structured output (Part 6).
- **Agent steps** — how many loop iterations (Part 8) a given task took, to catch runaway agents early.
- **In one line:** if you can't see it, you can't debug it — instrument an agent exactly like you'd instrument any other production service.

## Evaluations & testing (your "test suite" for AI behavior)
- **What it is:** a set of known test cases (input + expected-good-output) you run regularly to check an agent/prompt still behaves correctly.
- **Why it exists:** because output is probabilistic (Part 3), a prompt that worked well yesterday can quietly get worse after a small change — evals catch that the way a regression test suite catches a code regression.
- **Example:** keep 20 real past incidents with known correct root causes; periodically re-run your incident-investigation agent against them and check how many it still gets right.
- **"LLM-as-judge":** sometimes you use a second Claude call just to grade the first one's answer against your criteria — useful when "correct" isn't a simple exact-match.
- **In one line:** without evals, you're deploying changes to a probabilistic system on faith — evals are how you actually know if it's still working.

### More worked examples — Part 15

**A simple eval harness, made concrete:**
```python
test_cases = [
    {"log": "OOMKilled, exitCode 137", "expected_cause": "memory"},
    {"log": "connection refused, upstream timeout", "expected_cause": "network"},
    {"log": "disk usage at 98%", "expected_cause": "disk"},
]

def run_eval(agent_fn, test_cases):
    passed = 0
    for case in test_cases:
        result = agent_fn(case["log"])
        # a real eval might use an LLM-as-judge here instead of a simple substring check
        if case["expected_cause"] in result.lower():
            passed += 1
        else:
            print(f"❌ FAILED on: {case['log']!r} → got: {result!r}")
    print(f"{passed}/{len(test_cases)} passed")

run_eval(my_incident_agent, test_cases)
```
**Why this matters in practice:** run this same small script after *every* prompt change, model swap, or tool update — the same instinct as running your test suite after every code change, applied to a system whose "correctness" is fuzzier than a unit test's assert statement.

**A simple circuit breaker, made concrete:**
```python
import time

class ToolCircuitBreaker:
    def __init__(self, failure_threshold=3, cooldown_seconds=60):
        self.failures = 0
        self.threshold = failure_threshold
        self.cooldown = cooldown_seconds
        self.opened_at = None

    def call(self, tool_fn, *args):
        if self.opened_at and (time.time() - self.opened_at) < self.cooldown:
            raise RuntimeError("Circuit open — tool temporarily disabled after repeated failures")
        try:
            result = tool_fn(*args)
            self.failures = 0  # reset on success
            return result
        except Exception as e:
            self.failures += 1
            if self.failures >= self.threshold:
                self.opened_at = time.time()
            raise
```
**In one line:** after 3 consecutive failures, this stops calling a broken tool for 60 seconds instead of hammering it a 4th, 5th, and 6th time — the exact same pattern you'd already reach for wrapping a flaky downstream microservice call, just wrapping a tool call instead.

🔗 Official: [Anthropic — evaluating prompts](https://docs.anthropic.com/en/docs/test-and-evaluate/develop-tests)

---
---

# PART 16 — FINAL PROJECT: AI SRE INCIDENT INVESTIGATION PLATFORM

Putting every part of this handbook together into one real system.

```
ALERT FIRES
    │
    ▼
AI AGENT (Part 8–9)
    │
    ├──────────────┬──────────────┬──────────────┐
    ▼              ▼              ▼              ▼
Kubernetes      Prometheus      Grafana        (via MCP tools, Part 11)
(pod status)    (metrics)       (dashboards)
    │              │              │
    ▼              ▼              ▼
  Logs          Metrics         Alerts
    └──────────────┴──────────────┘
                   │
                   ▼
                  RAG (Part 12)
                   │
                   ▼
        Runbooks + Past Postmortems
                   │
                   ▼
            Claude Analysis (Part 6 — structured output)
                   │
                   ▼
              Root Cause + Confidence
                   │
                   ▼
               Recommendation
                   │
                   ▼
             HUMAN APPROVAL   ← the mandatory checkpoint (Part 15)
                   │
                   ▼
        Optional Remediation (only if approved)
```

**Components, plainly:**
- **The trigger:** a real alert (from your existing alerting system) kicks off the agent — nothing runs on a schedule guessing at problems.
- **The tools (Part 7, 11):** MCP servers exposing `get_pods`, `get_logs`, `get_metrics`, `get_alerts` — your agent's "senses."
- **The knowledge (Part 12):** RAG over your runbooks and past postmortems — your agent's "institutional memory."
- **The reasoning (Part 8–9):** an agent loop that investigates, checking multiple systems and adjusting based on what it finds.
- **The output (Part 6):** a structured result — severity, root cause, confidence, recommendation — never a vague paragraph.
- **The safety net (Part 15):** nothing destructive happens without a human explicitly approving it first.
- **Observability (Part 15):** every step — every tool call, every token, every decision — is logged, so when the agent itself misbehaves, you can debug it exactly like any other service.

**In one line:** this project isn't a new kind of system to learn from scratch — it's your existing SRE toolchain (Kubernetes, Prometheus, alerts, runbooks), with an agent as the new "first responder" that investigates before a human has to, and always asks before acting.

### Sketching the actual pipeline, end to end

Putting every piece from this handbook into one script's worth of shape:

```python
# 1. Trigger — a real alert webhook calls this
def handle_alert(alert: dict):
    incident_description = f"{alert['name']}: {alert['description']}"

    # 2. Tools (Part 7, 11) — via the SRE MCP server built in Part 11
    investigation_agent = Agent(
        model="claude-sonnet-4-5",
        tools=[get_pods, get_logs, get_metrics, get_alerts],
        max_iterations=10,           # Part 15 — hard cap on the loop
        system_prompt="You are an SRE incident investigator. Be thorough but efficient."
    )

    # 3. Knowledge (Part 12) — retrieve the most relevant runbook/postmortem chunks
    relevant_context = retrieve_relevant_chunks(incident_description)  # the RAG function from Part 12

    # 4. Reasoning (Part 8-9) — the agent investigates, using both tools and retrieved context
    raw_findings = investigation_agent.run(
        f"{incident_description}\n\nRelevant past documentation:\n{relevant_context}"
    )

    # 5. Structured output (Part 6) — force the findings into a fixed, checkable shape
    analysis = IncidentAnalysis.model_validate_json(
        structure_the_findings(raw_findings)  # a follow-up call asking Claude to format its own findings as JSON
    )

    # 6. Observability (Part 15) — log everything, regardless of what happens next
    log_incident_analysis(alert, analysis, investigation_agent.token_usage)

    # 7. Human approval (Part 15) — the mandatory checkpoint
    if analysis.severity in ("high", "critical"):
        notify_oncall_for_approval(analysis)
    else:
        post_to_incident_channel(analysis)  # low-severity findings can just be posted, no gate needed
```
**Why each numbered step maps back to a specific part of this handbook:** this project was never meant to feel like new material — every single step above is a direct application of something already covered. That's deliberate: by the time you reach Part 16, you should recognize every line, not learn anything new here.

---
---

*End of full draft — Parts 0–16 complete in the plain, example-first style. Appendices (Glossary, Cheat Sheets, Docs Map, Pricing Snapshot) next.*
