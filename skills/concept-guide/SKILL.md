---
name: concept-guide
description: >
  Use this skill whenever a user asks you to explain a technical concept, topic, or technology —
  especially anything coding or programming related. Triggers include: "explain X", "how does X work",
  "teach me X", "what is X", "I don't understand X", "walk me through X", "show me how to X",
  "help me learn X", or any request to understand something new. Always use this skill when
  the explanation would benefit from a step-by-step guide or example code. Do NOT skip this
  skill just because a topic seems simple — even basic concepts benefit from this structured
  format. Also triggers for: "how do I get started with X", "what's the difference between
  X and Y", "give me an example of X", "write a template for X". This skill is for technical
  concepts only — software engineering, programming, and adjacent technologies.
---

# Concept Guide Skill (Technical)

This skill produces a consistent, three-part learning response for any technical concept the user wants to understand.

---

## Step 0 — Read the Difficulty Flag

Before writing anything, check if the user specified a difficulty level. They may say it explicitly ("explain it like I'm a beginner") or it may be implied by their phrasing.

| Signal | Level | What to do |
|---|---|---|
| "beginner", "I'm new", "ELI5", simple vocabulary in their message | 🟢 Beginner | No jargon at all. Heavy analogies. Explain every term. Short steps. |
| No signal given (default) | 🟡 Intermediate | Some jargon OK if briefly defined. Balanced depth. |
| "advanced", "I know the basics", "deep dive", technical phrasing | 🔴 Advanced | Skip basics. Go straight to nuance, edge cases, internals, tradeoffs. |

State the detected level at the top of your response on one line:
`📊 Level: 🟢 Beginner` (or 🟡 / 🔴)

If unsure, default to 🟡 Intermediate and add: *"Let me know if you want this simpler or more in-depth."*

#### Code Style by Difficulty Level

Once difficulty is set, apply these constraints to ALL code examples:

| Aspect | 🟢 Beginner | 🟡 Intermediate | 🔴 Advanced |
|---|---|---|---|
| Language features | Basic only (if/else, loops, functions) | Standard library, some async | Idiomatic patterns, generics, metaprogramming |
| Code density | 1 idea per line, verbose | Balanced — clear but concise | Terse, idiomatic, uses advanced syntax |
| Error handling | Shown but not required | Required with try/catch/handle | Comprehensive with custom error types |
| Generics / type params | None | Simple type hints | Full generic constraints |
| Async/concurrency | Avoid | Basic usage (async/await) | Advanced patterns (racing, pooling, streams) |
| Dependencies | Minimal — standard lib only | Common well-known packages | Specialized or version-specific libs |

---

## Step 0.5 — Detect Language & Genre

Before writing any code, scan the conversation for signals about:

- **Language** — detect the programming language from user's question, past messages, or any code they've shared. Default to **Python** if none found, but state the assumption explicitly.
- **Genre** — determine the user's domain from their question, then use it to guide language choice, template style, and ecosystem references throughout the response.

| Genre | Common Languages | Template Focus | Ecosystem Context |
|---|---|---|---|
| backend | Python, Go, Rust, Node.js | APIs, DB queries, auth, middleware, caching | Web frameworks (FastAPI, Express), ORMs, message queues |
| frontend | TypeScript, JavaScript, CSS, HTML | Components, state management, styling, routing, a11y | React, Vue, Angular, build tools (Vite, Webpack) |
| machine learning | Python, R | Model training, data pipelines, inference, feature engineering | PyTorch, TensorFlow, scikit-learn, Hugging Face |
| fullstack | TypeScript, Python, Go | End-to-end features, auth flow, data flow, SSR/SSG | Next.js, Django, tRPC, Prisma, Tailwind |
| data analyst | Python, SQL, R | ETL, aggregation, visualization, reporting | pandas, Polars, dplyr, Jupyter, Metabase |
| automation | Python, bash, PowerShell | Scripts, scheduled tasks, API orchestration, scraping | Playwright, cron, CI workflows, systemd timers |
| devtools | TypeScript, Rust, Go | CLI interfaces, SDK wrappers, plugins, code transforms | Vite, esbuild, LSP protocols, WASM |
| devops | HCL, YAML, Go, Python | Infrastructure, deployment pipelines, containers, observability | Terraform, Docker, Kubernetes, ArgoCD, Prometheus |
| mobile | Kotlin, Swift, TypeScript (React Native) | Screens, navigation, local storage, platform APIs | Jetpack Compose, SwiftUI, Expo |
| security | Python, Go, Rust | Threat modeling, auth flows, input validation, crypto | OWASP dependencies, JWT libraries, HashiCorp Vault |
| game dev | C#, C++, Rust | Game loops, ECS/component patterns, rendering | Unity, Unreal, Bevy, Godot |

**Detection rules:**
- Use the user's question, surrounding conversation, and any code they've shared
- Pick the **single best-matching genre** — don't list all possibilities
- If multiple genres apply (e.g. "how do I build a fullstack app"), default to **fullstack**
- If no genre can be determined, use **language-only** with no genre specialization
- State the detected genre at the top: `🎯 Genre: [genre]` (only if detected)

---

## Step 0.75 — Scope Gate: Quick Concept vs Full Deep-Dive

Before generating the full output, decide whether this concept needs a **mini response** or the **full treatment**.

| Trigger | Verdict | Output |
|---|---|---|
| Concept is trivially simple (variable, loop, if/else, basic syntax) | 🟢 Quick Concept | Abbreviated format — brief explanation + cheat sheet + test questions. Skip broken code exercise and starter template. |
| User asks "what is X" with no follow-up context | 🟢 Quick Concept | Abbreviated format. |
| User asks for tutorial, guide, walkthrough, or "help me learn X" | 🔴 Full Deep-Dive | Full output — everything in Output Structure below. |
| Concept has significant depth or multiple moving parts | 🔴 Full Deep-Dive | Full output. |

Flag the verdict at the top on its own line:
`⚡ Quick Concept` or `📘 Full Deep-Dive`

For **🟢 Quick Concept**, use this abbreviated format:
- 📊 Level + 🎯 Genre lines
- 1–2 paragraph plain-English explanation (with analogy)
- ⛔ When NOT to use (2 bullets max)
- 📋 Cheat Sheet (full)
- 🧠 Test Yourself (2 questions)
- 🔍 Perplexity prompt
- 📍 What to Learn Next

For **🔴 Full Deep-Dive**, produce the complete output structure below.

---

## Output Structure

Always produce sections in this exact order (unless scope gate says otherwise):

### 1. Concept Explanation

Open with a clear, plain-English explanation of what the concept is and why it matters. Cover:

- **What it is** — define it simply, no jargon unless immediately explained
- **Why it exists / what problem it solves** — give the "aha" motivation
- **How it fits into the bigger picture** — mention related ideas briefly

Keep this section conversational and digestible. Aim for 3–6 short paragraphs. Do NOT use bullet-heavy walls — write mostly in prose.

#### 🏦 Analogy Bank (required)

Every concept explanation MUST include at least one analogy — this is not optional. Analogies are the fastest bridge from confusion to understanding.

Rules:
- Draw from everyday life (kitchens, traffic, libraries, restaurants — avoid tech analogies when explaining tech)
- Scale to difficulty level: concrete and simple for 🟢, more abstract for 🔴
- Format as a clearly labelled block so it stands out visually:

```
💡 Analogy: Think of [CONCEPT] like a [REAL-WORLD THING].
[2–3 sentences mapping the analogy onto the concept.]
⚠️ Where this breaks down: [one sentence on what the analogy doesn't capture.]
```

Always include the "where this breaks down" line — it prevents over-extending the analogy and models honest thinking.

---

### 2. Step-by-Step Guide

Provide a numbered step-by-step guide. Each step should:

- Have a **bold title** (e.g., **Step 1 — Install the package**)
- Include a 1–3 sentence explanation of *what* you're doing and *why*
- Include a code block immediately after the explanation if that step involves writing code
- Be ordered so a complete beginner could follow from top to bottom

---

### 2.5 — ⛔ When NOT to Use This

After the step-by-step guide, always include a "When NOT to use this" section. This is one of the most skipped parts of tutorials and one of the most valuable things a learner can know.

Format:

```
⛔ When NOT to use [CONCEPT]:

- [Situation 1] — [one sentence why this concept is the wrong tool here, and what to use instead]
- [Situation 2] — [same]
- [Situation 3] — [same]

🔁 Better alternatives when [CONCEPT] doesn't fit:
→ [Alternative A] — good when [condition]
→ [Alternative B] — good when [condition]
```

Aim for 3–4 "when not to" bullets. Be specific — "don't use it in large projects" is too vague; "don't use it when you need type safety across a team codebase — use TypeScript interfaces instead" is actionable.

---

### 3. Starter Code Template

After the guide, provide a **ready-to-use code template** the user can copy and start editing. The template must:

- Be complete enough to run or be dropped into a project without modification (add placeholder values where needed)
- Use comments generously — every non-obvious line should have a short inline comment
- Include a **guide block below each logical section** of code using comments, explaining:
  - What that section does
  - What the user should change or fill in
  - Any common mistakes to watch out for

#### Template format — comment header + empty writing space

Each section of the template follows this exact pattern:

```
// ── SECTION NAME ─────────────────────────────────────────
// What this does: <one sentence>
// What to write here: <what the user fills in>
// Common mistake: <one pitfall, if any>

                        ← empty line(s) here for the user to write their code
```

**Rules:**
- The comment block is the *header* for that section — it goes above the empty space, not below
- Leave 2–4 blank lines after each comment block so the user has visual room to write
- For sections that have a partial example (e.g. a function signature), put the signature, then a comment placeholder inside it, then leave space:

```
// ── MAIN LOGIC ───────────────────────────────────────────
// What this does: the core operation — fetch, transform, compute, etc.
// What to write here: your main function body
// Common mistake: not handling the case where the input is null/undefined

function doSomething(input) {
  // ✍️ write your logic here


}
```

- Use `// ✍️ write your logic here` as the universal "this is where you type" signal inside code bodies
- Adapt comment style to the language: `//` for JS/TS/Java/C, `#` for Python/Ruby/Bash, `<!-- -->` for HTML

Place one of these blocks for every logical section: imports, config/constants, helper functions, main logic, output/export, error handling.

---

## Tone & Style Rules

- Write as a knowledgeable friend, not a textbook
- Prefer short sentences. Break up dense ideas.
- Never skip the template for a coding topic — it's the most valuable part
- If the user's question is vague, make a reasonable assumption about what they want and state it at the top: *"I'll explain X as it's used in Y context — let me know if you meant something different."*
- Do not add a conclusion paragraph. The template is the ending.

---

## Example Trigger Mapping

| User says | What to do |
|---|---|
| "Explain async/await" | Full deep-dive: concept + steps + template |
| "What is a REST API?" | Concept explanation + steps (how to call one) + template (fetch example) |
| "What is a variable?" | Quick concept — brief explanation + cheat sheet only |
| "Explain if/else" | Quick concept |
| "Teach me React hooks" | Full deep-dive |
| "What's the difference between SQL and NoSQL?" | Concept explanation + optional comparison template |
| "Give me a Python template for reading a CSV" | Skip to template directly (user already knows the concept) |
| "How do I deploy a Docker container?" | Full deep-dive with infrastructure context |

---

## 4. End-of-Response Extras

Always append these after the template (or after the cheat sheet in quick concept mode). They turn a one-shot answer into a launchpad for deeper learning.

---

### 🐛 Broken Code Exercise

After the starter template, always include a broken code exercise. This is a short snippet with deliberate bugs the user must find and fix. It teaches debugging instincts, not just syntax. *(Skip this section for 🟢 Quick Concept responses.)*

Structure of the exercise:

```
─────────────────────────────────────────────────
🐛 Broken Code Exercise — find and fix the bugs

[Paste a 10–20 line code snippet with 2–3 deliberate errors]

Common errors included in this snippet:

❌ Error 1 — [short name]
   Line [N]: `[exact line of code with the bug]`
   Problem: [one sentence — what is wrong and why it fails]
   Fix direction: [hint without giving the answer — e.g. "check what type this returns"]

❌ Error 2 — [short name]
   Line [N]: `[exact line of code with the bug]`
   Problem: [one sentence]
   Fix direction: [hint]

❌ Error 3 — [short name, if applicable]
   Line [N]: `[exact line of code]`
   Problem: [one sentence]
   Fix direction: [hint]

✏️ Mini Exercise:
Fix all the errors above, then extend the code to also [one small addition that uses the concept — e.g. "handle a 404 response" or "make it work with an array of inputs"].
─────────────────────────────────────────────────
```

Rules:
- Bugs must be realistic — the kind of mistake a real beginner would actually make (forgetting `await`, wrong variable name, off-by-one, missing return, wrong method name, etc.)
- Include the **exact line number and exact code** so the user knows precisely what to look at
- Give a fix direction (hint), NOT the solution — the user figures it out
- The mini exercise extends the concept, not just fixes syntax
- Scale bug difficulty to the user's level: 🟢 = obvious typo-level bugs, 🟡 = logic errors, 🔴 = subtle edge-case bugs

---

### 📋 Cheat Sheet

Before the Perplexity prompt, always include a cheat sheet — a dense, copy-paste-into-notes summary of everything covered.

Format:

```
─────────────────────────────────────────────────
📋 Cheat Sheet — [CONCEPT]

What it is:          [one sentence definition]
Why use it:          [one sentence on the core use case]
When NOT to use:     [one sentence on the main anti-pattern]
Biggest misconception: [one sentence — what people assume but is wrong]

Version note:        [library/framework v.X+ or "language agnostic"]

Key syntax:
  [most important code pattern — 1–3 lines max]

Most common mistake:
  ❌ [wrong version — exact code]
  ✅ [right version — exact code]

Mental model:        [the analogy in 5 words or less]
Next step:           [one concept to learn after this]
─────────────────────────────────────────────────
```

Keep this brutally short. If it doesn't fit in under 25 lines, cut it. This is for a sticky note, not a document.

---

### 🔍 Perplexity Fact-Check & Expansion Prompt

At the very end of every response, generate a ready-to-paste Perplexity prompt the user can use to verify, expand, and find gaps in what was covered. Format it like this:

```
─────────────────────────────────────────────────
🔍 Paste this into Perplexity to go deeper:

"I just learned about [CONCEPT]. The explanation covered:
[bullet list of the 3–5 main points from this response].

Please:
1. Fact-check these points — are any outdated or incomplete?
2. What important aspects were NOT mentioned?
3. What are the most common misconceptions beginners have about this?
4. What should I learn next after understanding this?"
─────────────────────────────────────────────────
```

Fill in [CONCEPT] and the bullet points from your actual response. Make it specific — a generic prompt gets generic results.

---

### 🧠 "Test Yourself" Questions

After the template (or cheat sheet in quick concept mode), include 3 short questions the user can answer mentally (or out loud) to check their understanding. Format:

```
🧠 Test yourself:
1. [conceptual question — can they explain it in their own words?]
2. [applied question — what would happen if...?]
3. [debugging question — what's wrong with this code snippet? or: when would you NOT use this?]

If you'd like me to check your answers, reply with them and I'll review.
```

Keep questions punchy. Don't answer them — leave them open. The final line invites the user into an interactive loop.

---

### 🔗 "What to Learn Next" Map

End with a 3-item "what's next" list that gives the user a direction forward:

```
📍 What to learn next:
→ [Prerequisite they may have missed, if any]
→ [Natural next concept that builds on this one]
→ [Advanced version of this concept for when they're ready]
```

---

<!-- internal checklist — do not render -->
Before finalizing your response, verify every item below. Do not skip any section for full deep-dive topics.

**Setup**
- [ ] Difficulty level detected and labelled (🟢 / 🟡 / 🔴) at the top
- [ ] Genre detected and labelled (🎯 Genre) if identifiable
- [ ] Scope verdict (⚡ Quick Concept / 📘 Full Deep-Dive) decided
- [ ] Language detected or defaulted (explicitly stated)
- [ ] Code style scaled to difficulty level (see Code Style by Difficulty Level table)

**Explanation**
- [ ] Concept explained in plain English with motivation
- [ ] Analogy block present with 💡 label AND ⚠️ "where it breaks down" line

**Guide**
- [ ] Step-by-step guide present (if full deep-dive), each step has explanation + code block
- [ ] ⛔ "When NOT to use this" section with 3+ bullets and alternatives

**Template** (full deep-dive only)
- [ ] Starter template uses comment-header + empty-space format per section
- [ ] Every code body has `// ✍️ write your logic here` placeholder

**Exercises & Reference**
- [ ] (Full deep-dive) 🐛 Broken code snippet has 2–3 bugs, each with exact line + error name + fix direction
- [ ] (Full deep-dive) ✏️ Mini exercise at the end of the broken code section
- [ ] 📋 Cheat sheet under 25 lines, includes wrong vs right code example, biggest misconception, and version note
- [ ] (Full deep-dive) 🧠 3 "Test yourself" questions; 2 questions for quick concept
- [ ] Interactive answer-check offer included in test yourself section
- [ ] 📍 "What to learn next" map (prerequisite → next → advanced)
- [ ] 🔍 Perplexity prompt is specific to THIS response's actual content

**Style**
- [ ] No jargon left unexplained
- [ ] Tone is friendly, not academic
- [ ] Difficulty level is reflected throughout (depth, vocabulary, analogy complexity)
- [ ] Genre mode used to guide language choice and ecosystem references
- [ ] No conclusion paragraph — the template is the ending
