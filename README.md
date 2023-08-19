# rustfmt

[![OSS by Subtale](https://img.shields.io/badge/oss_by-subtale-white?style=flat-square&labelColor=%2389216B&color=%23DA4453)][oss]
[![MIT License](https://img.shields.io/badge/license-MIT-brightgreen?style=flat-square&labelColor=%2389216B&color=%23DA4453)][mit]

> This repository contains Subtale's **opinionated** rustfmt configuration file ([`.rustfmt.toml`][config]).

## Usage

The [`.rustfmt.toml`][config] should be included in the root directory of your Rust project. This will ensure that rustfmt picks it up when formatting.

## Configuration options

Below you can find the reasoning behind all of the configuration options present in Subtale's custom rustfmt configuration.

Some of these settings are set to their default values; this is intentional to ensure that they are kept as default.

### `unstable_features = true`

This is a required option because some of the below settings are considered "unstable" by rustfmt.

### `combine_control_expr = false`

Separates control expression from their function calls:

```rs
fn example() {
    // If
    foo!(
        if x {
            foo();
        } else {
            bar();
        }
    );

    // IfLet
    foo!(
        if let Some(..) = x {
            foo();
        } else {
            bar();
        }
    );
}
```

### `condense_wildcard_suffixes = true`

Replaces consecutive underscore variables with a single `..` within tuple patterns:

```rs
let (lorem, ipsum, _, _) = (1, 2, 3, 4);
// becomes
let (lorem, ipsum, ..) = (1, 2, 3, 4);
```

### `fn_single_line = true`

Puts single-expression functions on a single line:

```rs
fn lorem() -> usize { 42 }
```

### `format_code_in_doc_comments = true`

Formats code snippets in doc comments.

### `format_macro_matchers = true`

Formats metavariables in macro declarations:

```rs
macro_rules! foo {
    ($a:ident : $b:ty) => {
        $a(42): $b;
    };
    ($a:ident $b:ident $c:ident) => {
        $a = $b + $c;
    };
}
```

### `group_imports = "StdExternalCrate"`

Reorganizes imports into three distinct groups:

1. `std`, `core`, and `alloc`
2. external crates
3. `self`, `super`, and `crate` imports

```rs
use alloc::alloc::Layout;
use core::f32;
use std::sync::Arc;

use broker::database::PooledConnection;
use chrono::Utc;
use juniper::{FieldError, FieldResult};
use uuid::Uuid;

use super::schema::{Context, Payload};
use super::update::convert_publish_payload;
use crate::models::Event;
```

### `hex_literal_case = "Upper"`

Forces uppercase hex literals.

### `imports_granularity = "Crate"`

Merges imports from the same crate into a single `use` statement.

```rs
use foo::{
    a, b,
    b::{f, g},
    c,
    d::e,
};
use qux::{h, i};
```

### `imports_layout = "HorizontalVertical"`

Forces the style of items inside an imports block.

```rs
use foo::{xxxxxxxxxxxxxxxxxx, yyyyyyyyyyyyyyyyyy, zzzzzzzzzzzzzzzzzz};

use foo::{
    aaaaaaaaaaaaaaaaaa,
    bbbbbbbbbbbbbbbbbb,
    cccccccccccccccccc,
    dddddddddddddddddd,
    eeeeeeeeeeeeeeeeee,
    ffffffffffffffffff,
};
```

### `match_block_trailing_comma = true`

Puts a trailing comma after a block-based match arm:

```rs
match lorem {
    Lorem::Ipsum => {
        println!("ipsum");
    },
    Lorem::Dolor => println!("dolor"),
}
```

### `normalize_comments = true`

Converts `/* */` comments to `//` comments where possible.

### `normalize_doc_attributes = true`

Converts `#[!doc = "..." ]` attributes to `///` comments where possible.

### `reorder_impl_items = true`

Reorders impl items. `type` and `const` are grouped together, then macros and functions:

```rs
impl Iterator for Dummy {
    fn next(&mut self) -> Option<Self::Item> {
        None
    }

    type Item = i32;
}
// becomes
impl Iterator for Dummy {
    type Item = i32;

    fn next(&mut self) -> Option<Self::Item> {
        None
    }
}
```

### `reorder_imports = true`

Ensures that imports and extern crate statements are sorted alphabetically (in groups).

```rs
use dolor;
use ipsum;
use lorem;
use sit;
```

### `use_field_init_shorthand = true`

Uses the field init shorthand where possible:

```rs
fn main() {
    let x = 1;
    let y = 2;
    let z = 3;
    let a = Foo { x: x, y: y, z: z };
}
// becomes
fn main() {
    let x = 1;
    let y = 2;
    let z = 3;
    let a = Foo { x, y, z };
}
```

### `use_try_shorthand = true`

Replaces use of the `try!` macro with the `?` shorthand:

```rs
try!(ipsum.map(|dolor| dolor.sit()));
// becomes
ipsum.map(|dolor| dolor.sit())?;
```

### `wrap_comments = true`

Breaks comments to fit within the maximum line width (80 characters).

[oss]: https://oss.subtale.com
[mit]: LICENSE
[config]: .rustfmt.toml
