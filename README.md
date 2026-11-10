# The Obfuscation Eschewer

*Dialectic over eristic: the Socratic tradition, operationalised.*

A single-page web app that reads a workplace email or chat message, spots bad-faith argument tactics, suggests the most likely root cause behind them and drafts a calm, constructive reply.

**Live app:** `https://<your-username>.github.io/obfuscation-eschewer/` *(replace once GitHub Pages is enabled)*

---

## Why it exists

Most workplace arguments that go nowhere aren't settled on their merits. Someone appeals to an unnamed consensus, moves the goalposts or hides behind a deadline, and the conversation becomes about winning rather than being right. Aristotle called arguments that only *look* like refutations **sophistical**. Schopenhauer catalogued 38 stratagems for winning a dispute regardless of truth and called the practice **eristic**.

This tool turns those classical catalogues into a practical field guide. It is built to help you return a conversation to **dialectic**, a shared search for what is actually true. It never suggests counter-tactics, and it treats every finding as a hypothesis about the message, not a verdict on the person.

## How to use it

1. Paste an email or chat message into the **Analyse** view.
2. Optionally add one line of context (for example: "Peer in delivery; I wrote the proposal").
3. Choose a reply tone: **Diplomatic**, **Direct** or **Formal**.
4. Read the results:
   - **Flagged phrases:** the message with each trigger highlighted and numbered
   - **Tactics:** each one with the quoted phrase, a one-sentence explanation, a likely/possible confidence and a response move
   - **Likely root cause:** for example fear of blame, territory or sunk cost, with what tends to help
   - **Suggested reply:** acknowledges any legitimate concern, asks for evidence or specifics, keeps credibility intact on both sides and lowers the temperature
5. Fill in any `[bracketed]` parts of the reply, then copy it.

The **Field guide** view lists all 28 tactics, filterable by root cause and searchable.

## Two modes of analysis

| | Quick scan | Analyse with Claude |
|---|---|---|
| Where it runs | Entirely in your browser | Sends the message to Claude |
| Available on GitHub Pages | Yes | No (only inside Claude) |
| Method | Keyword patterns from the field guide | Reads tone, context and intent |
| Good-faith reading and legitimate concern | No | Yes |
| Reply | Assembled from the field guide's response moves | Written for the specific message, redrafted on tone change |
| Limitations | Misses unusual phrasing; can flag innocent wording | Can still misread; lists its uncertainties |

When the app runs outside Claude (for example on GitHub Pages), the Claude button hides itself and the quick scan and field guide work as normal.

## Principles

- **Assume good faith** wherever the text allows. A blunt or stressed message is not automatically eristic.
- **Flag uncertainty.** Every finding is marked *likely* or *possible*.
- **No manipulative counter-tactics.** Every response move aims to bring the argument back to evidence.
- **Nobody loses face.** Replies never name a tactic or accuse the sender.

## Privacy

- The quick scan sends nothing anywhere: no server, no analytics, no storage beyond your own browser (your last draft and tone are kept in local storage for convenience).
- **Analyse with Claude** sends the message and context to Claude. Remove names, client details and anything confidential first, and follow your organisation's policy on using AI tools with work content.

## The field guide

### Root causes

| Root cause | What it looks like | What helps |
|---|---|---|
| Fear of risk or blame | Worried about failure, exposure or being held responsible. | Lower the perceived risk: a pilot, a rollback plan, shared ownership of the decision. |
| Territory | Protecting remit, ownership or budget. | Credit their ownership and involve them in shaping the outcome. |
| Sunk cost | Invested in the current approach, personally or financially. | Honour what was built; compare options on future cost and value only. |
| Knowledge gap | Missing context or expertise, and possibly reluctant to say so. | Explain without condescension; offer a short briefing or walkthrough. |
| Status and face | Concerned about how they look to peers or seniors. | Give them a way to agree, or to change their mind, without losing face. |
| Overload | Under time or capacity pressure; resistance sheds work. | Make the ask smaller and the path to a decision faster. |
| Accountability avoidance | Avoiding ownership of a decision or its consequences. | Make decisions, owners and risk acceptance explicit in writing. |
| Loss of control | Uncomfortable with change they did not initiate. | Offer real choices and a role in the decision. |

### Tactics

| # | Tactic | Definition | Likely root causes | Lineage |
|---|---|---|---|---|
| 01 | Appeal to consensus | Claims a group already agrees, so the point needs no further argument. | Status and face, Fear of risk or blame | Schopenhauer 30 (universal opinion as authority) |
| 02 | Vague attribution | Cites unnamed sources so the claim cannot be checked or answered. | Accountability avoidance, Fear of risk or blame | Schopenhauer 30 (unnamed authority); Aristotle, ignoratio elenchi |
| 03 | Appeal to rank | Substitutes a senior person's preference for a reason. | Status and face, Accountability avoidance | Schopenhauer 30 (authority in place of reasons) |
| 04 | Credential challenge | Questions the speaker's standing instead of the argument. | Status and face, Territory | Schopenhauer 38 (argumentum ad personam) and 16 |
| 05 | Moving the goalposts | Raises the standard of proof once the original condition appears to have been met. | Sunk cost, Fear of risk or blame | Modern name; Aristotle, ignoratio elenchi (proving a different point) |
| 06 | Burden shifting | Demands that you disprove their claim, or prove a negative, before it can be discussed. | Fear of risk or blame, Accountability avoidance | Modern name; related to Aristotle's begging the question and Schopenhauer 22 |
| 07 | Urgency shield | Uses time pressure to close down discussion or questions. | Overload, Fear of risk or blame | Modern workplace pattern; Schopenhauer 18 (cutting the dispute short) |
| 08 | False dilemma | Presents two options as the only ones, so rejecting one means accepting the other. | Loss of control, Overload | Schopenhauer 13 (offering a choice between two) |
| 09 | Straw man | Restates your position in an exaggerated or simplified form, then argues against that. | Fear of risk or blame, Knowledge gap | Schopenhauer 1 (extension); Aristotle, ignoratio elenchi |
| 10 | Shifting definitions | Uses a key term in two senses, so the argument slides between them. | Knowledge gap, Territory | Aristotle, homonymy (equivocation); Schopenhauer 2 |
| 11 | Overgeneralisation | Draws a general rule from one case, or uses one exception to dismiss a whole approach. | Fear of risk or blame, Sunk cost | Aristotle, secundum quid; Schopenhauer 3 and 25 |
| 12 | Begging the question | Assumes the conclusion in the premise, so the argument goes in a circle. | Knowledge gap, Sunk cost | Aristotle, petitio principii; Schopenhauer 6 |
| 13 | Loaded question | A question that presupposes something you have not accepted. | Status and face, Accountability avoidance | Aristotle, fallacy of many questions; Schopenhauer 7 |
| 14 | Odious labelling | Dismisses a proposal by attaching a negative label rather than engaging with it. | Territory, Status and face | Schopenhauer 32 (odious category) and 12 (loaded terms) |
| 15 | Theory versus practice | Concedes the reasoning but rejects it as impractical without saying why. | Knowledge gap, Fear of risk or blame | Schopenhauer 33 (all very well in theory, but…) |
| 16 | Tone or character attack | Shifts from the argument to your tone, motives or character. | Status and face, Overload | Schopenhauer 38 (becoming personal) |
| 17 | Diversion | Changes the subject when the original point becomes uncomfortable. | Accountability avoidance, Fear of risk or blame | Schopenhauer 18 and 29 (diversion) |
| 18 | Feigned incomprehension | Claims not to understand, implying the idea is unclear or absurd. | Territory, Status and face | Schopenhauer 31 (ironic declaration of incompetence) |
| 19 | Appeal to self-interest | Argues from what is good for you rather than what is true or right. | Fear of risk or blame, Status and face | Schopenhauer 35 (will over insight) |
| 20 | Jargon fog | Uses dense jargon or sheer volume of words to imply an argument that is never actually made. | Knowledge gap, Status and face | Schopenhauer 36 (bewildering with bombast); Aristotle, 'babbling' |
| 21 | Premature closure | Declares the matter settled when it hasn't been agreed. | Loss of control, Overload | Schopenhauer 14 (claiming victory regardless) |
| 22 | Consistency trap | Points to something you said or did before to discredit the current point, rather than address it. | Territory, Status and face | Schopenhauer 16 (argumentum ex concessis); tu quoque |
| 23 | Playing to the audience | Addresses onlookers, such as copied seniors or a meeting, rather than the person making the argument. | Status and face, Fear of risk or blame | Schopenhauer 28 (argumentum ad auditores) |
| 24 | Slippery slope | Predicts a chain of escalating consequences without showing that each step is likely. | Fear of risk or blame, Loss of control | Aristotle, fallacy of the consequent; Schopenhauer 24 (false consequences) |
| 25 | Sunk cost and habit | Defends a course because of past investment or habit, not its present merits. | Sunk cost, Loss of control | Modern name; related to Schopenhauer 30 (custom as authority) |
| 26 | Provocation | Needles or mocks to provoke an emotional reaction that weakens your position. | Status and face, Overload | Schopenhauer 8 (making the opponent angry) and 27 |
| 27 | Refuting the weakest point | Attacks the weakest supporting point and treats the whole position as refuted. | Fear of risk or blame, Territory | Schopenhauer 37 (a bad proof refutes the thesis) |
| 28 | Process gatekeeping | Blocks a proposal on remit or procedure without engaging with its substance. | Territory, Loss of control | Modern workplace pattern; Aristotle, ignoratio elenchi (answering a different question) |

Stratagem numbers refer to the 38 stratagems in Schopenhauer's *The Art of Controversy* (Saunders translation). Entries marked *Modern* are present-day workplace patterns with the same structure as the classical ones.

## Sources

Both source texts are in the public domain:

- Aristotle, *On Sophistical Refutations*, translated by W. A. Pickard-Cambridge. [Internet Classics Archive (MIT)](https://classics.mit.edu/Aristotle/sophist_refut.html)
- Arthur Schopenhauer, *The Art of Controversy* (*Eristische Dialektik*, also known as *The Art of Being Right*), translated by T. Bailey Saunders, 1896. [Project Gutenberg eBook #10731](https://www.gutenberg.org/ebooks/10731)

The field guide is an original adaptation for workplace communication, not a reproduction of either text.

## Files

| File | Purpose |
|---|---|
| `index.html` | The complete app: one self-contained file with no build step and no dependencies beyond Google Fonts |
| `obfuscation-eschewer-build-prompt.md` | A self-contained prompt that rebuilds the app in Claude or another AI assistant, carrying the same field guide, patterns and reply rules |
| `README.md` | This file |

### Running it locally

Download `index.html` and open it in any modern browser. The quick scan and field guide work offline.

### Rebuilding it with an AI assistant

Give `obfuscation-eschewer-build-prompt.md` to Claude (or a similar assistant), pasted or attached as a file. The prompt tells the assistant to use the embedded field guide word for word, so every rebuild works from the same content even when the layout differs.

## Contributing

Suggestions for new tactics, better patterns or clearer response moves are welcome via Issues. Please include an anonymised example message showing the pattern. To keep this app and the build prompt in step, a change to the field guide should be made in both.

## Disclaimer

This is a thinking aid, not a judge of character. Pattern detection is imperfect, and the same words can be bad faith in one context and a fair point in another. Read every finding in context, and when in doubt, ask a genuine question.

## Licence

*Choose a licence when creating the repository (MIT is the common choice for code) and record it here.*
