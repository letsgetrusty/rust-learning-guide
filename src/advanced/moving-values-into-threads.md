# Moving Values into Threads

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/06WcsNPUNC8?si=m-6VuA6t_i11N2JC&amp;start=476" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Moving

```rust,editable,compile_fail
// Fix the code to make it compile.

use std::thread;

fn main() {
    let msg = "Hello from spawned thread".to_owned();
    let handle = thread::spawn(|| {
        println!("{msg}");
    });
    handle.join().unwrap();
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
use std::thread;

fn main() {
    let msg = "Hello from spawned thread".to_owned();
    let handle = thread::spawn(move || {
        println!("{msg}");
    });
    handle.join().unwrap();
}
  ```
</details>
