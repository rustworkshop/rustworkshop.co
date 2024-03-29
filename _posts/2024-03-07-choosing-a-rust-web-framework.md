---
title: Choosing a Rust web framework
layout: post
author: Tim Abell
reddit: https://www.reddit.com/r/rust/comments/1b974xn/choosing_a_rust_web_framework/
---


Unlike in dotnet and ruby on rails where there is more or less ONE TRUE WAY™ to build web things there are several competing backend frameworks / crates for web backends in Rust. There are many choices for the front-end with and without Rust, but for this post I'm just going to look at backend options as we want to build something more interesting on the server than an single-page-application (SPA) or static site.

What we're looking for in this post is a sensible default choice for the average API / web project. This post lays out the current lay of the land and proposes that with all the options available the Tokio "stack" is a good starting point.

Before we start looking at the actual web crates we need to talk a bit about async...

## Async in Rust

> In Rust there are *multiple* async runtime crates available.

Unlike C# where async has one language-supported way to do it that works out-of-the-box, the Rust language provides some of the pieces but to actually use async you'll need an "async runtime" crate. (More info in ["The Rust Book" section on async support](https://rust-lang.github.io/async-book/01_getting_started/03_state_of_async_rust.html#language-and-library-support).)

In Rust there are *multiple* async runtime crates available.

The crates that you are actually trying to use for your project might require a particular async runtime to work. As such your web framework and other crates that you use need to be compatible.

> "After two decades of JavaScript and decent experience with Go, [async] is the most significant source of frustration and friction with Rust"
>
> ~ [Jarrod Overson, "Was Rust Worth It? From JavaScript to Rust, three years in"](https://jsoverson.medium.com/was-rust-worth-it-f43d171fb1b3#:~:text=this%20is%20the%20most%20significant%20source%20of%20frustration)

The consequence of this is that your up-front choice of async runtime will likely be hard to change further down the road. Your choice of web framework might in turn dictate a particular async runtime, so will also be hard to change.

### Async and Performance

Coming from C# where async is embedded in the language and the standard "dotnet framework" libraries, you don't really consider much whether to use async as it just infects everything you do for better or worse.

Here in Rust there is more choice. It's worth noting that where async shines is in handling massive concurrent load on network servers where there's a lot of waiting on IO (disk/network). For many applications however the concurrent load isn't the limiting factor so async is potentially unnecessary complexity.

There's a temptation to think "async" is a panacea for speed, but async doesn't solve every performance problem. The main strength of async is in I/O blocked parallel operations, exactly the kind of thing webservers do (i.e. lots of waiting on disk and network while trying to serve thousands or millions of requests/clients in parallel).

To learn more about async in Rust the ["Rust Async Book"](https://rust-lang.github.io/async-book/) is excellent.

So what are our options for async in Rust?

### Available Async Runtimes

Here's the main options for bringing the async features of the rust language to life. Without one of these you can use some of the async keywords but they won't actually do anything.

#### Tokio

[Tokio](https://docs.rs/tokio/) is far more than an async runtime. It has many of the things you'll need to build a web API or host a dynamic server-rendered website. As such choosing Tokio for your async runtime implies a set of other choices (and vice versa if you choose other parts of the tokio stack, you'll likely use the tokio async runtime.)

[Tokio's async](https://tokio.rs/tokio/tutorial/async) is "A runtime for writing reliable network applications without compromising speed."

  - Tokio also provides the [Axum](https://github.com/tokio-rs/axum) web application framework.
  - There is a crate for bridging some of the incompatibility between tokio and "futures": [async_compat](https://docs.rs/async-compat/)
  - It was suggested to me that some people find the `'static` lifetime bound on tokio tasks to be restrictive, or at least a bit tricky to use (though for writing an API the spawning is likely handled per-request by the framework so less of an issue). It also is something that I understand might be not unique to tokio according to the reddit responses to this blog post. Here's a flavour of that particular challenge:
    - <https://tokio.rs/tokio/tutorial/spawning#static-bound>
    - <https://stackoverflow.com/questions/69955340/how-to-deal-with-tokiospawn-closure-required-to-be-static-and-self>
    - <https://github.com/tokio-rs/tokio/issues/2170>
    - <https://users.rust-lang.org/t/working-around-the-static-requirement-in-tokio-spawn-blocking/89831>
    - <https://users.rust-lang.org/t/satisfying-tokio-spawn-static-lifetime-requirement/78773>
    - <https://without.boats/blog/the-scoped-task-trilemma/>

#### Async-std

[async-std](https://docs.rs/async-std/) is an "Async version of the Rust standard library".

Perhaps better for smaller more focussed usages of async rather than "generic web development" where Tokio provides more out of the box.

#### Embassy

[Embassy](https://embassy.dev/) - "Embassy is the next-generation framework for embedded applications".

Made for use in embedded / low power devices with more constraints than your average cloud web host. If you're building something for the embedded space this is definitely one to check out.

#### Smol

[Smol](https://github.com/smol-rs/smol) is declared to be "A small and fast async runtime."

Seems worth a look if you want compactness over general applicability but doesn't appear to have the crate ecosystem that Tokio benefits from.

#### Which to choose?

For the average web/API project Tokio seems like a good starting point, with a large ecosystem and many crates you can use with it.

## Web & API frameworks & crates for the backend

Now that we've looked at the async runtimes and how it affects the rest of your choices, let's have a look at what's available for building server driven web applications and APIs in Rust.

Here's the biggest frameworks / crates out there for doing server-side (aka backend) web development for both APIs and server-rendered html.

### Available web frameworks

#### Axum

[Axum by tokio](https://github.com/tokio-rs/axum) - recommended, part of tokio (the async framework of choice) - focus on a macro free API which simplifies error handling and debugging

- [axum on lib.rs](https://lib.rs/crates/axum)

#### Actix

[Actix](https://actix.rs/) - uses macros for routing API and the actor pattern for internal communication. Considered fast.

- [actix on lib.rs](https://lib.rs/crates/actix)
- [Rustacean Station: Kraken's migration to Rust microservices, with Rob Ede](https://rustacean-station.org/episode/rob-ede-kraken/)

#### Rocket

[Rocket](https://rocket.rs/) - inconsistent development & releases, but otherwise good and solid according to blessed.rs. Uses macros to routing API and general web boilerplate. A popular choice if you don't mind some magic happening behind the scenes. 

- [rocket on lib.rs](https://lib.rs/crates/rocket)

#### Salvo

[Salvo](https://salvo.rs/) "A powerful and simple Rust web server framework"

- [PR for adding salvo to blessed.rs](https://github.com/nicoburns/blessed-rs/pull/81/files)
- [Salvo on lib.rs](https://lib.rs/crates/salvo)

#### Warp

[Warp](https://crates.io/crates/warp) "A super-easy, composable, web server framework for warp speeds."

- [Warp on lib.rs](https://lib.rs/crates/warp)
- <https://blog.logrocket.com/building-rest-api-rust-warp/>
- Built on top of [hyper.rs](https://hyper.rs/)
  - Which is built on tokio (so uses tokio-async)

#### Tide

[Tide](https://github.com/http-rs/tide) "a minimal and pragmatic Rust web application framework built for rapid development. It comes with a robust set of features that make building async web applications and APIs easier and more fun"

- [Tide on lib.rs](https://lib.rs/crates/tide)

#### Poem

>  "Poem is a full-featured and easy-to-use web framework"

I don't know much about this one yet, but a couple of people on reddit suggested this was nice to work with on the reddit responses to this post.

- <https://docs.rs/poem/>
- <https://github.com/poem-web/poem>
- <https://news.ycombinator.com/item?id=32800796>
- <https://www.reddit.com/r/rust/comments/s0g9x9/thoughts_on_poem_axum/>
- <https://tech.marksblogg.com/poem-rust-web-framework.html>

#### Hyper

> "hyper is a fast and correct HTTP implementation"

Hyper provides and http server, but not a lot of the rest of the things you'd need for a full backend web application such as routing and templating.

- <https://hyper.rs/>
- <https://docs.rs/hyper/>
- <https://github.com/hyperium/hyper>

#### Pavex

- <https://github.com/LukeMathWalker/pavex>
- <https://pavex.dev/>

Currently in closed-access beta, this is an interesting looking new thing that promises to provide far better developer experience in the domain of backend web systems. Notably because of the pre-compilation parsing that allows it to provide programming error information to the programmer that explains the actual problem is within the context of backend development rather than some mysterious type/trait error. It also has a pretty clean design for describing your particular service.

You can [watch Luca introduce and demonstrate Pavex at Rust Nation 2024](https://www.youtube.com/live/6mZRWFQRvmw?feature=shared&t=8488).

### Macros

Your choice may be affected by how you feel about macros - do you want to avoid magic and trickier error messages that you may get with a macro based framework, or are you happy to just write your business logic and leave all that to the framework.

### Where to start

For "generic" backend web development it's looks like Tokio is again a good default option.

## Books

For more detail on many aspects of Rust for the web and much more you might find the [survey of the available Rust books](/2023/06/18/rust-programming-books/) useful, which has a category specifically for web / API books.

## Further Resources

- [The Rust Workshop survey of the available Rust books](/2023/06/18/rust-programming-books/)- [blessed.rs has a curated crate list](https://blessed.rs/crates#section-networking-subsection-http-foundations)
- [lib.rs](https://lib.rs/crates) has useful stats on it, e.g. [the rocket crate page](https://lib.rs/crates/rocket) shows it's #5 (presumably by downloads)
  - "*Lightweight, opinionated, curated, unofficial alternative to crates.io*".
  - it has a [list of http crates](https://lib.rs/web-programming/http-server) - the top four are axum, actix, warp and rocket
- [Server framework benchmarks by techempower](https://www.techempower.com/benchmarks/#section=data-r22&hw=ph&test=plaintext)
- [Rust Digger](https://rust-digger.code-maven.com/) via the [Rustacean Station Podcast - Rust Digger](https://rustacean-station.org/episode/gabor-szabo/)
- [Rust web frameworks: development in 2023 ~ Yalantis](https://yalantis.com/blog/rust-web-frameworks/)
- Old article on async - much has changed since: <https://bryangilbert.com/post/code/rust/adventures-futures-tokio-rust/>
- Asking ["What's the current best practice for web dev in Rust? on reddit"](https://www.reddit.com/r/rust/comments/17dt9bo/whats_the_current_best_practice_for_web_dev_in/) flushed out some good advice and things to check out.

## Thanks

- Many thanks to David Haig for early review and significant contributions to this article.
- Many thanks to those on Reddit who responded with things I'd missed out.
