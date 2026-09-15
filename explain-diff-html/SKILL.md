---
name: explain-diff-html
description: Use when the user asks for a rich explanation of a code change, diff, branch, or PR. Produces HTML output.
---

# Explain Diff

Please make me a rich, interactive explanation of the specified code change.

It should have these sections:

- Background: Explain the existing system relevant to this change. (You should broadly explore surrounding code for this.) We don't know how much the reader already knows, so include a deep background for beginners (note that it can be skipped if the reader is already familiar), and then a more narrow background directly relevant to the change.
- Intuition: Explain the core intuition for the code change. The focus here is to explain the essence, not the full details. Use concrete examples with toy data. Use figures and diagrams liberally.
- Code: Do a high-level walkthrough of the changes to the code. Group/order the changes in an understandable way.
- Quiz: Come up with five questions that test the reader's knowledge of this PR. This should be medium difficulty, difficult enough that you actually need to understand the substance of the PR to answer them, but not gotchas. The goal is to help the reader make sure that they've actually understood. These should be presented as interactive multiple-choice questions, and when the user clicks, it tells them whether they were correct and gives feedback.

Format:

- Output a single self-contained HTML file which includes CSS and JavaScript. Make the whole thing one long page with section headers and a table of contents. Don't use tabs for the top-level structure. Basic responsive styling so you can view it on a phone is nice too. Put the file in a global place on my computer outside of the code repo, and make sure the filename always starts with today's date in `YYYY-MM-DD-` format, because it helps keep the files time-sorted and out of version control. For example: /tmp/2026-01-12-explanation-<slug>.html
- Include complete light and dark appearance modes for the entire page, including backgrounds, text, borders, callouts, diagrams, controls, quiz feedback, inline code, code blocks, and every syntax token color. Define both palettes with CSS custom properties and apply them consistently; do not leave components with fixed colors that become illegible in either mode.
- Put an accessible light/dark toggle in a persistent, easy-to-find location. It must work without reloading the page, expose its current state with an appropriate label and `aria-pressed` or equivalent semantics, and be operable by keyboard.
- On first load, use a previously saved choice from `localStorage`; if none exists, follow `prefers-color-scheme`. Save explicit toggle choices so future visits use the reader's preference. Set `color-scheme` and update browser-facing theme metadata when the active mode changes.
- Avoid a flash of the wrong theme: apply the initial mode before the page renders whenever practical. Honor `prefers-reduced-motion` and do not require animated transitions for the theme change.
- Please write with the clarity and flow of Martin Kleppmann, making it engaging and written in classic style. Transitions between sections should be smooth.
- Some tips on diagrams. Ideally, you should pick a small number of diagram families that can be reused throughout the explanation to explain various cases. Some useful kinds of diagrams:
  - A very simplified version of the UI that the user sees in the app, to explain UI changes.
  - A system diagram showing data flow or communication between components. Make sure to include example data here!
- Don't use ASCII diagrams. Always use simple HTML designs for your diagrams, HTML lists for lists of things, etc.
  - Syntax-highlight every code block with language-aware colors in both appearance modes. Keep the page self-contained: do not load a highlighting library, stylesheet, font, or other asset from a CDN or external URL.
  - Use `<pre><code class="language-<name>">...</code></pre>`. HTML-escape the source first, then wrap tokens in semantic spans such as `tok-keyword`, `tok-string`, `tok-number`, `tok-comment`, `tok-type`, `tok-function`, `tok-property`, and `tok-operator`. Define a legible, high-contrast color for each token class in the page's embedded CSS.
  - Highlight diff blocks semantically: additions in green, deletions in red, hunk or file headers in cyan or blue, and unchanged context in the default code color.
  - Preserve the exact code text, indentation, and newlines so it remains readable and copyable. Style `<pre>` with `white-space: pre` or `pre-wrap` and horizontal overflow as needed.
  - Before saving, inspect every code block in the HTML source and confirm that code is escaped, highlighting markup does not alter the displayed text, token colors are visibly distinct in both light and dark modes, and newlines are preserved.
- Before saving, test the initial system-preference behavior, manual light/dark switching, persistence after reload, keyboard operation, and readability of every major component in both modes.
- Use callouts for key concepts or definitions, important edge cases, etc.
