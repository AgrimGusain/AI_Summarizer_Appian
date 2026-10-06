# AI Summarizer

A small prototype built on **Appian Community Edition** that turns pasted text (emails, articles, meeting notes) into an AI-generated summary. Appian's low-code interface designer handles the UI, and an Appian Connected System and Integration calls the Groq LLM API.

![AI Summarizer screenshot](screenshot.jpg)

## What it does

1. Paste text or an email into the box.
2. Pick a summary style: **Short**, **Detailed**, or **Action items**.
3. Click **Summarize** and the summary appears in a card below.

Extras: live character count, a "Summarizing..." loading state, a Clear button, and a friendly error message if the API call fails.

## Tech

| Piece | What I used |
|---|---|
| Platform | Appian Community Edition (low-code) |
| UI | Appian SAIL interface (`AIS_SummarizerApp`) |
| API call | Connected System + Integration (`AIS Groq API`, `AIS_GroqSummarize`) |
| LLM | Groq chat-completions API |
| Delivery | Appian Site ("AI Summarizer") with a *Summarize Text* page |

The Groq API key is stored in the Appian Connected System, not in the code or in this repo.

## Build log

1. Created the Appian application `AI Summarizer` and an `AIS` object prefix.
2. Registered the Groq API as a **Connected System** and wrote an **Integration** (`AIS_GroqSummarize`) that takes `text` and `style` and returns the model's reply.
3. Built the interface with three local variables (`text`, `style`, `summary`) and a Summarize button. Its `saveInto` calls the integration, and `onSuccess` and `onError` write the result into `summary`.
4. Polished the UI into numbered steps with instructions, a character counter, radio-button styles, a result card, a loading state, and a Clear button.
5. Created an Appian **Site** and added the interface as the *Summarize Text* page, so it runs as a real app instead of a designer preview.
6. Hit and fixed a few validation errors along the way (invalid `formLayout` parameter, `refreshAfter` value, and button `style` values must be `SOLID`/`OUTLINE`).

## Things I learned

- SAIL components are strict about parameter values, and the designer's error messages say exactly which ones are valid.
- Integrations return the raw HTTP response, so the JSON body has to be parsed with `a!fromJson()` before reading `choices[1].message.content`.
- A page needs a Site to be used outside the designer preview.

## Limitations / next steps

- The summary shows the model's Markdown as plain text (for example `**Summary:**`).
- No history is saved yet. A record type could store past summaries.
- File upload (PDF or DOCX) is not supported yet.
- The live app runs inside Appian Community Edition and needs an Appian login, so this repo has the screenshot and write-up.

## Demo

Screenshot above. A short screen recording is linked here: _add link_

