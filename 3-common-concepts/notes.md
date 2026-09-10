# Mutability

Cannot change immutable variables

```
let x = 5;
println!(x)

// we can't change the value of x like this here
x = 6;
```

Make sure its mutable 

```
let mut x = 5;
x = 6;
```

`const` constants cannot be made mutable, they are always immutable. And they can be declared in any scope, including global scope

```
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

# Shadowing

_Shadowing_ is used to convert or change variables

```
fn main() {
    let x = 5;

    

    // Here x is shadowed and incremented by 1
    let x = x + 1;

    {
        // Here x is shadowed and multiplied by 2
        let x = x * 2;
    }
}
```

```
    // This is a String
    let spaces = "   ";
    
    
// This is an int
    let spaces = spaces.len();
```

If we try to change this with mutability we will get a compile error because we are not allowed to mutate a variable's type

```
    // This won't work
    let mut spaces = "   ";
    spaces = spaces.len();
```

# Data Types, Functions & More

The data type can be assigned on variable assignment with `:` keyword

```
// Here the guess variable is assigned to be a unsigned 32-bit integer
let guess: u32 
```

More information about data types and more can be found in `Common Programming Concepts`

```
rustup doc --book
```