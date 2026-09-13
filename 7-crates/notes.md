# Packages and Crates

There are two types of crates: binary and library crates. 

*   Binary crates are executable programs that is compilable and can be ran on a CLI or a server.
*   Library crates doesn't have a `main` function that only carry functionality intended to be shared with multiple projects.

Cheat sheet for modules is found in `7.2. Control Scope and Privacy with Modules`

For example for a directory

```
backyard
├── Cargo.lock
├── Cargo.toml
└── src
    ├── garden
    │   └── vegetables.rs
    ├── garden.rs
    └── main.rs
```

The import for `Asparagus` class would look like this

```
// This imports the Asparagus class
use crate::garden::vegetables::Asparagus;

// This makes the compiler include the code in garden.rs
pub mod garden;

fn main() {
    let plant = Asparagus {};
    println!("I'm growing {plant:?}!");
}
```

In Rust modules are private by default. To include them we have to make them public