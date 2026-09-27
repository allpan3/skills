---
name: explain-diff-html
description: Create a self-contained HTML review of a code change, diff, branch, or PR, with feature-grouped explanations and complete, navigable source diffs.
---

# Explain Diff HTML

Produce a review the reader can use to understand the change and inspect its code. Use the user's language for explanations. Default to complete coverage of the requested diff; honor an explicitly narrower request for selected features or excerpts.

Start from [assets/review-template.html](assets/review-template.html). Preserve its visual system and review interactions while replacing its illustrative content with the actual change. It is a standalone HTML starter, not a required templating engine. Do not copy project-specific facts, historical measurements, or fixed feature counts from a previous review.

## Establish the snapshot

- Resolve the comparison before writing explanations: base revision, target revision or working tree, repository, and included paths. Record the actual commit IDs and how the diff was obtained. For a remote-branch comparison, refresh that ref when access permits; disclose an unavailable refresh instead of calling a stale ref current.
- Distinguish branch commits, staged changes, unstaged changes, and untracked files. For example, `git diff <base> --` combines committed and uncommitted differences; it is not automatically an uncommitted-only diff. Check whether HEAD and base have identical trees before making that claim.
- Save one exact patch snapshot outside the reviewed repository. Derive the file inventory, hunk counts, additions, deletions, displayed code, and downloadable patch from that snapshot. Read surrounding source from matching revisions for context and syntax highlighting.
- State untracked-file exclusions separately. Do not treat generated files or older review artifacts as current code. Preserve the reviewed working tree; generating a review does not require implementation edits or rerunning the project regression suite.
- If the source changes while preparing the review, retain and identify the original snapshot or deliberately regenerate it. Do not mix versions silently.

## Reading structure

Use one long page with a persistent table of contents, not top-level tabs. Scale the explanation to the change without omitting in-scope code.

1. **Scope and overview.** State the concrete behavior change, comparison basis, and snapshot statistics. Explain whether this is a complete or intentionally partial review.
2. **Background.** Explain the existing data flow and terminology needed for this change. Put deeper beginner material in an optional disclosure, followed by focused context for this review. Inspect surrounding code rather than inferring behavior only from changed lines.
3. **Intuition.** Use concrete examples and simple HTML/CSS diagrams. Add a small interactive example when it makes causality or a tradeoff easier to understand. Label illustrative inputs and assumptions; a demo is not measured evidence. Do not use ASCII diagrams.
4. **Code, grouped by feature.** Choose conceptual feature groups in dependency or reading order. Each file appears once in its primary group; cross-link shared concerns. Use the complete file/hunk structure below.
5. **Evidence and limits.** State what was actually checked, when, and against which baseline. Separate measured results from estimates, historical experiments from new runs, and local block costs from system totals. Do not imply that making this page validated the implementation.
6. **Quiz.** Provide five interactive multiple-choice questions about the real change, with immediate explanations for correct and incorrect answers. Test understanding rather than trivia; keep this section skippable.
7. **Inventory and exact patch.** Link every in-scope file to its disclosure and provide the original patch as a download.

Write clear, connected explanations: what changed, why, how it works, and where its assumptions matter. Avoid repeating a file-level summary under every hunk.

## Complete code walkthrough

For each feature, explain the old/new behavior and interactions between files before presenting the code.

For each file, show its repository-relative path, change status, additions/deletions, and hunk count. Use an expandable file card containing:

- **Before:** the old behavior at the named base.
- **After:** the behavior at the named target.
- **Review focus:** the important invariant, design choice, or consequence.
- All its original diff hunks, each in a nested disclosure with a specific local explanation and old/new line ranges. Distinguish documentation changes, wiring/call-site synchronization, and behavioral changes. Git's hunk context header may name a preceding function; do not blindly treat it as the identity of newly added code.

Preserve entire added and deleted files. Collapse long hunks and allow internal scrolling, but do not truncate them. For a large single hunk, give an additional reading guide to its major regions. Mechanical changes can have short explanations; still include their exact code.

Use unified diffs by default, with two line-number columns for old/new positions. Keep decorative line numbers out of copied text, for example with data attributes and CSS pseudo-elements. Additions are green, deletions red, and headers blue/cyan. Within each line, retain language-aware syntax highlighting in both themes.

Escape source before wrapping it in semantic token spans. Use `<pre><code class="language-diff">` for hunks and the appropriate language class for ordinary snippets. Tokenize complete matching old/new files before slicing when needed to preserve multiline strings/comments. Do not let a lexer insert or normalize source newlines. Each rendered diff line must occupy its own visual row, not merely exist in `textContent`.

## Visual and interaction contract

Use the template's restrained typography, generous spacing, teal accents, bordered cards, and complete light/dark palettes. Preserve:

- A sticky top bar with a keyboard-accessible theme toggle, and a sticky left-hand section directory on wide screens. On narrow screens, the directory enters normal flow; tables and code scroll internally without page-wide overflow.
- Scope metric cards, feature introductions, paired Before/After panels, review-focus callouts, and nested file/hunk disclosures.
- File/description search, visible-file counts, expand-filtered/collapse-all controls, per-hunk copying, inventory/deep links that reveal their targets even after filtering, and an exact patch download.
- Local CSS/JavaScript only: no external fonts, syntax-highlighter CDN, or network dependencies. The delivered page remains a single self-contained file.

On first load, use a saved theme choice or the system preference. Apply it before rendering to avoid a flash, update `color-scheme` and theme metadata, persist manual choices, and honor reduced motion. The toggle exposes its state and action through accessible attributes. Define all surfaces, text, syntax tokens, diff backgrounds, diagrams, controls, and feedback colors with theme variables; test token contrast on addition/deletion backgrounds too.

The starter contains one fictional file, demo, and quiz question to illustrate the components. Replace those examples, update all labels/IDs/search metadata/links, and generate the requested five questions. Do not ship its sample claims or sample patch in a real review.

## Validate and deliver

Before delivery:

- Compare each rendered code block's `textContent` with its exact source/hunk; compare the downloaded patch with the saved patch. Verify that every requested file/hunk appears once and totals agree. Keep any temporary manifests and validation scripts outside the reviewed repository.
- In a browser, exercise initial system themes, manual/keyboard switching, persistence, metadata, search, disclosure controls, direct links, copy/download, demo, and quiz. Check wide and narrow layouts and inspect screenshots in both themes, including an expanded code block. DOM text equality alone will not detect lines accidentally rendered horizontally.
- Confirm code escaping, indentation, and newlines; check readable contrast throughout. If browser automation is unavailable, report that limitation rather than claiming browser validation.
- Confirm the reviewed working tree was not changed by producing the page.

Save the finished HTML outside the reviewed repository with a filename beginning with today's `YYYY-MM-DD-`. Link the rendered artifact, not its source. For an SSH/VSCode workflow, provide an exact `python3 -m http.server` command serving the artifact directory on `127.0.0.1`, instructions to forward that port, and the localhost URL. Do not assume the reader can open a remote filesystem link locally. Starting a public server or publishing externally is not part of this skill.
