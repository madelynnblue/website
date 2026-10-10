---
title: "SQL Pretty Printer with Comments"
date: 2026-10-08
---

I've long had a silly interest in [SQL formatters](/sqlfmt-ng).
All of the ones I've written so far have not had support for comments.
The parsers omit comments when generating the AST, so there's no good way to inject them back into the output.
Parsers omit comments and whitespace because they are trivia and not significant.
I've wanted to add this feature for a while, and recently figured out how.

[Red-Green Trees](https://github.com/dotnet/roslyn/blob/main/docs/compilers/Design/Red-Green%20Trees.md) are a kind of parser that retains comments and whitespace while still providing everything else needed by an AST.
I implemented a tiny SQL dialect using [rowan](https://github.com/rust-analyzer/rowan), a Rust library that implements this pattern.
Then I taught sqlfmt-ng how to render comments.

For the query

```
SELECT 
1,   /* 3 */
12,  /* 2 */
123, /* 1 */
1234 /* 0 */
-- FROM block:
FROM T -- the table
```

what we would like is:

- the spaces before the block comments (`/* ... */`) are preserved
- block comments can be inlined if there's enough space
- the line comment (`-- ...`) either stays attached to its proceeding token or on its own line
- line comments can not be inlined

sqlfmt-ng at 0 width produces:

```
SELECT
	1,   /* 3 */
	12,  /* 2 */
	123, /* 1 */
	1234 /* 0 */
-- FROM block:
FROM
	T -- the table
```

and at a middle width:

```
SELECT
	1, /* 3 */ 12, /* 2 */ 123, /* 1 */ 1234 /* 0 */
-- FROM block:
FROM T -- the table
```

and at the highest width:

```
SELECT 1, /* 3 */ 12, /* 2 */ 123, /* 1 */ 1234 /* 0 */
-- FROM block:
FROM T -- the table
```

Neat!
I'm unlikely to do much more with this since it's a toy dialect, but understanding the principles here might influence me in the future.
