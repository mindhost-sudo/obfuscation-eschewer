# Build prompt: The Obfuscation Eschewer

Paste everything below the line into Claude (or another capable AI assistant) to build the app. The data blocks are the single source of truth: every copy built from this prompt carries the same field guide, scan patterns, reply wording and analysis rules.

---

Build me a single-page web app called **The Obfuscation Eschewer** (tagline: *Dialectic over eristic: the Socratic tradition, operationalised*). It analyses workplace messages for bad-faith argument tactics and suggests calm, constructive replies.

## Non-negotiable: use the supplied content verbatim

Everything the app says about tactics, root causes, tones and reply wording must come from the data blocks in this prompt. **Do not add, remove, rename, reorder, merge or reword** any tactic, root cause, pattern, template or rule. Do not supplement it from your own knowledge of Aristotle or Schopenhauer. If something seems wrong, build it as written and mention it in your reply to me afterwards. Use British English throughout the interface.

## Sources (for reference and attribution only)

The field guide was derived from these public-domain texts. Show them in a "Sources" note at the foot of the Field guide view, as links:

- Aristotle, *On Sophistical Refutations*, tr. W. A. Pickard-Cambridge. Internet Classics Archive (MIT): https://classics.mit.edu/Aristotle/sophist_refut.html
- Arthur Schopenhauer, *The Art of Controversy* (*Eristische Dialektik*, also published as *The Art of Being Right*), tr. T. Bailey Saunders, 1896. Project Gutenberg eBook #10731: https://www.gutenberg.org/ebooks/10731

Stratagem numbers in the `lineage` fields (for example "Schopenhauer 30") follow the numbering I to XXXVIII in that translation. Entries marked "Modern" are workplace patterns with the same structure as the classical ones.

## How it works

Two views, switched by tabs in a dark masthead: **Analyse** and **Field guide (28)**.

### Analyse view

Left pane:
1. A large text area for the message (opens pre-filled with EXAMPLE_MESSAGE so the app is never empty).
2. An optional one-line context field (placeholder: "e.g. Peer in delivery; I wrote the proposal; client deadline is real").
3. A three-way tone selector from TONES (Diplomatic is the default).
4. Buttons: **Analyse with Claude**, **Load example** and **Clear**. Ctrl/Cmd+Enter also runs the analysis.
5. A privacy note: "The quick scan runs in your browser as you type and sends nothing anywhere. Analyse with Claude sends the message and context to Claude for a fuller reading. Remove names or client details you would rather not share."

Right pane, top to bottom, all brief and scannable:
1. **Summary**: a source chip ("Quick scan · keyword patterns" or "Claude analysis"), a temperature chip (low = green, medium = amber, high = red), a tactic count with how many are "likely", and a one-sentence overall reading.
2. **Flagged phrases**: the message with each triggering phrase highlighted and numbered.
3. **Tactics**: a numbered list. Each item shows the tactic name (clicking it opens its card in the Field guide), a likely/possible chip, the quoted phrase, a one-sentence explanation and "Response move:" followed by the tactic's mitigation.
4. **Likely root cause**: the primary root cause with its confidence, any secondary cause, a one-line rationale and "What helps:" from ROOTS.
5. **Good faith**: the charitable reading and the legitimate concern (Claude analysis only).
6. **Suggested reply**: the reply text with a Copy button. If the tone changes after a Claude analysis, show "Redraft as [tone]".
7. **Uncertainty**: a bullet list, shown only when there are items.

### Quick scan (always available, runs locally as the user types)

- For each tactic, run every string in `patterns` as a JavaScript RegExp with flags `gi`. The tactic is flagged when the number of matches is at least `min_hits` (default 1). Its confidence is "likely" if matches exceed that threshold, otherwise "possible".
- The quote is the sentence that contains the first match. The explanation is the tactic's `flag`.
- Root cause: for each flagged tactic, add 2 points to its first root cause and 1 point to its second. The highest total is primary and the next is secondary, both at "low" confidence, with the rationale "Tallied from the root causes typical of the matched tactics."
- Temperature: "high" if 4 or more tactics or any of `adhominem`/`provocation`; "medium" if 2 or 3; otherwise "low".
- Summary sentence when nothing matches: "No keyword patterns matched. The message may simply be direct; Claude analysis can check for subtler tactics." Otherwise: "N possible tactic(s) matched by keyword. Treat these as prompts to read more closely, not conclusions."
- Reply: REPLY_TEMPLATES.open[tone], then the `reply_line` of up to three flagged tactics (prefer "likely", keep them in message order), each as its own paragraph, then REPLY_TEMPLATES.close[tone]. If nothing matched, use open + "Could you say a little more about what you'd need to see to be comfortable with this?" + close.

### Claude analysis (only if this environment lets the page call Claude)

If the page can call Claude, send ANALYSIS_PROMPT with every {{placeholder}} filled from the data below, and expect the JSON it specifies. Keep only tactics whose `id` exists in FIELD_GUIDE_DATA. Highlight a quote only if it appears in the message (try exact match first, then case-insensitive). For "Redraft as [tone]", ask Claude to rewrite the existing reply in the new tone under the same reply rules and return only the reply text.

If the page **cannot** call Claude, hide the Analyse button, show "Claude analysis isn't available in this view. The quick scan still works.", and build everything else exactly as described.

### Field guide view

- A short introduction: "Aristotle's *Sophistical Refutations* catalogued arguments that only appear to refute. Schopenhauer's *The Art of Being Right* listed 38 stratagems for winning a dispute regardless of truth. This guide translates both into the patterns that show up in workplace email and chat, plus a few modern ones with the same structure." Below it: "Each tactic notes its likely root cause and a response move. The moves aim to bring the argument back to evidence; none of them are counter-tactics. A tactic in a message is a hypothesis about the message, never a verdict on the person."
- Root-cause filter cards (one per ROOTS entry, click to toggle) and a search box covering name, definition, lineage, phrasing and mitigation.
- A card grid. Each card shows its number (01 to 28), name, lineage in italics, definition, "Sounds like" phrases, root cause chips and the mitigation.
- The Sources note.

## Guidelines the app must embody

Assume good faith where the text allows; flag uncertainty; never suggest manipulative counter-tactics. The goal is dialectic (finding the truth), not eristic (winning). Keep the output brief and scannable.

## Visual design

Masthead and dark sections use #003134; headers, highlights, accents and primary buttons use #007173 (a lighter teal such as #4cc2c0 on dark backgrounds). Support light and dark themes. Use a serif display face for headings (Spectral or Georgia), a clean sans serif for body text (IBM Plex Sans or the system UI font) and a monospace face for quoted phrases. Use two columns on wide screens and a single column on phones.

## DATA: ROOTS

```json
{
  "fear": {
    "name": "Fear of risk or blame",
    "desc": "Worried about failure, exposure or being held responsible.",
    "helps": "Lower the perceived risk: a pilot, a rollback plan, shared ownership of the decision."
  },
  "territory": {
    "name": "Territory",
    "desc": "Protecting remit, ownership or budget.",
    "helps": "Credit their ownership and involve them in shaping the outcome."
  },
  "sunk": {
    "name": "Sunk cost",
    "desc": "Invested in the current approach, personally or financially.",
    "helps": "Honour what was built; compare options on future cost and value only."
  },
  "knowledge": {
    "name": "Knowledge gap",
    "desc": "Missing context or expertise, and possibly reluctant to say so.",
    "helps": "Explain without condescension; offer a short briefing or walkthrough."
  },
  "status": {
    "name": "Status and face",
    "desc": "Concerned about how they look to peers or seniors.",
    "helps": "Give them a way to agree, or to change their mind, without losing face."
  },
  "overload": {
    "name": "Overload",
    "desc": "Under time or capacity pressure; resistance sheds work.",
    "helps": "Make the ask smaller and the path to a decision faster."
  },
  "accountability": {
    "name": "Accountability avoidance",
    "desc": "Avoiding ownership of a decision or its consequences.",
    "helps": "Make decisions, owners and risk acceptance explicit in writing."
  },
  "control": {
    "name": "Loss of control",
    "desc": "Uncomfortable with change they did not initiate.",
    "helps": "Offer real choices and a role in the decision."
  }
}
```

## DATA: TONES

```json
{
  "diplomatic": {
    "label": "Diplomatic",
    "desc": "warm, collaborative and face-saving; acknowledges feelings briefly"
  },
  "direct": {
    "label": "Direct",
    "desc": "brief, plain and courteous; no padding, short sentences"
  },
  "formal": {
    "label": "Formal",
    "desc": "measured, professional register suitable for senior leaders or clients"
  }
}
```

## DATA: REPLY_TEMPLATES

```json
{
  "open": {
    "diplomatic": "Thanks for taking the time to set this out. I can see it matters, and I want to get it right.",
    "direct": "Thanks for this. A few points so we can settle it quickly.",
    "formal": "Thank you for your message and for setting out your concerns."
  },
  "close": {
    "diplomatic": "Happy to pick this up on a short call if that's easier, so we can settle it on the facts together.",
    "direct": "Once I have that, I can give you a clear answer.",
    "formal": "I would welcome the opportunity to review these points together at your convenience."
  }
}
```

## DATA: FIELD_GUIDE_DATA (28 tactics)

`patterns` are JavaScript RegExp source strings (JSON-escaped; use them with flags `gi`).

```json
[
  {
    "id": "consensus",
    "name": "Appeal to consensus",
    "lineage": "Schopenhauer 30 (universal opinion as authority)",
    "definition": "Claims a group already agrees, so the point needs no further argument.",
    "sounds_like": [
      "Everyone agrees that…",
      "The team's view is…",
      "Nobody thinks this will work."
    ],
    "root_causes": [
      "status",
      "fear"
    ],
    "flag": "Presents agreement as settled without saying who agrees or why.",
    "mitigation": "Ask who was consulted and what their specific objections were, then treat each as an argument to answer rather than a vote to count.",
    "reply_line": "Could you let me know who you've spoken to and what their main reservations were? I'd rather address those points directly.",
    "patterns": [
      "\\beveryone (agrees|thinks|knows|feels|is aligned|is of the view)",
      "\\bnobody (thinks|believes|wants|agrees|likes)",
      "\\b(the )?(whole |entire )?team (agrees|feels|thinks|is aligned)",
      "\\bgeneral consensus\\b",
      "\\bwe('re| are) all (agreed|aligned)\\b",
      "\\ball of us (agree|think|feel)"
    ]
  },
  {
    "id": "vague",
    "name": "Vague attribution",
    "lineage": "Schopenhauer 30 (unnamed authority); Aristotle, ignoratio elenchi",
    "definition": "Cites unnamed sources so the claim cannot be checked or answered.",
    "sounds_like": [
      "People are saying…",
      "Concerns have been raised…",
      "I've heard from a few people…"
    ],
    "root_causes": [
      "accountability",
      "fear"
    ],
    "flag": "Relies on unnamed sources, so the claim can't be tested or answered.",
    "mitigation": "Ask for the substance rather than the names: what exactly was the concern, and in what context? Offer to speak to the source directly.",
    "reply_line": "Could you share what the specific concerns were? I'm happy to speak to whoever raised them so nothing gets lost in translation.",
    "patterns": [
      "\\b(some |a few |several |many )?people (are saying|have said|have mentioned|have raised|feel that|think that)",
      "\\b(concerns|questions|issues|doubts) have been (raised|expressed|flagged)",
      "\\b(i've|i have|we've|we have) (heard|spoken|talked)( to| from| with)? (a few|some|several|a couple of|various) (people|colleagues|folks|stakeholders)",
      "\\bit('s| has been) (said|mentioned|suggested|noted|felt) that\\b",
      "\\bthere('s| is) a (feeling|sense|perception|view) that\\b",
      "\\bword is\\b"
    ]
  },
  {
    "id": "authority",
    "name": "Appeal to rank",
    "lineage": "Schopenhauer 30 (authority in place of reasons)",
    "definition": "Substitutes a senior person's preference for a reason.",
    "sounds_like": [
      "The VP wants it this way.",
      "Leadership has already decided.",
      "Our CISO would never accept that."
    ],
    "root_causes": [
      "status",
      "accountability"
    ],
    "flag": "Uses a senior person's preference in place of a reason.",
    "mitigation": "Respect the authority's role, then ask for the rationale or requirement behind the position so you can meet it.",
    "reply_line": "Understood that leadership has a view here. Could you share the reasoning or requirement behind it, so I can make sure the proposal meets it?",
    "patterns": [
      "\\b(the |our )?(vp|svp|evp|ceo|cto|ciso|cio|coo|director|board|leadership|management|execs?|exec team|senior management) (wants|has decided|decided|expects|insists|won't|will not|would never|has been clear|has made it clear|is adamant)",
      "\\b(leadership|management|the board|the execs?) (has|have) (already )?(decided|agreed|signed off)",
      "\\b(above|beyond) my pay ?grade\\b",
      "\\bcame (down )?from the top\\b"
    ]
  },
  {
    "id": "credential",
    "name": "Credential challenge",
    "lineage": "Schopenhauer 38 (argumentum ad personam) and 16",
    "definition": "Questions the speaker's standing instead of the argument.",
    "sounds_like": [
      "Have you ever actually run a SOC?",
      "With respect, you're not an engineer.",
      "Come back when you've done it."
    ],
    "root_causes": [
      "status",
      "territory"
    ],
    "flag": "Targets your standing rather than the argument itself.",
    "mitigation": "Don't defend your CV. Acknowledge their experience, invite it, and bring the discussion back to evidence anyone could check.",
    "reply_line": "You clearly have hands-on experience here and I'd value it. Which part of the approach do you think won't hold up in practice? That will help me test it properly.",
    "patterns": [
      "\\bhave you (ever )?(actually |personally |even )?(run|managed|worked (in|on|with)|done|built|led|operated|been (in|on))\\b",
      "\\b(you're|you are) not (an? )?(engineer|expert|technical|analyst|architect|practitioner|operator)",
      "\\bcome back (to me )?when you('ve| have)\\b",
      "\\b(what|how much) do you (actually )?(know|understand) about\\b",
      "\\byou wouldn't (know|understand)\\b"
    ]
  },
  {
    "id": "goalposts",
    "name": "Moving the goalposts",
    "lineage": "Modern name; Aristotle, ignoratio elenchi (proving a different point)",
    "definition": "Raises the standard of proof once the original condition appears to have been met.",
    "sounds_like": [
      "That's fine, but now it also needs…",
      "OK, but that's not really the issue.",
      "That was then; now we need…"
    ],
    "root_causes": [
      "sunk",
      "fear"
    ],
    "flag": "Introduces a new condition after the previous one appears to have been met.",
    "mitigation": "Restate the original criterion and confirm whether it has been met before taking on new ones; agree the full set of criteria in writing.",
    "reply_line": "To make sure we're working to the same bar: was the original requirement met? If there are further criteria, could we list them all now so I can address them in one go?",
    "patterns": [
      "\\b(that's|that is) (fine|great|good|all very well),? but (now|it also|we also|it still|there's still)",
      "\\b(ok|okay|fine|sure),? but (that's|that is) not (really )?(the|my) (issue|point|question|concern)",
      "\\b(it|this) (now|also) (needs|has) to\\b",
      "\\bthat was (then|before)\\b",
      "\\b(now|but) (we|you) (also )?need to (show|prove|demonstrate)\\b"
    ]
  },
  {
    "id": "burden",
    "name": "Burden shifting",
    "lineage": "Modern name; related to Aristotle's begging the question and Schopenhauer 22",
    "definition": "Demands that you disprove their claim, or prove a negative, before it can be discussed.",
    "sounds_like": [
      "Prove it won't fail.",
      "Unless you can guarantee…",
      "Show me why we shouldn't…"
    ],
    "root_causes": [
      "fear",
      "accountability"
    ],
    "flag": "Asks you to prove a negative or guarantee an outcome, which no proposal can do.",
    "mitigation": "Acknowledge the risk, decline the impossible standard, and offer a proportionate one: evidence, a pilot, a rollback plan or a recorded risk acceptance.",
    "reply_line": "No approach can come with a guarantee, but I can set out the evidence, the residual risk and a rollback plan. Would that give you what you need to decide?",
    "patterns": [
      "\\bunless you can (guarantee|prove|promise|show|demonstrate)\\b",
      "\\b(prove|guarantee|show me) (that )?(it|this|that|there) (won't|will not|can't|cannot|doesn't|does not|will never|is no)",
      "\\b(can you|you need to|you'll need to|you must) (guarantee|prove)\\b",
      "\\bshow me why (we|i) shouldn't\\b",
      "\\b(it's|it is) (on you|up to you) to (prove|show)\\b",
      "\\buntil you (can )?(prove|show)\\b"
    ]
  },
  {
    "id": "urgency",
    "name": "Urgency shield",
    "lineage": "Modern workplace pattern; Schopenhauer 18 (cutting the dispute short)",
    "definition": "Uses time pressure to close down discussion or questions.",
    "sounds_like": [
      "We don't have time to debate this.",
      "Just get it done.",
      "The client needs an answer by Friday, so…"
    ],
    "root_causes": [
      "overload",
      "fear"
    ],
    "flag": "Uses time pressure to close the discussion rather than to answer it.",
    "mitigation": "Accept the deadline as real and separate it from the decision: offer the fastest route to a sound answer, and name what is at risk if that step is skipped.",
    "reply_line": "I understand the deadline is tight and I can turn this round quickly. The one thing I need to confirm first is the point above, so we don't have to undo it later.",
    "patterns": [
      "\\b(we )?(don't|do not|haven't|have not) (got |have )?(the )?time (to|for)\\b",
      "\\bno time (to|for)\\b",
      "\\bjust (get it done|do it|ship it|send it|push it through|make it work)\\b",
      "\\b(needs?|wants?) (an answer|a decision|this|it) (by|before) (eod|cob|close of play|friday|monday|tuesday|wednesday|thursday|tomorrow|today|end of (the )?(day|week))",
      "\\b(this is|it's|it is) (urgent|time[- ]critical|time[- ]sensitive)\\b",
      "\\bwe (can't|cannot) (afford|wait) to\\b",
      "\\bnow is not the time\\b"
    ]
  },
  {
    "id": "dilemma",
    "name": "False dilemma",
    "lineage": "Schopenhauer 13 (offering a choice between two)",
    "definition": "Presents two options as the only ones, so rejecting one means accepting the other.",
    "sounds_like": [
      "Either we ship as-is or we lose the client.",
      "It's this or nothing.",
      "You're either with us or…"
    ],
    "root_causes": [
      "control",
      "overload"
    ],
    "flag": "Offers two options as if they were the only ones available.",
    "mitigation": "Acknowledge both options, name a third (phased, reduced scope, pilot), and ask what criteria would decide between them.",
    "reply_line": "I can see both of those options. Would a middle route work, for example a phased or reduced-scope version, so we keep the timeline without carrying the full risk?",
    "patterns": [
      "\\beither (we|you|i|they)\\b[^.?!\\n]{0,90}\\bor (we|you|i|they)\\b",
      "\\b(it's|it is) (this|that) or nothing\\b",
      "\\b(the )?only (two )?(options?|choices?|alternatives?) (is|are)\\b",
      "\\btake it or leave it\\b",
      "\\byou('re| are) either (with|for)\\b"
    ]
  },
  {
    "id": "strawman",
    "name": "Straw man",
    "lineage": "Schopenhauer 1 (extension); Aristotle, ignoratio elenchi",
    "definition": "Restates your position in an exaggerated or simplified form, then argues against that.",
    "sounds_like": [
      "So you're saying we should just ignore…",
      "You want to throw out the whole process?",
      "What you're proposing is basically…"
    ],
    "root_causes": [
      "fear",
      "knowledge"
    ],
    "flag": "Restates your position in a stronger or simpler form than you put it.",
    "mitigation": "Calmly restate your actual position in one sentence, then check you've understood their concern in theirs.",
    "reply_line": "To be clear, my proposal is narrower than that: [restate it in one sentence]. Have I understood your main concern correctly?",
    "patterns": [
      "\\bso (what )?you('re| are) (saying|suggesting|proposing)\\b",
      "\\byou (want|are proposing|'re proposing) (to )?(just )?(throw out|scrap|abandon|ignore|get rid of|tear up|bin)\\b",
      "\\bwhat you('re| are) (proposing|suggesting|saying) is (basically|essentially|really|just)\\b"
    ]
  },
  {
    "id": "equivocation",
    "name": "Shifting definitions",
    "lineage": "Aristotle, homonymy (equivocation); Schopenhauer 2",
    "definition": "Uses a key term in two senses, so the argument slides between them.",
    "sounds_like": [
      "That's not real 'coverage'.",
      "Depends what you mean by 'done'.",
      "When I say secure, I mean…"
    ],
    "root_causes": [
      "knowledge",
      "territory"
    ],
    "flag": "A key term may be carrying two different meanings in the same argument.",
    "mitigation": "Agree a working definition of the key term before continuing, and write it down.",
    "reply_line": "I think we may be using '[term]' in slightly different ways. Could we agree a definition first, so we're comparing like with like?",
    "patterns": [
      "\\b(it )?depends (on )?what you mean by\\b",
      "\\b(that's|that is|it's|it is) not (real|true|proper|actual|genuine) \\w+",
      "\\bwhen i say\\b",
      "\\b(that's|that is) not what (i|we) (mean|meant) by\\b"
    ]
  },
  {
    "id": "hasty",
    "name": "Overgeneralisation",
    "lineage": "Aristotle, secundum quid; Schopenhauer 3 and 25",
    "definition": "Draws a general rule from one case, or uses one exception to dismiss a whole approach.",
    "sounds_like": [
      "We tried that once and it failed.",
      "That never works.",
      "Look what happened at the last client."
    ],
    "root_causes": [
      "fear",
      "sunk"
    ],
    "flag": "Generalises from a single case or anecdote.",
    "mitigation": "Take the example seriously, ask what specifically went wrong, and show what is different this time.",
    "reply_line": "That example is useful. Do you know what caused it to fail? If it was [factor], I can show how this approach handles it differently.",
    "patterns": [
      "\\b(we|they) tried (that|this|it) (once|before|already|last year)\\b",
      "\\b(that|this|it) (never|always) (works|fails|goes wrong|ends well)\\b",
      "\\blook (at )?what happened (at|with|to|last)\\b",
      "\\b(every time|whenever) (we|they) (try|tried|do|did) (this|that)\\b",
      "\\b(this|that|it) has (never|always) (worked|failed)\\b"
    ]
  },
  {
    "id": "begging",
    "name": "Begging the question",
    "lineage": "Aristotle, petitio principii; Schopenhauer 6",
    "definition": "Assumes the conclusion in the premise, so the argument goes in a circle.",
    "sounds_like": [
      "We need X because X is required.",
      "It's right because it's best practice.",
      "Obviously this is the correct way."
    ],
    "root_causes": [
      "knowledge",
      "sunk"
    ],
    "flag": "The conclusion is assumed in the reason given for it.",
    "mitigation": "Ask what independent evidence or requirement supports the conclusion.",
    "reply_line": "Could you point me to the requirement or evidence behind that? It would help me see the reasoning as well as the conclusion.",
    "patterns": [
      "\\b(because|since) (it's|it is) (best practice|the standard|the right way|how it's done|required)\\b",
      "\\b(obviously|clearly|self-evidently),? (this|that|it|we) (is|are|should|must)\\b",
      "\\b(it's|it is) (obvious|self-evident|common sense) that\\b"
    ]
  },
  {
    "id": "loaded",
    "name": "Loaded question",
    "lineage": "Aristotle, fallacy of many questions; Schopenhauer 7",
    "definition": "A question that presupposes something you have not accepted.",
    "sounds_like": [
      "Why do you keep missing these deadlines?",
      "When did you decide to ignore the SLA?",
      "Why is your team always the bottleneck?"
    ],
    "root_causes": [
      "status",
      "accountability"
    ],
    "flag": "The question takes for granted something you haven't agreed to.",
    "mitigation": "Separate the presupposition from the question: address the assumption factually, then answer the real question.",
    "reply_line": "Before I answer, I want to check one assumption: [restate it]. On the underlying question, here is where things stand: [facts].",
    "patterns": [
      "\\bwhy (do|does) (you|your team|they) (keep|always|never|continually|constantly)\\b",
      "\\bwhy (is|are) (you|your team|your side) (always|never|constantly)\\b",
      "\\bwhen did you (decide|stop|start) to\\b",
      "\\bhow come (you|your team) (always|never|keep)\\b"
    ]
  },
  {
    "id": "label",
    "name": "Odious labelling",
    "lineage": "Schopenhauer 32 (odious category) and 12 (loaded terms)",
    "definition": "Dismisses a proposal by attaching a negative label rather than engaging with it.",
    "sounds_like": [
      "That's just vendor marketing.",
      "Classic ivory-tower thinking.",
      "Sounds like more bureaucracy."
    ],
    "root_causes": [
      "territory",
      "status"
    ],
    "flag": "Dismisses the idea by labelling it rather than engaging with it.",
    "mitigation": "Acknowledge the worry behind the label, then ask which specific element triggers it.",
    "reply_line": "I'd hate for this to add overhead or hype. Which part specifically gives that impression? I can either justify it or cut it.",
    "patterns": [
      "\\b(that's|that is|this is|it's|it is|sounds like|smells like|reads like) (just |classic |typical |pure |more |basically |a )?(vendor|marketing|sales|ivory[- ]tower|academic|consultant|bureaucra\\w*|box[- ]ticking|tick[- ]box|theatre|theater|buzzword|hype|red tape|paper exercise|corporate)",
      "\\b(vendor|consultant|marketing|corporate|management)[- ]?speak\\b"
    ]
  },
  {
    "id": "theory",
    "name": "Theory versus practice",
    "lineage": "Schopenhauer 33 (all very well in theory, but…)",
    "definition": "Concedes the reasoning but rejects it as impractical without saying why.",
    "sounds_like": [
      "Fine in theory, but in the real world…",
      "That's not how it works on the ground.",
      "Looks good on paper."
    ],
    "root_causes": [
      "knowledge",
      "fear"
    ],
    "flag": "Accepts the logic in principle but rejects it in practice without saying why.",
    "mitigation": "Agree that practice matters and ask for the specific practical obstacle; that is usually the most valuable information in the exchange.",
    "reply_line": "I agree practice is what counts. What's the main obstacle you'd expect on the ground? If you can name it, I'll build it into the plan.",
    "patterns": [
      "\\b(fine|great|good|works|all very well|nice|sounds good|looks good) (in theory|on paper)\\b",
      "\\bin the real world\\b",
      "\\b(not|isn't|is not) how (it|things) (works?|happens?|goes|go) (on the ground|in practice|in reality|here|in real life)\\b",
      "\\bivory tower\\b"
    ]
  },
  {
    "id": "adhominem",
    "name": "Tone or character attack",
    "lineage": "Schopenhauer 38 (becoming personal)",
    "definition": "Shifts from the argument to your tone, motives or character.",
    "sounds_like": [
      "You're being emotional about this.",
      "This is typical of you.",
      "You just want to protect your patch."
    ],
    "root_causes": [
      "status",
      "overload"
    ],
    "flag": "Moves from the substance to your tone, motives or character.",
    "mitigation": "Don't match the register. Acknowledge any fair point about tone in a single line, then return to the substance.",
    "reply_line": "If my earlier message came across as sharp, that wasn't the intent. On the substance, the question I'd still like to resolve is [question].",
    "patterns": [
      "\\byou('re| are) (being )?(so )?(emotional|defensive|difficult|naive|dramatic|precious|obstructive|negative|aggressive|paranoid|unreasonable|over-?reacting)\\b",
      "\\b(typical|classic) (of )?you\\b",
      "\\byou (just |only )?(want|care about|always want) (to )?(protect|cover|look good|win|be right|your)\\b",
      "\\byour (attitude|tone|behaviour|behavior) (is|has been)\\b",
      "\\b(that's|that is) (a bit )?rich coming from\\b"
    ]
  },
  {
    "id": "diversion",
    "name": "Diversion",
    "lineage": "Schopenhauer 18 and 29 (diversion)",
    "definition": "Changes the subject when the original point becomes uncomfortable.",
    "sounds_like": [
      "The real issue is…",
      "Before we get into that, what about…",
      "Let's not forget last quarter."
    ],
    "root_causes": [
      "accountability",
      "fear"
    ],
    "flag": "Changes the subject away from the point under discussion.",
    "mitigation": "Acknowledge the new topic as worth discussing, park it explicitly, and return to the open question.",
    "reply_line": "That's a fair topic and I'm happy to take it separately. To close this one first: [restate the open question].",
    "patterns": [
      "\\bthe (real|bigger|actual|more important) (issue|problem|question|point) (is|here)\\b",
      "\\b(before we get into|let's not forget|let's not lose sight)\\b",
      "\\b(more importantly|that aside|speaking of which),",
      "\\blet's (focus|talk) (on|about) (something else|the real)\\b"
    ]
  },
  {
    "id": "incomprehension",
    "name": "Feigned incomprehension",
    "lineage": "Schopenhauer 31 (ironic declaration of incompetence)",
    "definition": "Claims not to understand, implying the idea is unclear or absurd.",
    "sounds_like": [
      "I genuinely don't see why we'd ever…",
      "Maybe I'm missing something, but…",
      "I'm struggling to follow the logic."
    ],
    "root_causes": [
      "territory",
      "status"
    ],
    "flag": "Professes not to understand; this may be genuine, or may imply the idea is absurd.",
    "mitigation": "Take it at face value: give a shorter, clearer explanation and ask which part is unclear. This is where assuming good faith matters most.",
    "reply_line": "Fair challenge; let me put it more simply: [one-sentence version]. Which part is least clear, so I can fill the gap?",
    "patterns": [
      "\\b(maybe|perhaps) i('m| am) missing something\\b",
      "\\bi('m| am) (struggling|failing) to (see|follow|understand)\\b",
      "\\bi (genuinely |honestly |just |really |simply )?(don't|do not|can't|cannot) (see|understand|follow) (why|how|the logic)\\b",
      "\\bhelp me understand (why|how) (anyone|we|you) would\\b"
    ]
  },
  {
    "id": "interest",
    "name": "Appeal to self-interest",
    "lineage": "Schopenhauer 35 (will over insight)",
    "definition": "Argues from what is good for you rather than what is true or right.",
    "sounds_like": [
      "Think about how this will look for you.",
      "This won't help your review.",
      "Do you really want to be the one who…?"
    ],
    "root_causes": [
      "fear",
      "status"
    ],
    "flag": "Argues from consequences for you rather than from the merits.",
    "mitigation": "Thank them for the heads-up, then bring it back to the merits for the organisation and the client.",
    "reply_line": "I appreciate you looking out for me. I'd still like to decide this on what's right for the client, so the question I'd ask is [question].",
    "patterns": [
      "\\b(think about|consider) (how|what) (this|it) (will|would|could) (look|mean) (for|to) you\\b",
      "\\bdo you (really )?want to be the one\\b",
      "\\b(for your own sake|if i were you)\\b",
      "\\byour (career|review|bonus|reputation|promotion)\\b"
    ]
  },
  {
    "id": "bombast",
    "name": "Jargon fog",
    "lineage": "Schopenhauer 36 (bewildering with bombast); Aristotle, 'babbling'",
    "definition": "Uses dense jargon or sheer volume of words to imply an argument that is never actually made.",
    "sounds_like": [
      "Given the synergistic risk posture across the holistic control plane…",
      "Long chains of acronyms without a conclusion."
    ],
    "root_causes": [
      "knowledge",
      "status"
    ],
    "flag": "Dense jargon may be standing in for a specific point.",
    "mitigation": "Summarise what you think the point is in plain words and ask them to confirm or correct it.",
    "reply_line": "Let me check I've understood the core point: [plain-English summary]. Is that right, or have I missed something?",
    "patterns": [
      "\\b(synerg\\w*|holistic\\w*|paradigm\\w*|leverag\\w+|best[- ]of[- ]breed|world[- ]class|next[- ]gen\\w*|value[- ]add\\w*|strategic alignment|move the needle|boil the ocean|circle back|north star|low[- ]hanging fruit|single pane of glass)\\b"
    ],
    "min_hits": 2
  },
  {
    "id": "victory",
    "name": "Premature closure",
    "lineage": "Schopenhauer 14 (claiming victory regardless)",
    "definition": "Declares the matter settled when it hasn't been agreed.",
    "sounds_like": [
      "So we're agreed then.",
      "As we established…",
      "I'll take that as a yes."
    ],
    "root_causes": [
      "control",
      "overload"
    ],
    "flag": "Treats a point as agreed when it may not have been.",
    "mitigation": "Politely correct the record in writing, stating what is and isn't agreed.",
    "reply_line": "Just to keep the record accurate: we agreed [X], but [Y] is still open. Can we confirm that before moving on?",
    "patterns": [
      "\\b(so )?(we're|we are) (all )?(agreed|aligned|settled|on the same page)( then)?\\b",
      "\\bas (we|i) (established|agreed|concluded)\\b",
      "\\b(i'll|i will) take (that|this|your silence) as a yes\\b",
      "\\b(that's|that is) (settled|decided|closed|final) then\\b",
      "\\bunless i hear otherwise\\b"
    ]
  },
  {
    "id": "inconsistency",
    "name": "Consistency trap",
    "lineage": "Schopenhauer 16 (argumentum ex concessis); tu quoque",
    "definition": "Points to something you said or did before to discredit the current point, rather than address it.",
    "sounds_like": [
      "But you said last month…",
      "Funny, you didn't object when…",
      "You do the same thing yourself."
    ],
    "root_causes": [
      "territory",
      "status"
    ],
    "flag": "Uses an earlier position of yours against the current one; this is fair if nothing has changed.",
    "mitigation": "Acknowledge the earlier position honestly, explain what changed (or concede), then return to the merits.",
    "reply_line": "Fair point, I did say that. What changed is [new information]. On the merits as they stand now, [point].",
    "patterns": [
      "\\b(but |yet )?you (said|told us|agreed|argued|wrote) (last|in|at|on|the other|previously|before|earlier)\\b",
      "\\byou (didn't|did not) (object|complain|mind|say anything) when\\b",
      "\\byou (do|did) the (same|exact same) (thing )?\\b",
      "\\b(that's|that is) not what you said\\b"
    ]
  },
  {
    "id": "audience",
    "name": "Playing to the audience",
    "lineage": "Schopenhauer 28 (argumentum ad auditores)",
    "definition": "Addresses onlookers, such as copied seniors or a meeting, rather than the person making the argument.",
    "sounds_like": [
      "As I'm sure everyone on this thread can see…",
      "Copying in the director for visibility.",
      "I think the room will agree…"
    ],
    "root_causes": [
      "status",
      "fear"
    ],
    "flag": "Addresses onlookers rather than you, which raises the stakes for both sides.",
    "mitigation": "Keep your reply calm and factual for everyone reading, and offer to take the detail offline with the sender.",
    "reply_line": "For everyone's visibility, a short summary of the facts: [one line]. Happy to go through the detail with you directly.",
    "patterns": [
      "\\b(copying|cc'?ing|looping|adding) (in )?(\\w+ ){0,3}(for (visibility|awareness|transparency)|so (they|he|she|everyone))",
      "\\b(i'm sure )?(everyone|all of you|the (room|group|team)) (on|in) (this|the) (thread|email|chain|call|room|meeting) (can|will) (see|agree)\\b",
      "\\bfor (everyone's|the group's|the wider team's) (benefit|awareness|visibility)\\b"
    ]
  },
  {
    "id": "slope",
    "name": "Slippery slope",
    "lineage": "Aristotle, fallacy of the consequent; Schopenhauer 24 (false consequences)",
    "definition": "Predicts a chain of escalating consequences without showing that each step is likely.",
    "sounds_like": [
      "If we allow this, every client will want it.",
      "This opens the floodgates.",
      "Where does it end?"
    ],
    "root_causes": [
      "fear",
      "control"
    ],
    "flag": "Predicts escalating consequences without showing each step is likely.",
    "mitigation": "Acknowledge the first step's risk and propose a boundary or control that stops the chain.",
    "reply_line": "I take the point about precedent. Would it help to define the limits up front, for example [boundary], so this stays a one-off?",
    "patterns": [
      "\\bopens? (up )?the (floodgates|door to)\\b",
      "\\bslippery slope\\b",
      "\\bwhere (does|will) (it|this) (end|stop)\\b",
      "\\b(next|before long|soon) (everyone|every (client|team|customer)) will (want|ask|expect)\\b",
      "\\b(set|sets|setting) a (dangerous |bad )?precedent\\b",
      "\\bcan of worms\\b"
    ]
  },
  {
    "id": "sunkcost",
    "name": "Sunk cost and habit",
    "lineage": "Modern name; related to Schopenhauer 30 (custom as authority)",
    "definition": "Defends a course because of past investment or habit, not its present merits.",
    "sounds_like": [
      "We've always done it this way.",
      "We've invested too much to change now.",
      "That's how it's done here."
    ],
    "root_causes": [
      "sunk",
      "control"
    ],
    "flag": "Defends the current approach by past investment or habit rather than present merit.",
    "mitigation": "Recognise the investment and what it built, then compare options on future cost and risk only.",
    "reply_line": "The existing investment has real value and I'd want to keep what works. Could we compare the options on what each costs and risks from here?",
    "patterns": [
      "\\b(we've|we have|it's|it has) always (done it|been done|worked|been)( this| that)?( way)?\\b",
      "\\b(we've|we have) (already )?(invested|spent|sunk|put) (too much|so much|a lot|years|months)\\b",
      "\\bhow (things are|we do (it|things)) (done )?(around )?here\\b",
      "\\bif it (ain't|isn't|is not) broke\\b",
      "\\btoo late to (change|turn back)\\b"
    ]
  },
  {
    "id": "provocation",
    "name": "Provocation",
    "lineage": "Schopenhauer 8 (making the opponent angry) and 27",
    "definition": "Needles or mocks to provoke an emotional reaction that weakens your position.",
    "sounds_like": [
      "Interesting idea, if a bit naive.",
      "Surely you can't be serious.",
      "Good luck with that."
    ],
    "root_causes": [
      "status",
      "overload"
    ],
    "flag": "Sarcasm or needling invites an emotional response; staying level is the counter.",
    "mitigation": "Don't take the bait. Reply only to the substantive content, in a slightly warmer register than the original.",
    "reply_line": "Thanks for the challenge. Setting style aside, the substantive point I'd like to resolve is [point].",
    "patterns": [
      "\\bsurely you (can't|cannot|don't|do not) (be serious|mean|think)\\b",
      "\\b(that's|that is) (frankly )?(naive|absurd|ridiculous|nonsense|laughable)\\b",
      "\\b(good luck with that|nice try|bless)\\b",
      "\\b(are you|you can't be) serious\\b"
    ]
  },
  {
    "id": "badproof",
    "name": "Refuting the weakest point",
    "lineage": "Schopenhauer 37 (a bad proof refutes the thesis)",
    "definition": "Attacks the weakest supporting point and treats the whole position as refuted.",
    "sounds_like": [
      "Your figure on slide 3 is wrong, so the whole case falls apart.",
      "If that number's off, why trust the rest?"
    ],
    "root_causes": [
      "fear",
      "territory"
    ],
    "flag": "Treats one flawed detail as if it disproves the whole position.",
    "mitigation": "Concede the error cleanly and fix it, then show which conclusions stand without it.",
    "reply_line": "Thanks for catching that; I'll correct it. The conclusion doesn't depend on that point, though: [the reasons that still stand].",
    "patterns": [
      "\\b(the )?(whole|entire|rest of the|rest of your) (case|proposal|argument|thing|document|analysis|plan) (falls apart|is (wrong|flawed|invalid|worthless|suspect))",
      "\\b(why|how) (would|should|can) (i|we) trust (the rest|anything)\\b",
      "\\b(undermines|invalidates) (the )?(whole|entire|rest)\\b"
    ]
  },
  {
    "id": "gatekeeping",
    "name": "Process gatekeeping",
    "lineage": "Modern workplace pattern; Aristotle, ignoratio elenchi (answering a different question)",
    "definition": "Blocks a proposal on remit or procedure without engaging with its substance.",
    "sounds_like": [
      "This isn't your call.",
      "That needs to go through the board first.",
      "Not in scope for your team."
    ],
    "root_causes": [
      "territory",
      "control"
    ],
    "flag": "Raises remit or process instead of the substance; the process point may still be valid.",
    "mitigation": "Respect the process and ask what the route is; separately ask whether, on substance, they support the idea.",
    "reply_line": "Happy to take this through the right route. Which forum or approver should it go to, and on the substance, would you support it there?",
    "patterns": [
      "\\b(this|that|it) (isn't|is not) (your|their) (call|decision|place|remit|area|job)\\b",
      "\\b(not in|outside( of)?) (your|the|our) (remit|scope|lane|mandate)\\b",
      "\\b(needs|has) to go (through|via) (the )?(cab|board|committee|governance|procurement|legal|change board|proper process)\\b",
      "\\bstay in your lane\\b",
      "\\bwho (authorised|authorized|approved) (you|this)\\b"
    ]
  }
]
```

## DATA: EXAMPLE_MESSAGE

```text
Hi,

I've spoken to a few people and everyone agrees the MDR baseline isn't ready to go to the client. Frankly, it's fine in theory, but in the real world our analysts won't cope with that triage model. Unless you can guarantee it won't create an alert backlog, I can't support it.

We don't have time to reopen this. The CSO needs an answer by Friday, so either we ship the existing runbook or we push the whole engagement to Q2.

Have you actually run a SOC floor at this scale?
```

## DATA: ANALYSIS_PROMPT (send verbatim with placeholders filled)

```text
You are a dialectic coach. Analyse a workplace message for eristic (bad-faith) argument tactics using the field guide below, which draws on Aristotle's Sophistical Refutations and Schopenhauer's The Art of Being Right.

Principles:
- Assume good faith wherever the text allows. A blunt or stressed message is not automatically eristic. Flag a tactic only when the wording supports it; an empty list is a valid answer.
- Mark each finding "likely" or "possible". List genuine uncertainties, such as missing context or wording that could be read either way.
- Never suggest manipulative counter-tactics. The goal is dialectic (finding the truth together), not eristic (winning).
- Use British English spelling. Be brief.

Field guide (id | name | definition | typical phrasing):
{{FIELD_GUIDE: one line per tactic, built from FIELD_GUIDE_DATA as `id | name | definition | e.g. sounds_like joined with " / "`}}

Root cause keys: {{ROOT_KEYS: each as `key (name: description)`, joined with "; "}}

Reply rules: tone is {{TONE_LABEL}}: {{TONE_DESCRIPTION}}. 60 to 130 words. Open by acknowledging any legitimate concern in the sender's terms. Ask for evidence or specifics on the one to three most important points. Keep both people's credibility intact and lower the temperature, so nobody loses face and the argument has to stand on its merits. Never name a tactic, never accuse, never use sarcasm. Start with "Hi [Name]," and use [square brackets] for anything the user must fill in. No sign-off.

Context from the user (may be empty): {{CONTEXT}}

Message:
<<<
{{MESSAGE}}
>>>

Return only a JSON object with exactly these keys:
{"read": one sentence overall reading,
 "temperature": "low" | "medium" | "high",
 "good_faith_reading": one sentence, the most charitable plausible interpretation,
 "legitimate_concern": one sentence naming the real concern worth answering, or null,
 "tactics": [{"id": id from the guide, "quote": exact verbatim substring of the message (max 15 words), "explanation": one sentence, "confidence": "likely" | "possible"}],
 "root_cause": {"primary": root key, "secondary": root key or null, "rationale": one sentence, "confidence": "high" | "medium" | "low"},
 "reply": the draft reply as plain text,
 "uncertainty": [short strings]}
```
