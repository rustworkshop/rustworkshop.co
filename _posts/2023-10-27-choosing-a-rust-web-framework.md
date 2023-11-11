---
title: Choosing a rust web framework
layout: post
author: Tim Abell
---

For my next side-project I'm going to do a small web based system (party invites), and unlike in dotnet and ruby on rails where there is ONE TRUE WAY™ to build web things there are several competing backend frameworks / crates for web backends in rust, and even more front-end things (as always seems to be the way with front-end these days, especially now WASM has got serious).

I've already [surveyed the available rust books](/2023/06/18/rust-programming-books/), and figured out which ones are for web / api work, but not which ones use what frameworks.

I started with a question on reddit: [r/rust "What's the current best practice for web dev in rust?"](https://www.reddit.com/r/rust/comments/17dt9bo/whats_the_current_best_practice_for_web_dev_in/) which flushed out some good advice and things to check out. I then followed up with some ~~googling~~ [DuckDuckGo](https://duckduckgo.com/)ing.

Here's what I've learned so far about what's out there.

## Resources

- [blessed.rs has a curated crate list](https://blessed.rs/crates#section-networking-subsection-http-foundations)
- [lib.rs](https://lib.rs/crates) has useful stats on it, e.g. [the rocket crate page](https://lib.rs/crates/rocket) shows it's #5 (presumably by downloads)
  - "*Lightweight, opinionated, curated, unofficial alternative to crates.io*".
  - it has a [list of http crates](https://lib.rs/web-programming/http-server) - the top four are axum, actix, warp and rocket

## Backend

- [Axum by tokio](https://github.com/tokio-rs/axum) - recommended, part of tokio (the async framework of choice)
  - [lib.rs - axum](https://lib.rs/crates/axum)
- [Actix](https://actix.rs/) - speedy(er?)
  - [lib.rs - actix](https://lib.rs/crates/actix)
- [Rocket](https://rocket.rs/) - inconsistent development & releases, but otherwise good and solid according to blessed.rs
  - [lib.rs - salvo](https://lib.rs/crates/salvo)
- [Salvo](https://salvo.rs/)
  - [PR for adding salvo to blessed.rs](https://github.com/nicoburns/blessed-rs/pull/81/files)
  - [lib.rs - salvo](https://lib.rs/crates/salvo)
- [Askama](https://djc.github.io/askama/) - a template engine
- [Warp](https://crates.io/crates/warp)
  - [lib.rs - warp](https://lib.rs/crates/warp)
  - <https://blog.logrocket.com/building-rest-api-rust-warp/>

## Frontend

- [HTMX](https://htmx.org/) is a tiny javascript library for web interfaces, but not really anything to do with Rust
- [Leptos](https://leptos.dev/) ...
  - [lib.rs - leptos](https://lib.rs/crates/leptos)

Note that your crates need to use the same async approach / crate I gather otherwise all-hell / incompatibility will be upon you. (i.e. tokio all the things)

I want to see just how far I can push the full-stack-rust thing, so will avoid thing like htmx that are javascript based.
