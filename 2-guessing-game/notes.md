# Guessing Game

Import packages

```
// from std import io 
use std::io
```

Declare variable

```
let var = 5;

// Mutable (changable) variable
let mut var = 2;

// From a String object create new instance and assign to guess variable 
let mut guess = String::new();
```

Use `read_line()` function from `io::stdin()` instance to get input from user. Store it in `guess` variable's address.

```
io::stdin()
    .read_line(&mut guess)
```

Note:

```
&stuff <- indicates the stuff's reference

&guess <- this is guess variable's reference 

References are immutable therefore we do

&mut guess <- mutable
```

Use `.expect()` for error handling.

```
io::stdin()
    .read_line(&mut guess)
    .expect("Failed to read line")
```

read\_line() returns a `Result` (enum). It can be a `Ok` or `Err` 

Print values with placeholders

```
// {guess} writes the variable there
println!("You guessed: {guess}")
println!("You guessed: {}", guess)
```

# Generating a Secret Number

Change dependencies to include packages

```
[dependencies]
rand = "0.8.5"
```

Then build to download packages

```
cargo build
```

Generate random numbers with `use rand::Rng`

```
use rand::Rng;

...
let secret_number = rand::thread_rng().gen_range(1..=100);
```

# Ordering Library

Use `Ordering` library to compare values

```
// import library
use std::cmp::Ordering;

// usage
match guess.cmp(&secret_number) {
    Ordering::Less => println!("Too small!"),
    Ordering::Greater => println!("Too big!"),
    Ordering::Equal => println!("You win!"),
}
```

Converting `guess` as an integer so the program compiles

Used _shadowing_ to convert a variable to a different data type

```
let guess: u32
```

`.trim()` is used to remove whitespaces, `.parse()` is used to convert String to another type

```
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```

# Allowing Multiple Guesses

`loop { }` is used to create an infinite loop like `while True`

```
    loop {
        println!("Please input your guess.");

        // --snip--

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => println!("You win!"),
        }
    }
```

Quit the loop when guessed correctly

```
        // --snip--

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

# Handling Invalid Input

Right now the game crashes when a user types a non-number

Catch errors to handle these problems

```
        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };
```