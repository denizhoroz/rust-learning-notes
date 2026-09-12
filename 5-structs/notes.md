# Structs

Structs act like any other programming language in Rust

```
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
```

When creating instances, only the entire instance can be mutable OR not

```
fn main() {
    let mut user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };

    user1.email = String::from("anotheremail@example.com");
}
```

This below example can be used to copy previous instance values to a newly created instance

```
fn main() {
    // --snip--

    let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
}
```

The methods for structs are made using `impl` (implements) function

Below is a example sage of a debugged struct with methods

```
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }

    fn peri(&self) -> u32 {
        self.width * 2 + self.height * 2
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels. Perimeter is {}",
        rect1.area(),
        rect1.peri(),
    );
}
```