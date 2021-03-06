# Transitioning your code to a new edition

Transitioning between editions is built around lints. Fundamentally, the
process works like this:

* Get your code compiling with no warnings.
* Opt in to the new edition.
* Fix any new warnings that may result.

Luckily, we've been working on a tool to help assist with this process,
`rustfix`. It can take suggestions from the compiler and automatically
re-write your code to comply with new features and idioms.

> `rustfix` is still quite young, and very much a work in development. But it works
> for the basics! We're working hard on making it better and more robust, but
> please bear with us for now.

## Installing rustfix

You can get `rustfix` from GitHub, and eventually, `crates.io`. Given that you're probably using Cargo,
you'll want to run this:

```shell
$ cargo install cargo-fix --git https://github.com/rust-lang-nursery/rustfix
```

And that's it!

## Prepare for the next edition

Before we talk about how to move to the new edition, a reminder:


* New editions might change the way you write Rust -- they add syntax,
  language, and library features but also remove others. For example,
  `async`/`await` is available in Rust 2018, but not Rust 2015.
* It's our intention that the migration to new editions is as smooth an experience
  as possible. It's considered a bug if it's difficult to upgrade your crate to
  a new edition. If you have a difficult time then a bug should be filed
  with either rustfix or Rust itself.

With that out of the way, let's get into it!

### The preview period

First, editions have a "preview" phase. This lets you try out the new edition
in nightly Rust. During the preview, there's an extra step you need to take
to opt in. At the time of writing, Rust 2018 is in its preview phase.

To do that, add this feature flag to your `lib.rs` or `main.rs`:

```rust
#![feature(rust_2018_preview)]
```

This will ensure that you're enabling all of the relevant features. Note that
during the time the preview is available, we may continue to add/enable new
features with this flag!

### Running rustfix

There are some lints that can help you prepare for the next edition, but
they're not currently turned on by default. `rustfix` has your back though!
To turn them on and have `rustfix` fix up your code, run this:

```shell
$ cargo +nightly fix --prepare-for 2018
```

This would turn on those lints, and fix up the project for the 2018 edition.
If there's something that `rustfix` doesn't know how to fix automatically yet,
the usual compiler warning will be printed; you'll need to fix those
manually. Do so until you get a run with no warnings.

## Commit to the next edition

Once you're happy with those changes, it's time to use the new edition.
Add this to your `Cargo.toml`:

```toml
cargo-features = ["edition"]

[package]
edition = '2018'
```

That `cargo-features` line should go at the very top; and `edition` goes into
the `[package]` section. As mentioned above, right now this is a nightly-only
feature of Cargo, so you need to enable it for things to work.

At this point, your project should compile with a regular old `cargo +nightly
build`. However, since you've said you're using the new edition, you may get
more warnings! Time to bust out `rustfix` again.

## Fix new warnings

To fix up these warnings, we can use `rustfix`:

```shell
$ cargo +nightly fix
```

This will try to fix up all of the new warnings. Congrats! You're done!