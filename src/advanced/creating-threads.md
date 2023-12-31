# Creating Threads

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/06WcsNPUNC8?si=ZaQGPPI2VJDfdM5A&amp;start=81" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Joining 1

```rust,editable,compile_fail
// Modify the code to ensure that main thread waits for other threads to finish.

use std::thread;

fn main() {
    // print hello 10 times from spawned thread
    let handle = thread::spawn(|| {
        for i in 0..10 {
            println!("{i} Hello from spawned thread!")
        }
    });

    // print hello 10 times from main thread
    for i in 0..10 {
        println!("{i} Hello from main thread!");
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
use std::thread;

fn main() {
    // print hello 10 times from spawned thread
    let handle = thread::spawn(|| {
        for i in 0..10 {
            println!("{i} Hello from spawned thread!")
        }
    });

    // print hello 10 times from main thread
    for i in 0..10 {
        println!("{i} Hello from main thread!");
    }
    handle.join().unwrap();
}
  ```
</details>

#### Joining 2

```rust,editable,compile_fail
// Fix the code to make sure that sum is calculated correctly.

use std::{
    ops::Range,
    thread::{self, JoinHandle},
};

fn summation_thread(range: Range<u64>) -> JoinHandle<u64> {
    thread::spawn(|| {
        let mut sum = 0;
        for num in range {
            sum += num;
        }
        sum
    })
}

// calculate the sum of numbers 1..=40_00_000 using four threads
fn main() {
    let handle1 = summation_thread(1..10_00_000);
    let handle2 = summation_thread(10_00_000..20_00_000);
    let handle3 = summation_thread(20_00_000..30_00_000);

    let mut sum = 0u64;
    for num in 30_00_000..=40_00_000 {
        sum += num;
    }
    println!("Sum of numbers 1..=40_00_000: {sum}");
    assert_eq!(sum, 8000002000000);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
use std::{
    ops::Range,
    thread::{self, JoinHandle},
};

fn summation_thread(range: Range<u64>) -> JoinHandle<u64> {
    thread::spawn(|| {
        let mut sum = 0;
        for num in range {
            sum += num;
        }
        sum
    })
}

// calculate the sum of numbers 1..=40_00_000 using four threads
fn main() {
    let handle1 = summation_thread(1..10_00_000);
    let handle2 = summation_thread(10_00_000..20_00_000);
    let handle3 = summation_thread(20_00_000..30_00_000);

    let mut sum = 0u64;
    for num in 30_00_000..=40_00_000 {
        sum += num;
    }
    sum += handle1.join().unwrap();
    sum += handle2.join().unwrap();
    sum += handle3.join().unwrap();
    println!("Sum of numbers 1..=40_00_000: {sum}");
    assert_eq!(sum, 8000002000000);
}
  ```
</details>

#### Sleeping

```rust,editable,compile_fail
// When main thread terminates, all the spawned threads also get terminated.
// Make the main thread sleep for half a second before terminating, to ensure that spawned thread gets enough time to complete.
// Do not join the spawned thread.

use std::thread;

fn main() {
    thread::spawn(|| {
        println!("Count down from 10 to 1 🚀");
        for num in (1..=10).rev() {
            println!("Count down: {num}!");
        }
    });
    println!("Count up from 1 to 10...");
    for num in 1..=10 {
        println!("Count up: {num}");
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
use std::thread;
use std::time::Duration;

fn main() {
    thread::spawn(|| {
        println!("Count down from 10 to 1 🚀");
        for num in (1..=10).rev() {
            println!("Count down: {num}!");
        }
    });
    println!("Count up from 1 to 10...");
    for num in 1..=10 {
        println!("Count up: {num}");
    }
    thread::sleep(Duration::from_millis(500));
}
  ```
</details>
