# Dynamic Assessment Writing Mediator

An AI mediator that guides second language writers to correct their own work instead of
correcting it for them. Dynamic Assessment is my MA research field, and this project turns
that research into a working instrument.

> **Status: currently in development.** The mediation loop works end to end against a live
> model. The code is private while the app is under active development; this page documents
> the project, and I am happy to give a full walkthrough on request.

## The problem

Standard writing feedback measures what a learner can already do alone. A finished essay is
marked, errors are listed, and the learner is handed the corrections. This misses the more
useful question: what can this learner do with the right amount of help, and how much help
do they actually need?

Dynamic Assessment, which comes from Vygotsky's sociocultural theory, treats instruction and
assessment as a single activity. The mediator offers the least explicit hint that might work,
becomes more explicit only when the learner cannot self correct, and withdraws help as soon
as independence appears. The level of support a learner required is itself the measurement.

This application makes that method work at scale, with a language model doing the mediation.

## How the AI mediation works

The mediator follows the Aljaafreh and Lantolf (1994) regulatory scale, thirteen graduated
levels running from implicit noticing to explicit correction, across four subskills mapped
to the IELTS band descriptors: coherence, task achievement, grammar, and vocabulary.

Each turn, the model receives a layered context: the method, the exam, the task, the topic,
the question, a text description of any visual, and the learner's current draft. It returns
one mediation move at a time, together with structured signals the application acts on:

- the exact phrase it is pointing at, which the app highlights inside the learner's own writing
- the explicitness level it used, which sets the visual intensity of that highlight
- a structured correction once an issue is genuinely resolved, which the learner can apply
- a signal for when the learner is ready to revise their text

Applied fixes are fed back on later turns as a resolved ledger, so the mediator does not
revisit settled issues. Every exchange is logged for analysis.

## The API integration layer

The app is built around a provider agnostic AI layer rather than a single vendor SDK.

- Mediation runs through Namirasoft Inference, an LLM orchestration API, called
  asynchronously with polling, and the same layer is written to work against OpenAI and
  Anthropic compatible endpoints.
- The provider, endpoint, model, and key are all configurable at runtime from the
  researcher panel, so switching models requires no code change.
- The model's replies follow a structured output contract: the error span, the mediation
  level, the fix token, and the ready to edit signal are machine readable parts of each
  response, parsed and acted on by the app rather than displayed as raw text.
- Context is engineered per call: a layered first message, per level overrides, a resolved
  issue ledger, and three switchable context strategies (memory, compound, and full) that
  trade token cost against continuity.

## What works today

- The full mediation loop with a live model: graduated hints, one issue at a time
- The regulatory scale, contingency rules, and self regulation aim encoded in the system prompt
- Error highlighting in the learner's own text, with intensity tied to the mediation level
- Scoped editing, structured fixes, and change tracing on the essay
- IELTS Task 1 and Task 2 content, with Task 1 visuals generated locally as SVG
- Autosave and resume, so an interrupted session is not lost
- A timed pretest that sets each learner's starting level from real writing
- Session logging with full transcripts, and a progress view with band and skill charts
- A hidden researcher panel for editing the mediation prompt, context layers, and AI settings

## What is planned

- AI scoring of writing (current band scores are a placeholder heuristic)
- The independence and Zone of Proximal Development visualisation
- Per task session timers, a consent flow, data export, and a researcher dashboard
- Accounts, authentication, and a database (it currently runs single user and local)
- Exams beyond IELTS: TOEFL, B2 First, C1 Advanced, PTE

## Screenshots

### The writing session
Writing stays on the left, mediation on the right, so the learner never loses sight of their own text.

![Writing session](screenshots/01-writing-session.png)

### A mediated error
The mediator points at one issue at a time. The highlight sits in the learner's own writing, and
its intensity reflects how explicit the hint was.

![Mediated error](screenshots/02-mediated-error.png)

### Applying a resolved fix
When an issue is genuinely resolved, the correction can be applied to the essay and is traced in
the text, so the learner can see what changed.

![Applied fix](screenshots/03-applied-fix.png)

### The journey
Progress across sessions, with band, skill breakdown, and the full transcript of every session.

![Journey](screenshots/04-journey.png)

## Stack

Node.js with no framework and no dependencies, vanilla HTML, CSS, and JavaScript with no
build step, and local JSON files for storage. Mediation runs through Namirasoft Inference,
an LLM orchestration layer, with support also written for OpenAI and Anthropic compatible
endpoints. The application is about 1,700 lines and runs on one machine.

## Background

Dynamic Assessment is the field of my MA research (Applied Linguistics), including published,
peer reviewed work on developing second language oral fluency through dynamic assessment.
This project extends that research from speaking to writing, with an AI mediator in place of
a human one.

## Contact

Younes Ebrahimzadeh
yones.ebrahimzadeh@yahoo.com | [LinkedIn](https://www.linkedin.com/in/younes-ebrahimzadeh-372a52156)
