# Propagating Errors

<div style="display: flex; justify-content: center;">
    <iframe class="youtube-video" src="https://www.youtube.com/embed/wM6o70NAWUI?si=W5Mh47GWaOz11f1M&amp;start=507" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
</div>

## Exercises

#### Propagating errors 1

```rust,editable,compile_fail
// Fix the evaluate method by propagating the error.

// This enum can represent any mathematical expression
enum Expression {
    Val(i32),
    Add(Box<Expression>, Box<Expression>),
    Sub(Box<Expression>, Box<Expression>),
    Mul(Box<Expression>, Box<Expression>),
    Div(Box<Expression>, Box<Expression>),
}
use {Expression::Add, Expression::Div, Expression::Mul, Expression::Sub, Expression::Val};
impl Expression {
    fn evaluate(&self) -> Result<i32, String> {
        match self {
            Val(val) => Ok(*val),
            Add(exp1, exp2) => {
                let val1 = exp1.evaluate();
                let val2 = exp2.evaluate();
                Ok(val1 + val2)
            }
            Sub(exp1, exp2) => {
                let val1 = exp1.evaluate();
                let val2 = exp2.evaluate();
                Ok(val1 - val2)
            }
            Mul(exp1, exp2) => {
                let val1 = exp1.evaluate();
                let val2 = exp2.evaluate();
                Ok(val1 * val2)
            }
            Div(exp1, exp2) => {
                let val1 = exp1.evaluate();
                let val2 = exp2.evaluate();
                if val2 == 0 {
                    return Err("Can not divide by zero".to_string());
                }
                Ok(val1 / val2)
            }
        }
    }
    fn to_boxed(self) -> Box<Self> {
        Box::new(self)
    }
}

fn main() {
    // calculate: 2 + 3 * 4
    let exp = Add(
        Val(2).to_boxed(),
        Mul(Val(3).to_boxed(), Val(4).to_boxed()).to_boxed(),
    );
    match exp.evaluate() {
        Ok(res) => println!("Expression evaluated to: {res}"),
        Err(error) => println!("Error: {error}"),
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
enum Expression {
    Val(i32),
    Add(Box<Expression>, Box<Expression>),
    Sub(Box<Expression>, Box<Expression>),
    Mul(Box<Expression>, Box<Expression>),
    Div(Box<Expression>, Box<Expression>),
}
use {Expression::Add, Expression::Div, Expression::Mul, Expression::Sub, Expression::Val};
impl Expression {
    fn evaluate(&self) -> Result<i32, String> {
        match self {
            Val(val) => Ok(*val),
            Add(exp1, exp2) => {
                let val1 = exp1.evaluate()?;
                let val2 = exp2.evaluate()?;
                Ok(val1 + val2)
            }
            Sub(exp1, exp2) => {
                let val1 = exp1.evaluate()?;
                let val2 = exp2.evaluate()?;
                Ok(val1 - val2)
            }
            Mul(exp1, exp2) => {
                let val1 = exp1.evaluate()?;
                let val2 = exp2.evaluate()?;
                Ok(val1 * val2)
            }
            Div(exp1, exp2) => {
                let val1 = exp1.evaluate()?;
                let val2 = exp2.evaluate()?;
                if val2 == 0 {
                    return Err("Can not divide by zero".to_string());
                }
                Ok(val1 / val2)
            }
        }
    }
    fn to_boxed(self) -> Box<Self> {
        Box::new(self)
    }
}

fn main() {
    // calculate: 2 + 3 * 4
    let exp = Add(
        Val(2).to_boxed(),
        Mul(Val(3).to_boxed(), Val(4).to_boxed()).to_boxed(),
    );
    match exp.evaluate() {
        Ok(res) => println!("Expression evaluated to: {res}"),
        Err(error) => println!("Error: {error}"),
    }
}
  ```
</details>

#### Propagating errors 2

```rust,editable,compile_fail
// Say we're writing a game where you can buy items with tokens. All items cost
// 5 tokens, and whenever you purchase items there is a processing fee of 1
// token. A player of the game will type in how many items they want to buy,
// and the `total_cost` function will calculate the total number of tokens.
// Since the player typed in the quantity, though, we get it as a string-- and
// they might have typed anything, not just numbers!

// Right now, this function isn't handling the error case at all (and isn't
// handling the success case properly either). What we want to do is:
// if we call the `parse` function on a string that is not a number, that
// function will return a `ParseIntError`, and in that case, we want to
// immediately return that error from our function and not try to multiply
// and add.

use std::num::ParseIntError;

pub fn total_cost(item_quantity: &str) -> Result<i32, ParseIntError> {
    let processing_fee = 1;
    let cost_per_item = 5;
    let qty = item_quantity.parse::<i32>();

    Ok(qty * cost_per_item + processing_fee)
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn item_quantity_is_a_valid_number() {
        assert_eq!(total_cost("34"), Ok(171));
    }

    #[test]
    fn item_quantity_is_an_invalid_number() {
        assert_eq!(
            total_cost("beep boop").unwrap_err().to_string(),
            "invalid digit found in string"
        );
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
use std::num::ParseIntError;

pub fn total_cost(item_quantity: &str) -> Result<i32, ParseIntError> {
    let processing_fee = 1;
    let cost_per_item = 5;
    let qty = item_quantity.parse::<i32>()?;

    Ok(qty * cost_per_item + processing_fee)
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn item_quantity_is_a_valid_number() {
        assert_eq!(total_cost("34"), Ok(171));
    }

    #[test]
    fn item_quantity_is_an_invalid_number() {
        assert_eq!(
            total_cost("beep boop").unwrap_err().to_string(),
            "invalid digit found in string"
        );
    }
}
  ```
</details>

#### Propagating errors 3

```rust,editable,compile_fail
// Modify the functions to propagate the error instead of panicking.

fn factorial(n: u32) -> Result<u32, String> {
    if n == 0 {
        return Ok(1);
    } else if n > 12 {
        // Factorial of values > 12 would overflow u32, so return an error
        return Err(String::from("Input too large"));
    }
    let result = n * factorial(n - 1).unwrap();
    Ok(result)
}

fn print_factorial(n: u32) -> Result<(), String> {
    let result = factorial(n).unwrap();
    println!("Factorial of {} is: {}", n, result);
    Ok(())
}

fn main() {
    let n = 13;
    if let Err(err) = print_factorial(n) {
        eprintln!("Error calculating factorial of {}: {}", n, err);
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
fn factorial(n: u32) -> Result<u32, String> {
    if n == 0 {
        return Ok(1);
    } else if n > 12 {
        // Factorial of values > 12 would overflow u32, so return an error
        return Err(String::from("Input too large"));
    }
    let result = n * factorial(n - 1)?;
    Ok(result)
}

fn print_factorial(n: u32) -> Result<(), String> {
    let result = factorial(n)?;
    println!("Factorial of {} is: {}", n, result);
    Ok(())
}

fn main() {
    let n = 13;
    if let Err(err) = print_factorial(n) {
        eprintln!("Error calculating factorial of {}: {}", n, err);
    }
}
  ```
</details>
