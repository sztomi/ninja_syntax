# ninja_syntax

This is a port of ninja_syntax.py to Rust. It allows you to easily emit ninja build files from
Rust with a syntax that is pretty similar to the official python module from the ninja repo.

## Example

```rust
use ninja_syntax::*;
use std::path::Path;

fn main() {
  let mut nw = NinjaWriter(Path::new("build.ninja"));
  nw.comment("Hello this is a comment");
  nw.newline();

  let rule = NinjaRule::new("cc", "cc $in -o $out");
  nw.rule(&rule);

  let mut build = NinjaBuild::new(&["test.o"], "cc");
  build.inputs(&["test.c"]);
  nw.build(&build);

  // write the file to disk
  nw.close().unwrap();
}
```

## Acknowledgements

Originally written by [Tobias Hieta][1], forked and licensed under MIT (with permission from the
author) by Tamás Szelei. Most of the code is the same, but slight changes were made to make it more
ergonomic and to use `textwrap` instead of a custom implementation of word wrapping. Finally, to
apply the *correct* indentation (which is two spaces). Originally part of [sa_ninja_gen][2], now 
being published as a separate crate.

[1]: https://github.com/tru/ninja-syntax
[2]: https://github.com/sztomi/sa_ninja_gen/tree/main
