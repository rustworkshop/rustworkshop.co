---
title: Choosing a Rust web framework
layout: post
author: Tim Abell
---

For my next side-project I'm going to do a small web based system (party invites), and unlike in dotnet and ruby on rails where there is ONE TRUE WAY™ to build web things there are several competing backend frameworks / crates for web backends in Rust, and even more front-end things (as always seems to be the way with front-end these days, especially now WASM has got serious).

I've already [surveyed the available Rust books](/2023/06/18/rust-programming-books/), and figured out which ones are for web / api work, but not which ones use what frameworks.

I started with a question on reddit: [r/rust "What's the current best practice for web dev in rust?"](https://www.reddit.com/r/rust/comments/17dt9bo/whats_the_current_best_practice_for_web_dev_in/) which flushed out some good advice and things to check out. I then followed up with some ~~googling~~ [DuckDuckGo](https://duckduckgo.com/)ing.

I want to see just how far I can push the full-stack-Rust thing, so will avoid things like HTMX that are javascript based. I'll start with some old-school back-end development of a static website and then venture tentatively into fancier things (SPAs and Mobile apps) once that's working. As well as building something real and useful, the secondary objective is to develop an opinion for the Rust Workshop on what a good default Rust web stack would look like. (Opinions being like [standards](https://xkcd.com/927/) and frameworks, by surveying all the existing opinions and forming another, now there's just one more; but that doesn't change the fact that an opinion is needed here).

Here's what I've learned so far about what's out there:

## Async

Note that your crates need to use the same async approach / crate I gather otherwise all-hell / incompatibility will be upon you. (i.e. tokio all the things). Unlike in C# and many other languages, Rust doesn't force a particular async "runtime" on you (for better or worse), so you and the crates you depend on have to make your own informed choice.

The article ["Was Rust Worth It? From JavaScript to Rust, three years in" by Jarrod Overson](https://jsoverson.medium.com/was-rust-worth-it-f43d171fb1b3) shares that:

> "After two decades of JavaScript and decent experience with Go, this is the most significant source of frustration and friction with Rust"

So worth at least trying to start in the "right" part of the `async` ecosystem.

As an aside there's a temptation to think `async` is a panacea for speed, but [async doesn't solve every performance problem (tweet)](https://twitter.com/tim_abell/status/1725054318108197032). The main strength of async is in I/O blocked parallel operations, exactly the kind of thing webservers are (lots of waiting on disk and network while trying to serve thousands or millions of requests/clients in parallel).

It seems that currently [tokio's async](https://tokio.rs/tokio/tutorial/async) is the defacto standard.

## Resources

- [The Rust Workshop survey of the available Rust books](/2023/06/18/rust-programming-books/)- [blessed.rs has a curated crate list](https://blessed.rs/crates#section-networking-subsection-http-foundations)
- [lib.rs](https://lib.rs/crates) has useful stats on it, e.g. [the rocket crate page](https://lib.rs/crates/rocket) shows it's #5 (presumably by downloads)
  - "*Lightweight, opinionated, curated, unofficial alternative to crates.io*".
  - it has a [list of http crates](https://lib.rs/web-programming/http-server) - the top four are axum, actix, warp and rocket
- [Server framework benchmarks by techempower](https://www.techempower.com/benchmarks/#section=data-r22&hw=ph&test=plaintext)
- [Rust Digger](https://rust-digger.code-maven.com/) via the [Rustacean Station Podcast - Rust Digger](https://rustacean-station.org/episode/gabor-szabo/)

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
  - Built on top of [hyper.rs](https://hyper.rs/)
    - Which is built on tokio (so uses tokio-async)

## Frontend

- [HTMX](https://htmx.org/) is a tiny javascript library for web interfaces, but not really anything to do with Rust
- [Leptos](https://leptos.dev/) ...
  - [lib.rs - leptos](https://lib.rs/crates/leptos)