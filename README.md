# rustfmt

[![MIT License](https://img.shields.io/badge/license-MIT-brightgreen)][mit]

> This repository contains Subtale's **opinionated** rustfmt configuration file ([`.rustfmt.toml`][config]).

## Usage

The [`.rustfmt.toml`][config] should be included in the root directory of your Rust project. This will ensure that rustfmt picks it up when formatting.

## Configuration options

Below you can find reasoning behind all of the configuration options present in Subtale's custom rustfmt configuration.

Some of these settings are set to their default values; this is intentional to ensure that they are kept as default.

### `unstable_features = true`

This is a required option because some of the below settings are considered "unstable" by rustfmt.

### `reorder_imports = true`

Ensures that imports and extern crate statements are sorted alphabetically (in groups).

```rs
use dolor;
use ipsum;
use lorem;
use sit;
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

[mit]: LICENSE
[config]: .rustfmt.toml
