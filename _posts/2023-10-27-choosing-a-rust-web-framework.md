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

## Web & API frameworks & crates for the backend

Here's the biggest frameworks / crates out there for doing server-side (aka backend) web development for both APIs and server-rendered html.

- [Axum by tokio](https://github.com/tokio-rs/axum) - recommended, part of tokio (the async framework of choice)
  - [axum on lib.rs](https://lib.rs/crates/axum)
- [Actix](https://actix.rs/) - speedy(er?)
  - [actix on lib.rs](https://lib.rs/crates/actix)
- [Rocket](https://rocket.rs/) - inconsistent development & releases, but otherwise good and solid according to blessed.rs
  - [salvo on lib.rs](https://lib.rs/crates/salvo)
- [Salvo](https://salvo.rs/)
  - [PR for adding salvo to blessed.rs](https://github.com/nicoburns/blessed-rs/pull/81/files)
  - [salvo on lib.rs](https://lib.rs/crates/salvo)
- [Askama](https://djc.github.io/askama/) - a template engine
- [Warp](https://crates.io/crates/warp)
  - [warp on lib.rs](https://lib.rs/crates/warp)
  - <https://blog.logrocket.com/building-rest-api-rust-warp/>
  - Built on top of [hyper.rs](https://hyper.rs/)
    - Which is built on tokio (so uses tokio-async)

## Frontend - Javascript, Rust and WASM

Frontend web is a complex world these days, and the ecosystem has exploded in the last decade. The objective here is not an exhaustive survey, rather a jumping off point for the most common modern staring points that would likely lead to a successful project.

Particularly with bootstrapping on a budget we can iterate our way to fancier things, starting with simpler server-side html rendering ("SSR") and then graduating to complex systems as needs and resources dictate.

There's some fundamental needs of web development that need to be kept in mind when choosing a system, and some of the choices you make up front can make it very hard to meet some of these needs as they become important for you later without a serious rewrite, so bear these in mind:

1. Search engines - client-rendered html is often invisible to search engines which can be a massive problem, hence the rise of "isomorphic" and "hydration" approaches that, while complicated, allow the best of client and server worlds
2. Dynamic, responsive user experiences - full server page rendering is easier to get started, but inevitably less responsive to the user than SPA designs that can minimize server round-trips and complete page re-renders. You can sprinkle a little ajax magic to selected parts of a server-rendered page, but a fully responsive web application is impossible to achieve with full page server rendering as a base without a major rewrite. Consider the balance of engineering cost and user experience, and the desired long term shape of your web site or web application.

### Rust frontend systems

- [Leptos](https://leptos.dev/)
  - [leptos on lib.rs](https://lib.rs/crates/leptos)
  - Embed html in rust code (like react).
  - Can do server rendering, client rendering, hydration etc.

### Non-rust

Even if your backend is Rust, you don't *have* to do the frontend in Rust too. I'm going to try for the edification, but for completeness here's some javascript tools you could use to build your html. Some of these were mentioned by folks when asked about web dev in Rust, some are just the big names out there.

- [HTMX](https://htmx.org/) is a tiny javascript library for web interfaces, mentioned by a r/Rust reddit user as part of their stack.
- React - the big one it seems - ["Should you use React in 2023?" from the LogRocket podcast](https://podrocket.logrocket.com/react-in-2023) gives a good overview of the current state of things
- Angular - I really don't hear so much about this these days, and the big rewrite burned a lot of people.

There are many others

## Further Resources

- [The Rust Workshop survey of the available Rust books](/2023/06/18/rust-programming-books/)- [blessed.rs has a curated crate list](https://blessed.rs/crates#section-networking-subsection-http-foundations)
- [lib.rs](https://lib.rs/crates) has useful stats on it, e.g. [the rocket crate page](https://lib.rs/crates/rocket) shows it's #5 (presumably by downloads)
  - "*Lightweight, opinionated, curated, unofficial alternative to crates.io*".
  - it has a [list of http crates](https://lib.rs/web-programming/http-server) - the top four are axum, actix, warp and rocket
- [Server framework benchmarks by techempower](https://www.techempower.com/benchmarks/#section=data-r22&hw=ph&test=plaintext)
- [Rust Digger](https://rust-digger.code-maven.com/) via the [Rustacean Station Podcast - Rust Digger](https://rustacean-station.org/episode/gabor-szabo/)
