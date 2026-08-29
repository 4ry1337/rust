## Control flow

```rust
// if is an expression:
let status = if number > 0 {
    "positive"
} else if number < 0 {
    "negative"
} else {
    "zero"
}
```

```rust
loop { // infinite loop
    println!("again!");
    break; // or until explicitly exit
}

// break is an expression too:
let mut counter = 0;
let result = loop {
    counter += 1;
    if counter == 10 {
        break counter * 2;
    }
};

// loops can be labeled:
let mut count = 0;
'counting_up: loop {
    let mut remaining = 10;
    loop {
        if remaining == 9 {
            break;
        }
        // break outer loop
        if count == 2 {
            break 'counting_up;
        }
        remaining -= 1;
    }
    count += 1;
}

// while and for loops exist too
```
