---
title: Choosing a Rust web framework
layout: post
author: Tim Abell
---


Unlike in dotnet and ruby on rails where there is ONE TRUE WAY™ to build web things there are several competing backend frameworks / crates for web backends in Rust, (and even more front-end things, especially now WASM is a serious option).

This post lays out the current lay of the land to help choose the right tool for the job.

For more detail, there's the [survey of the available Rust books](/2023/06/18/rust-programming-books/), which categorises the ones that area specifically for web / api work.

Asking ["What's the current best practice for web dev in rust? on reddit"](https://www.reddit.com/r/rust/comments/17dt9bo/whats_the_current_best_practice_for_web_dev_in/) flushed out some good advice and things to check out.

As well as learning and sharing, this post is here to help develop an opinion for the Rust Workshop on what a good default would be for a web stack for teams that love Rust.

## Async

In Rust there are multiple implementations of the async language features, available as creates. As such your web framework and other crates that you use need to be compatible.


> "After two decades of JavaScript and decent experience with Go, [async] is the most significant source of frustration and friction with Rust"
>
> ~ [Jarrod Overson, "Was Rust Worth It? From JavaScript to Rust, three years in"](https://jsoverson.medium.com/was-rust-worth-it-f43d171fb1b3#:~:text=this%20is%20the%20most%20significant%20source%20of%20frustration)

So it's worth at least trying to start in the "right" part of the `async` ecosystem.

As an aside there's a temptation to think `async` is a panacea for speed, but [async doesn't solve every performance problem (tweet)](https://twitter.com/tim_abell/status/1725054318108197032). The main strength of async is in I/O blocked parallel operations, exactly the kind of thing webservers are (lots of waiting on disk and network while trying to serve thousands or millions of requests/clients in parallel).

### Async and Performance

Coming from C# where async is embedded in the language and standard "dotnet framework" libraries you don't really consider much whether to use async as it just infects everything you do. Here in rust there is more choice. It's worth noting that where async shines is in handling massive concurrent load on network servers where there's a lot of waiting on IO (disk/network). But for a lot of applications the concurrent load isn't the limiting factor so async is potentially unnecessary complexity.

### Available async runtimes

- [Tokio's async](https://tokio.rs/tokio/tutorial/async)
  - some people find the `'static` lifetime bound on tokio tasks to be restrictive, or at least a bit tricky to use (though for writing an API the spawning is likely handled per-request by the framework so less of an issue):
    - <https://tokio.rs/tokio/tutorial/spawning#static-bound>
    - <https://stackoverflow.com/questions/69955340/how-to-deal-with-tokiospawn-closure-required-to-be-static-and-self>
    - <https://github.com/tokio-rs/tokio/issues/2170>
    - <https://users.rust-lang.org/t/working-around-the-static-requirement-in-tokio-spawn-blocking/89831>
    - <https://users.rust-lang.org/t/satisfying-tokio-spawn-static-lifetime-requirement/78773>
- [async-std](https://docs.rs/async-std/latest/async_std/) - "Async version of the Rust standard library" 
- [embassy](https://embassy.dev/) - "Embassy is the next-generation framework for embedded applications"
- [smol](https://github.com/smol-rs/smol) - "A small and fast async runtime."

<https://rust-lang.github.io/async-book/>


## Web & API frameworks & crates for the backend

### Frameworks

Here's the biggest frameworks / crates out there for doing server-side (aka backend) web development for both APIs and server-rendered html.

- [Axum by tokio](https://github.com/tokio-rs/axum) - recommended, part of tokio (the async framework of choice) - focus on a macro free API which simplifies error handling and debugging
  - [axum on lib.rs](https://lib.rs/crates/axum)
- [Actix](https://actix.rs/) - uses macros for routing api and the actor pattern for internal communication. Considered fast.
  - [actix on lib.rs](https://lib.rs/crates/actix)
  - [Rustacean Station: Kraken's migration to Rust microservices, with Rob Ede](https://rustacean-station.org/episode/rob-ede-kraken/)
- [Rocket](https://rocket.rs/) - inconsistent development & releases, but otherwise good and solid according to blessed.rs. Uses macros to routing api and general web boilerplate. A popular choice if you don't mind some magic happening behind the scenes. 
  - [rocket on lib.rs](https://lib.rs/crates/rocket)
- [Salvo](https://salvo.rs/)
  - [PR for adding salvo to blessed.rs](https://github.com/nicoburns/blessed-rs/pull/81/files)
  - [salvo on lib.rs](https://lib.rs/crates/salvo)
- [Warp](https://crates.io/crates/warp)
  - [warp on lib.rs](https://lib.rs/crates/warp)
  - <https://blog.logrocket.com/building-rest-api-rust-warp/>
  - Built on top of [hyper.rs](https://hyper.rs/)
    - Which is built on tokio (so uses tokio-async)
- [tide](https://github.com/http-rs/tide)
  - [tide on lib.rs](https://lib.rs/crates/tide)

Your choice may be affected by how you feel about macros - do you want to avoid magic and trickier error messages that you may get with a macro based framework, or are you happy to just write your business logic and leave all that to the framework.

### Templating engines

One piece of the puzzle

- [Askama](https://djc.github.io/askama/) - a template engine based on Jinja
  - [askama on lib.rs](https://lib.rs/crates/askama)

## Frontend - Javascript, Rust and WASM

Frontend web is a complex world these days, and the ecosystem has exploded in the last decade. The objective here is not an exhaustive survey, rather a jumping off point for the most common modern staring points that would likely lead to a successful project.

Particularly with bootstrapping on a budget we can iterate our way to fancier things, starting with simpler server-side html rendering ("SSR") and then graduating to complex systems as needs and resources dictate.

There are some fundamental needs of web development that need to be kept in mind when choosing a system, and some of the choices you make up front can make it very hard to meet some of these needs as they become important for you later without a serious rewrite, so bear these in mind:

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
- Vue.js

There are many others, I'm not even going to attempt a complete survey of js things for the time being.

## Further Resources

- [The Rust Workshop survey of the available Rust books](/2023/06/18/rust-programming-books/)- [blessed.rs has a curated crate list](https://blessed.rs/crates#section-networking-subsection-http-foundations)
- [lib.rs](https://lib.rs/crates) has useful stats on it, e.g. [the rocket crate page](https://lib.rs/crates/rocket) shows it's #5 (presumably by downloads)
  - "*Lightweight, opinionated, curated, unofficial alternative to crates.io*".
  - it has a [list of http crates](https://lib.rs/web-programming/http-server) - the top four are axum, actix, warp and rocket
- [Server framework benchmarks by techempower](https://www.techempower.com/benchmarks/#section=data-r22&hw=ph&test=plaintext)
- [Rust Digger](https://rust-digger.code-maven.com/) via the [Rustacean Station Podcast - Rust Digger](https://rustacean-station.org/episode/gabor-szabo/)
- [Rust web frameworks: development in 2023 ~ Yalantis](https://yalantis.com/blog/rust-web-frameworks/)
- Old article on async - much has changed since: <https://bryangilbert.com/post/code/rust/adventures-futures-tokio-rust/>

## Thanks

Many thanks to David Haig for early review and significant contributions to this article.