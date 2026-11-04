# XSS Fix Lab: Phish Report Viewer

## What this is

This is a teaching lab. The page `index.html` has a bug. The bug is on purpose. Do not use this page for real work.

The page is a small "phish report viewer." You type the sender name from a reported email. The page shows that name in a box.

## The bug in plain words

A phishing email can have any text in its sender display name. That text can include HTML.

This page puts the sender name into the box with `innerHTML`. `innerHTML` reads the name as HTML, not as plain text. So the sender name can change the page.

The bug is in one line of `index.html`:

```js
output.innerHTML = nameInput.value;
```

An attacker would put code in the name. You do not need code to see the bug. Bold text is enough. Type this name:

```
<b>Security Team</b>
```

- With the bug, the page shows **Security Team** in bold. The browser ran the tags as HTML.
- After the fix, the page shows the tags as text: `<b>Security Team</b>`.

The fix changes one line. `textContent` reads the name as plain text:

```js
output.textContent = nameInput.value;
```

## Exercise

Each step that runs CodeQL can take a few minutes. Wait for it.

1. On this repository, select **Use this template**, then **Create a new repository**.
2. Choose **Public**. (See "Why public" below.)
3. Open the **Actions** tab. Wait for the **CodeQL** run to finish. The first alert can take a few minutes after the first push. If no run starts, open Actions, select CodeQL, select Run workflow.
4. Open **Security**, then **Code scanning**. If you see no alert, read "How to read the alert" below. You can still do the fix.
5. Open the alert. Read it (see "How to read the alert" below).
6. Open `index.html`. Select the pencil icon to edit the file in the browser.
7. Find the line `output.innerHTML = nameInput.value;`. Change `innerHTML` to `textContent`. Change nothing else.
8. Select **Commit changes**. Commit to `main`.
9. Wait for the new CodeQL run. Go back to **Security**, then **Code scanning**. The alert should show as closed (fixed).

After the fix, the comment `// BUG (on purpose)` and the banner still say "bug on purpose". A reviewer would ask you to update them.

You can see the fix before you edit. In the template repository, not in your copy, open the branch `fix-sender-text`.

## How to read the alert

CodeQL may flag this line. The alert name can differ.

The CodeQL query that matches this pattern is **DOM text reinterpreted as HTML** (`js/xss-through-dom`). In CodeQL, the `value` of a page element is a source for this query. Writing to `innerHTML` is a sink. We did not run CodeQL on this exact repository before the talk. So your alert can have a different name, or CodeQL can find it under a different query.

Sources:

- Query help: https://codeql.github.com/codeql-query-help/javascript/js-xss-through-dom/
- Source list (includes `value`): https://github.com/github/codeql/blob/main/javascript/ql/lib/semmle/javascript/security/dataflow/XssThroughDomCustomizations.qll
- Query test cases (`.value` to `.html()`; `.val()` to `innerHTML`): https://github.com/github/codeql/blob/main/javascript/ql/test/query-tests/Security/CWE-079/XssThroughDom/xss-through-dom.js

An alert page has these parts:

- **Title**: the name of the problem.
- **File and line**: where CodeQL found it. Look for the `innerHTML` line in `index.html`.
- **Show paths** (if present): how the data moves. Here it starts at `nameInput.value` and ends at `innerHTML`.
- **Show more**: help text. It tells why the code is a problem and how to fix it.

## Why public

Code scanning is free only on public repos. If you choose **Private** on a free account, the CodeQL alert does not show.

## Do not put real data here

This repository is public. Everyone can see it. Do not put real email, real names, passwords, or work data in it. Use only test text, such as `<b>Security Team</b>`.

## AI use

- **Tool:** Claude (Anthropic)
- **Produced:** first draft of the page, workflow, and README
- **Verified:** page tested in a headless browser before and after the fix; see `TEST-OUTPUT.md`
- **Reviewed by:**
