# Enums

Enums are used like any other language

```
enum IpAddrKind {
    V4,
    V6,
}
```

Enum values are initialized like this

```
    let four = IpAddrKind::V4;
    let six = IpAddrKind::V6;
```

Data can be put into an enum

```
    enum IpAddr {
        V4(String),
        V6(String),
    }

    let home = IpAddr::V4(String::from("127.0.0.1"));

    let loopback = IpAddr::V6(String::from("::1"));
```

# Option Enum

Rust doesn't have a null value to avoid errors that is found in other languages, however it does have a concept that can tell if a value is absent.

```
enum Option<T> {
    None,
    Some(T),
}
```

It's used commonly so we don't have to declare it everytime

```
    let some_number = Some(5);
    let some_char = Some('e');

    let absent_number: Option<i32> = None;
```

Now variables with `Option` type can have `None` as a null value instead of a real value

`Option` and other types cannot be added, subtracted etc.

```
    let x: i8 = 5;
    let y: Option<i8> = Some(5);

    let sum = x + y;
```

# Match

Match can be used with enums in Rust

```
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

`match` cases should cover all posibilities

```
    fn plus_one(x: Option<i32>) -> Option<i32> {
        match x {
            None => None,
            Some(i) => Some(i + 1),
        }
    }

    let five = Some(5);
    let six = plus_one(five);
    let none = plus_one(None);
```

In this example `None` is not covered which causes the code to not work

```
    fn plus_one(x: Option<i32>) -> Option<i32> {
        match x {
        	// None is not covered here
            Some(i) => Some(i + 1),
        }
    }
```

> Rust also has a pattern we can use when we want a catch-all but don’t want to _use_ the value in the catch-all pattern: `_` is a special pattern that matches any value and does not bind to that value. This tells Rust we aren’t going to use the value, so Rust won’t warn us about an unused variable.

```
    let dice_roll = 9;
    match dice_roll {
        3 => add_fancy_hat(),
        7 => remove_fancy_hat(),
        _ => reroll(),
    }

    fn add_fancy_hat() {}
    fn remove_fancy_hat() {}
    fn reroll() {}
```

Adding `None` condition every time can be annoying when using match statements every time.

```
    let config_max = Some(3u8);
    match config_max {
        Some(max) => println!("The maximum is configured to be {max}"),
        _ => (),
    }
```

So we can use `if` statements instead to handle only one condition

```
    let config_max = Some(3u8);
    if let Some(max) = config_max {
        println!("The maximum is configured to be {max}");
    }
```