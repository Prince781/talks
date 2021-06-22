# Improving App Development in Vala

## What is Vala
- Created as a GNOME subproject in 2006
- Compiles to C, then to native code (with your favorite C compiler)
- Initial main ideas:
  - obviates writing many lines of boilerplate GObject C code
  - introduces high-level programming features
  - takes inspiration from C#
- Used everywhere by elementaryOS
- Also used extensively by GNOME

## The app development process
- Have an idea
- Pick a language
- Pick a framework

## The right language
- Good tooling makes the biggest difference
- The most useful tool is a language server

## The Vala Language Server

### Brief history
- Started development in mid-2017
- Hiatus
- Development picked up in early 2018
- Another hiatus
- Development signficantly picked up again since late 2019

### Precursors
- Valama IDE
- GNOME-Builder's vala-pack

### The Language Server Protocol
- Allows you to write _one_ language server for them all (editors)

### Implementing a language server

#### Compiler challenges
- Memory leaks in the compiler
- Syntax error recovery

#### User interaction
- Fast response
  - Delay context updates while the user is typing
  - Use backwards parser to extract expressions for rapid completion
  - Off-main-thread compilation or incremental recompilation?
    - Threaded compilation increases memory requirements but is faster
    - Incremental recompilation has significantly less memory requirements but still may not be responsive enough

#### Project management
- Integration with Meson build
  - Working around bugs and missing features
- Autotools is too complicated
- CMake is possible but scarcely used
- Handling other cases: single-file and script

#### Other things a language server does
- Refactoring operations
- Navigation (goto-X, show references)

## A language is an ecosystem
- Good tooling
  - IDEs/plugins
  - language servers
  - build system
  - static analyzers
  - linters
  - formatters
  - templates
  - better debugging
  - package manager
- Website + good documentation
- Cross-platform support
- Ergonomic language features

### How to make Vala better
- A year and a half ago, writing code in Vala was a bare experience.
- Since then things have changed, but there's still much room for improvement.
- I will now discuss the state of the Vala developer experience and identify areas that need improvement.

#### Code intelligence (quasi-tutorial)
- Visual Studio Code (vala)
- vim8/neovim (coc.nvim, nvim-lspconfig)
- GNOME Builder (vala)
- Kate (builtin)
- Emacs (emacs-lsp)

#### Build system
- Vala has Meson and is a first-class citizen there
- VLS uses Meson to understand your project
- What's needed: fixing bugs and improving Windows support

#### Static analysis
- It would be nice to have something like Clang's `scan-build` or GCC's `-fanalyzer` for Vala
- Requires significant changes to compiler (to be discussed later)

#### Linting and formatting
- In the vein of static analysis
- Give useful hints (fixups) that are out-of-scope for the compiler
  - like rust-clippy
- `vala-lint` is more of just a formatter

#### Templating
- We need a way to quickly setup a new Vala project
- C# has `dotnet new`
- Rust has `cargo new`
  - https://internals.rust-lang.org/t/pre-rfc-cargo-new-templates-v2/12089
- Vala now has `valdo new` (https://github.com/prince781/valdo)
  - Send me template PRs!

#### Better debugging
- Hovering over symbols doesn't work in many cases
- Get compiler to emit better source maps
- Write GDB plugin?

#### Dependency management
- Rust/JS/Go/C# - dependencies are per-project, statically linked in (or bundled)
- Vala - dependencies are system-wide, dynamically linked in
- How to bridge the gap?

#### Website
- valadoc.org is very good, but it could be better
  - it's mostly an API browser
  - design could take a page from Rust, V, gi-docgen
  - could we have docs on best practices, use of tooling, a la "effective Vala"?
- To increase Vala promotion, we need a real website
  - current site: https://live.gnome.org/Projects/Vala
  - better: https://vala-lang.org
    - help wanted: https://github.com/nahuelwexd/vala-website

#### Cross-platform support
- Building Vala apps on Windows isn't as easy as it is on *nix
  - `valac -C` works but `valac <file>` does not
  - Meson support is OK

#### Improving the language
- 