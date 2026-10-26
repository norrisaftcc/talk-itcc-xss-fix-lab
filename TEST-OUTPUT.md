# Test output

This file records a test of `index.html` in a headless browser. It describes the page on `main` (with the bug) and on the fix branches.

## How we tested

- Tool: Playwright with headless Chromium. The test script is not in this repository.
- For each branch, the test opened that branch's `index.html` from a local file.
- It typed `<b>Security Team</b>` in **Sender display name**. Then it selected **Show**.
- On `main`, the test expects one `<b>` element in the output box (bold text).
- On `fix-sender-text` and `fix-sender-text-2`, the test expects no `<b>` element. The text must show the tags as typed.
- The test also checks for script errors and for requests to other sites. It found none.

The blob value is the Git ID of `index.html` on that branch. You can check it with `git rev-parse --short <branch>:index.html`.

## Raw output

```
Browser: Chromium 153.0.8010.12 (headless, Playwright)
[main, index.html blob 3a4f683] input: <b>Security Team</b>
  <b> elements in output: 1
  output textContent:     "Security Team"
  output innerHTML:       "<b>Security Team</b>"
  <b> font-weight:        700
  page errors / external requests: none
  expected: bold rendered (one <b> element)
  result: PASS
[fix-sender-text, index.html blob a61af59] input: <b>Security Team</b>
  <b> elements in output: 0
  output textContent:     "<b>Security Team</b>"
  output innerHTML:       "&lt;b&gt;Security Team&lt;/b&gt;"
  <b> font-weight:        null
  page errors / external requests: none
  expected: literal tags as text (no <b> element)
  result: PASS
[fix-sender-text-2, index.html blob a61af59] input: <b>Security Team</b>
  <b> elements in output: 0
  output textContent:     "<b>Security Team</b>"
  output innerHTML:       "&lt;b&gt;Security Team&lt;/b&gt;"
  <b> font-weight:        null
  page errors / external requests: none
  expected: literal tags as text (no <b> element)
  result: PASS
Summary: 3/3 passed
```

## Screenshots (descriptions only)

The test took two screenshots. The images are not in this repository.

1. **`main` (with the bug):** The yellow banner says "Teaching lab. This page has a bug on purpose." The input box shows `<b>Security Team</b>`. Under **Sender**, the output box shows "Security Team" in bold. No tags show. The browser read the tags as HTML.
2. **`fix-sender-text` (after the fix):** The page is the same. The output box shows `<b>Security Team</b>` in normal weight, with the tags as text. The browser did not read the tags as HTML.

## Limits

- This test checks one harmless input in one browser (Chromium). It does not prove that the page is safe for all input.
- This test does not run CodeQL. It does not show whether a code scanning alert opens or closes.
