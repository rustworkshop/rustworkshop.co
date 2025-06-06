---
title: Rust CLI tool templates
layout: post
author: Tim Abell
---

Creating many small CLI (Command Line Interface) tools in rust is a great thing to do. The unix way is to build many small single-purpose tools and then allow users to chain & script them together in useful ways so being able to rapidly create the tools we need in rust is of great benefit.

Having created a quite a few CLI tools from scratch in my time, and looking to create another for a small task I went hunting for existing Rust CLI templates. It turns out there's quite a few, so this post captures that research. Contributions and updates welcome via PR.

Whether you're going to create your own CLI from scratch or you want a ready-to-go template to build from, there is lots of inspiration, ideas and great implementations to be found in the list below. Take a look around and let me know what you build!

## Top picks ⭐

- [rust-starter/rust-starter](https://github.com/rust-starter/rust-starter)
  - Last commit: May 2025 (last month)
  - MIT license
  - clap, [failure crate](https://docs.rs/failure/), config-rs, async log with slog, static binaries (rust-musl) (avoids glibc compatibility problems), [better_panic](https://docs.rs/better-panic/latest/better_panic/) (debug only), [human_panic](https://github.com/rust-cli/human-panic) (production)
  - logging
  - tests
  - shell completion (bash/fish/zsh)
  - github config
    - build
    - release
    - cross-platform
    - lint, codecov, test, audit
    - issue template
    - contributions
  - Also available as [rust-starter/rust-starter-generate](https://github.com/rust-starter/rust-starter-generate) cargo-generate template
  - gitignore
  - cargo graph png/dot
  - docker
  - [just](https://github.com/casey/just) (like make/npm)
  - [docs site](https://rust-starter.github.io/)
  - complete example cli
    - clap commands for config, completion, error, hazard
  - cargo dev/release etc profile config
  - Why I like this one: incredibly complete
- [rusty-ferris-club/rust-starter](https://github.com/rusty-ferris-club/rust-starter)
  - Last commit: Dec 2023
  - Apache 2 license
  - install sh / ps1 scripts
  - xtask
  - clone or use backpack (below) to use
  - three different example CLIs
    - flat
    - subcommands
    - lib
  - clap
  - [insta](https://insta.rs/) snapshot testing
  - [xtask](https://github.com/matklad/cargo-xtask) (like make/npm but for rust)
  - github config
    - issue template
    - build - check, test, coverage, fmt, clippy
    - release - multi-platform, upload to github releases
  - 4 projects nested under main repo - xtask plus 3 cli examples - uses [cargo "workspace" feature](https://doc.rust-lang.org/book/ch14-03-cargo-workspaces.html) to tie them together
  - Why I like this one: very complete example, interesting tooling, and the three styles of app, however lack of recent contributions is a bit of a worry (out of date dependencies)
- [danielschemmel/rust-cli-template](https://github.com/danielschemmel/rust-cli-template)
  - Last commit: May 2025 (last week)
  - [Missing license file](https://github.com/danielschemmel/rust-cli-template/issues/7) - this is a risk for any public re-use
  - docker
  - rustfmt config
  - editorconfig
  - gitignore
  - devcontainer config
  - github actions config
    - cargo-outdated
    - build/test/clippy
    - no release config
  - non-trivial example src
  - no lib folder
  - clap, anyhow, [pretty_assertions](https://docs.rs/pretty_assertions/latest/pretty_assertions/), tokio
  - ctrl-c handling
  - trace logging
  - Why I like this one: It's simple, but has the essential clap & github setup, however the lack of license bothers me.
- [shellshape/rust-cli-template](https://github.com/shellshape/rust-cli-template)
  - Last commit: May 2025 (last month)
  - MIT license
  - cargo-generate template (see below)
  - clap, [anyhow](https://crates.io/crates/anyhow), [figment](https://crates.io/crates/figment), [dirs](https://crates.io/crates/dirs)
  - everything in a template/ subfolder
  - config
  - liquid templated cargo toml
  - github config
    - cross platform - including risc, freebsd etc.
      - [houseabsolute/actions-rust-cross](https://github.com/houseabsolute/actions-rust-cross)
    - build
    - create github release
  - uses [rhai](https://rhai.rs/) ("scripting language and evaluation engine that integrates tightly with Rust")
  - src with main, config parser and commands module
  - Why I like this one: It's simple, the main/commands are well organised, it's MIT licensed, it has github configured

## Other templates

- [alfredodeza/rust-cli-example](https://github.com/alfredodeza/rust-cli-example)
  - Last commit: Jan 2024
  - MIT license
  - Part of a uni course on python/rust
  - devcontainer config
  - [clap](https://crates.io/crates/clap)
  - basic lib/main example
- [boilerplato/rust-cli-template](https://github.com/boilerplato/rust-cli-template)
  - Last commit: April 2020
  - MIT license
  - boilerplato template (see below)
  - clap, colored
  - example in src that implements a file size display tool
  - custom error types
- [Byron/cli-template-rs](https://github.com/Byron/cli-template-rs)
  - Missing license file - this is a risk for any public re-use
  - makefile
  - snapshot tests of output
  - github ci
  - clap/anyhow/thiserror
  - basic lib/main example
  - Last commit: Sep 2023
- [gitpod-samples/template-rust-cli](https://github.com/gitpod-samples/template-rust-cli)- specific to Gitpod code hosting
  - Last commit: Nov 2022
  - MIT license
  - based on rust-starter (below), customized for [gitpod](https://www.gitpod.io/) cloud development environments
- [Keats/rust-cli-template](https://github.com/Keats/rust-cli-template)
  - Last commit: Nov 2018
  - MIT license
  - clap
  - shell completions
  - coloured output
  - travis/appveyor config with releases to github
  - keats/kickstart template (see below) - as such not a compilable project without running through template tool first, placeholders in files and paths
  - Some custom error types
  - src broken up into a few files
  - editorconfig
  - stub changelog
- [kbknapp/rust-cli-template](https://github.com/kbknapp/rust-cli-template)
  - Last commit: Jul 2021
  - Apache 2 / MIT dual license
  - "A personal template for CLI projects in Rust" - i.e. not particularly intended for use by others
  - templated values in files - no mention of which templating tool is being used
- [tgm-templates/rust-cli](https://github.com/tgm-templates/rust-cli)
  - Last commit: April 2024
  - MIT license
  - just build (like make/npm)
  - clap
  - shell completion (zsh/omz/fish/bash)
  - minimal repo, not a lot of extras
  - cargo release profile config
  - src with main.rs and app.rs, nothing else
  - template json
  - no github config
- [tobx/rust-cli-app-template](https://github.com/tobx/rust-cli-app-template)
  - Last commit: April 2025 (two months ago)
  - MIT license
  - clap, config, owo-colors, thiserror
  - gitignore
  - editorconfig
  - vscode config
  - src with several example files and modules
  - no github config

## How to choose

Choosing one is partly a matter of taste and fit for the job at hand - choosing a fully featured template or something simpler for a simple task.

Avoid unmaintained templates.

Consider license implications for building on top of a template, lack of a license adds a risk.

It's hard to say whether it's better to take the kitchen-sink template, or one withe bare-minimum that you need and build on that, or to build your own personal template from one of them or from scratch. Horses for courses, and engineering tradeoffs.

Apply your brain here, and consider the context of what you are trying to achieve and in what context. There's lots here to build on.

Good luck and enjoy! There's something especially pleasing about a great command line tool, and rust and clap make them fast and ergonomic.

## Project creation tools

- [cargo-generate](https://github.com/cargo-generate/cargo-generate)
- [boilerplato/boilerplato](https://github.com/boilerplato/boilerplato)
- [humblepenguinn/tmplt](https://github.com/humblepenguinn/tmplt)
- [Keats/kickstart](https://github.com/Keats/kickstart)
- [rusty-ferris-club/backpack](https://github.com/rusty-ferris-club/backpack)

## Existing Rust Workshop CLI tools

These are the existing CLI tools that demonstrate some of the preferred approaches

- [gitopolis](https://github.com/rustworkshop/gitopolis) (multi-repo local git management)
  - automatic upgrade sh scripts
  - asdf-vm rust management
  - clap (of course)
  - end-to-end testing of actual binary
  - github config
    - dependabot
    - automatic releases from pushed tags
    - automated release notes from [conventional commits](https://www.conventionalcommits.org/) with [git-cliff](https://git-cliff.org/)
- [paths2html](https://github.com/rustworkshop/paths2html) (converts lists of paths to collapsible interactive html bullet list)
  - a unix style tool that has no arguments, it just takes a stream of text on stdin (file paths) and transforms it to a different stream of text on stdout (html)
  - no clap needed, because it doesn't need any params, it just flows data - a reminder that a one-size template wouldn't be a perfect fit for everything

## Related templates

- [rust-github/template](https://github.com/rust-github/template) - rust github config template
- [epage/_rust](https://github.com/epage/_rust) - generic rust template
