---
title: Web Frontends for Rust Programmers
layout: post
author: Tim Abell
---

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
