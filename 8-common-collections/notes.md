# Vectors

> Vectors allow you to store more than one value in a single data structure that puts all the values next to each other in memory. Vectors can only store values of the same type. They are useful when you have a list of items, such as the lines of text in a file or the prices of items in a shopping cart.

Declaration

```
    let v: Vec<i32> = Vec::new();
```

Macro

```
    let v = vec![1, 2, 3];
```

Adding values

```
    let mut v = Vec::new();

    v.push(5);
    v.push(6);
    v.push(7);
    v.push(8);
```

Reading values

```
    let v = vec![1, 2, 3, 4, 5];

    let third: &i32 = &v[2];
    println!("The third element is {third}");

    let third: Option<&i32> = v.get(2);
    match third {
        Some(third) => println!("The third element is {third}"),
        None => println!("There is no third element."),
    }
```

Iterating values

```
    let v = vec![100, 32, 57];
    for i in &v {
        println!("{i}");
    }
```

# Strings

> The `String` type, which is provided by Rust’s standard library rather than coded into the core language, is a growable, mutable, owned, UTF-8 encoded string type. When Rustaceans refer to “strings” in Rust, they might be referring to either the `String` or the string slice `&str` types, not just one of those types.

Creating 

```
    let mut s = String::new();
```

Converting

```
    let data = "initial contents";

    let s = data.to_string();

    // The method also works on a literal directly:
    let s = "initial contents".to_string();
```

Appending

```
    let mut s = String::from("foo");
    s.push_str("bar");
```

```
    let s1 = String::from("Hello, ");
    let s2 = String::from("world!");
    let s3 = s1 + &s2; // note s1 has been moved here and can no longer be used
```

Rust Strings doesn't support indexing

```
    // This won't work
    let s1 = String::from("hi");
    let h = s1[0];
 
```

Slicing

```
let hello = "Здравствуйте";

let s = &hello[0..4];
```

Iterating

```
for c in "Зд".chars() {
    println!("{c}");
}


for b in "Зд".bytes() {
    println!("{b}");
}

```

# Hash Maps

> The type `HashMap<K, V>` stores a mapping of keys of type `K` to values of type `V` using a _hashing function_, which determines how it places these keys and values into memory. Many programming languages support this kind of data structure, but they often use a different name, such as _hash_, _map_, _object_, _hash table_, _dictionary_, or _associative array_, just to name a few.

Creating hash map

```
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);
```

Accessing

```
    let team_name = String::from("Blue");
    let score = scores.get(&team_name).copied().unwrap_or(0);
```