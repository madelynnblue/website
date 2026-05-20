---
title: "sqlfmt-ng"
date: 2026-05-19
---

[sqlfmt-ng](https://ng.sqlfum.pt/) is an online SQL formatter.
It's a full rewrite of the [original sqlfum.pt](/sqlfmt/) that can support multiple dialects natively.
The original used the CockroachDB parser and enhanced the AST with pretty logic.
The [Materialize version](/mz-sqlfmt/) worked the same: using the native Materialize parser and augmenting the AST.

I've wanted to support more dialects for a while using their native parsers.
There are a few projects that try to implement a generic SQL parser for many dialects.
Those may or may not work well, I'm not sure.
But certainly if you use the native parsers (say, compiled to WASM or called using FFI), then you know your parser is correct.
The difficulty is prettying that output is it needs to be converted to the pretty structs, which is a fair amount of work.

With the advent of LLMs, that work became tenable since I could ask the computer to do it instead of a human,
using the existing hand-written prettyifiers as inspiration, and also with me guiding some of its output to my preference.
It works by having a common pretty IR format that can prettyify things in that IR on the website.
Any dialect that can produce the IR can get plugged in.

Currently it supports:

- Postgres using the native Postgres parser (compiled to WASM with emscripten)
- Materialize using the native Rust parser
- JSON

I'd like to support other dialects that have a native parser, instead of using a common parser library.
