# Ownership Rules

> *   Each value in Rust has an _owner_.
> *   There can only be one owner at a time.
> *   When the owner goes out of scope, the value will be dropped.

See `Understanding Ownership` in

```
rustup doc --book
```

> With the `String` type, in order to support a mutable, growable piece of text, we need to allocate an amount of memory on the heap, unknown at compile time, to hold the contents. This means:
> 
> *   The memory must be requested from the memory allocator at runtime.
> *   We need a way of returning this memory to the allocator when we’re done with our `String`.
> 
> That first part is done by us: When we call `String::from`, its implementation requests the memory it needs. This is pretty much universal in programming languages.

```
    {
        let s = String::from("hello"); // s is valid from this point forward

        // do stuff with s
    }                                  // this scope is now over, and s is no
                                       // longer valid
```

> There is a natural point at which we can return the memory our `String` needs to the allocator: when `s` goes out of scope. When a variable goes out of scope, Rust calls a special function for us. This function is called `drop`, and it’s where the author of `String` can put the code to return the memory. Rust calls `drop` automatically at the closing curly bracket.x

# Memory and Allocation

The variables below push two 5's onto the stack

```
    let x = 5;
    let y = x;
```

However for example doing the same thing with the String object does not work the same

```
    let s1 = String::from("hello");
    let s2 = s1;
```

The second string does not copy the String content, instead `s1` and `s2` use the same content address 

And when `s1` goes out of scope, the entire data for `s1` including the String content is gone as well. When `s1` and `s2` were to be dropped (go out of scope) simultaneously, they will both try to free the same memory. This is known as a _double free error._ To solve this, Rust ensures that the old string is dropped when the new string is declared. Therefore the expression below won't work

```
    let s1 = String::from("hello");
    let s2 = s1;

    

    // s1 is dropped, this won't work
    println!("{s1}, world!");
```

> In addition, there’s a design choice that’s implied by this: Rust will never automatically create “deep” copies of your data. Therefore, any _automatic_ copying can be assumed to be inexpensive in terms of runtime performance.

# Deep Copy with `.clone`

This usage below actually creates a copy instead of moving the old variable

```
    let s1 = String::from("hello");
    let s2 = s1.clone();

    println!("s1 = {s1}, s2 = {s2}");
```

# Ownership and Functions

```
fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here

    let x = 5;                      // x comes into scope

    makes_copy(x);                  // Because i32 implements the Copy trait,
                                    // x does NOT move into the function,
                                    // so it's okay to use x afterward.

} // Here, x goes out of scope, then s. However, because s's value was moved,
  // nothing special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{some_string}");
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{some_integer}");
} // Here, some_integer goes out of scope. Nothing special happens.
```

> If we tried to use `s` after the call to `takes_ownership`, Rust would throw a compile-time error. These static checks protect us from mistakes. Try adding code to `main` that uses `s` and `x` to see where you can use them and where the ownership rules prevent you from doing so.

# Return Values and Scope

```
fn main() {
    let s1 = gives_ownership();        // gives_ownership moves its return
                                       // value into s1

    let s2 = String::from("hello");    // s2 comes into scope

    let s3 = takes_and_gives_back(s2); // s2 is moved into
                                       // takes_and_gives_back, which also
                                       // moves its return value into s3
} // Here, s3 goes out of scope and is dropped. s2 was moved, so nothing
  // happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String {       // gives_ownership will move its
                                       // return value into the function
                                       // that calls it

    let some_string = String::from("yours"); // some_string comes into scope

    some_string                        // some_string is returned and
                                       // moves out to the calling
                                       // function
}

// This function takes a String and returns a String.
fn takes_and_gives_back(a_string: String) -> String {
    // a_string comes into
    // scope

    a_string  // a_string is returned and moves out to the calling function
}
```

# References and Borrowing

Using referencing `&` to not take ownership of an instance is called _borrowing_

```
fn main() {
    let s1 = String::from("hello");

    

    // Sending the address value (reference) of s1
    let len = calculate_length(&s1);

    println!("The length of '{s1}' is {len}.");
}


// &String takes an address value instead of the instance itself
fn calculate_length(s: &String) -> usize {
    s.len()
}
```

References are immutable like variables by default so this won't work

```
fn main() {
    let s = String::from("hello");

    change(&s);
}

fn change(some_string: &String) {
    // Cannot change immutable references
    some_string.push_str(", world");
}
```

So it is necessary to pass mutable references by using `&mut`

```
change(&mut s);
```

 Also we cannot create two mutable references of the same instance

```
    let mut s = String::from("hello");

    let r1 = &mut s;
    
    
// This one creates an error
    let r2 = &mut s;

    println!("{r1}, {r2}");
```

However this will work because there are only one mutable reference

```
    let mut s = String::from("hello");

    let r1 = &s; // no problem
    let r2 = &s; // no problem
    println!("{r1} and {r2}");
    // Variables r1 and r2 will not be used after this point.

    let r3 = &mut s; // no problem
    println!("{r3}");
```

# Dangling

If you try to return a reference of an instance that is created inside a function (dangling pointer) this will not work in Rust. The instance is immediately dropped after the function stops so there won't be any value to point towards

```
fn main() {
    let reference_to_nothing = dangle();
}

fn dangle() -> &String {
    let s = String::from("hello");

    

    // Cannot return reference, this won't work
    &s
}
```

Return the instance itself instead of the reference to pass ownership

```
fn no_dangle() -> String {
    let s = String::from("hello");

    s
}
```

# Slice Type

> Here’s a small programming problem: Write a function that takes a string of words separated by spaces and returns the first word it finds in that string. If the function doesn’t find a space in the string, the whole string must be one word, so the entire string should be returned.

```
// We don't need to pass ownership
fn first_word(s: &String) -> usize {


    // To check spaces we convert String to an array of bytes
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
        
        
    // Returns the position where space is found 
            return i;
        }
    }

    

    // When no space is found, the word end position is returned
    s.len()
}
```

```
fn main() {
    let mut s = String::from("hello world");

    let word = first_word(&s); // word will get the value 5

    s.clear(); // this empties the String, making it equal to ""

    // word still has the value 5 here, but s no longer has any content that we
    // could meaningfully use with the value 5, so word is now totally invalid!
}
```

This is not very ideal when we are slicing more than one element

# String Slices

```
    let s = String::from("hello world");

    let hello = &s[0..5];
    let world = &s[6..11];
```

Improved first word function

```
fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}
```

If immutable reference is created from an mutable reference, the mutable reference must remain when the immutable reference is used

```
fn main() {
    let mut s = String::from("hello world");

    let word = first_word(&s);

    

    // This causes the error
    s.clear(); // error!

    

    // The word immutable variable is connected to mutable s
    println!("the first word is: {word}");
} 
```