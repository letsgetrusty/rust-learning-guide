# Custom Errors

> Note: No official YouTube video yet, but our <a href="https://letsgetrusty.com/bootcamp-hsk41" target="_blank">Rust Developer Bootcamp</a> covers this topic!

<a href="https://letsgetrusty.com/bootcamp-hsk41" target="_blank">
    <img src="https://d1aettbyeyfilo.cloudfront.net/letsgetrusty/31007320_1703634176QCJNo_video_yet.png" alt="No video yet..."/>
</a>

#### Additional Resources
- <a href="https://youtu.be/f9hEjMUuNjw" target="_blank">Creating your own errors in Rust</a>

## Exercises

#### Creating custom errors

```rust,editable,compile_fail
// Complete the `factorial` function.

#[derive(Debug)]
enum Error {
    Overflow,
    InvalidInput,
}

fn factorial(num: i32) -> Result<i32, Error> {
    /*
       if num < 0 return InvalidInput error
       if num > 12 return Overflow error
       if num < 2 return num
       else return num * result of factorial num - 1
    */
}

fn main() {
    assert!(matches!(factorial(-12), Err(Error::InvalidInput)));
    assert!(matches!(factorial(20), Err(Error::Overflow)));
    assert!(matches!(factorial(5), Ok(120)));
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
#[derive(Debug)]
enum Error {
    Overflow,
    InvalidInput,
}

fn factorial(num: i32) -> Result<i32, Error> {
    if num < 0 {
        return Err(Error::InvalidInput);
    } else if num > 12 {
        return Err(Error::Overflow);
    }
    let res = if num < 2 { 
        num
    } else {
        num * factorial(num-1)?
    };
    Ok(res)
}

fn main() {
    assert!(matches!(factorial(-12), Err(Error::InvalidInput)));
    assert!(matches!(factorial(20), Err(Error::Overflow)));
    assert!(matches!(factorial(5), Ok(120)));
}
  ```
</details>

#### Converting errors 1

```rust,editable,compile_fail
// Complete the code by implementing `From<ParseIntError>` for `AddError`.

use std::num::ParseIntError;

#[derive(Debug)]
enum AddError {
    ParseError(ParseIntError),
    Overflow,
}

impl From<ParseIntError> for AddError {}

fn add(num1: &str, num2: &str) -> Result<u8, AddError> {
    let num1 = num1.parse::<u8>()?;
    let num2 = num2.parse::<u8>()?;
    num1.checked_add(num2).ok_or(AddError::Overflow)
}

fn main() {
    let (input1, input2) = ("23", "50");
    match add(input1, input2) {
        Ok(res) => println!("{input1} + {input2} = {res}"),
        Err(e) => println!("Error: {e:?}"),
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
use std::num::ParseIntError;

#[derive(Debug)]
enum AddError {
    ParseError(ParseIntError),
    Overflow,
}

impl From<ParseIntError> for AddError {
    fn from(value: ParseIntError) -> Self {
        Self::ParseError(value)
    }
}

fn add(num1: &str, num2: &str) -> Result<u8, AddError> {
    let num1 = num1.parse::<u8>()?;
    let num2 = num2.parse::<u8>()?;
    num1.checked_add(num2).ok_or(AddError::Overflow)
}

fn main() {
    let (input1, input2) = ("23", "50");
    match add(input1, input2) {
        Ok(res) => println!("{input1} + {input2} = {res}"),
        Err(e) => println!("Error: {e:?}"),
    }
}
  ```
</details>

#### Converting errors 2


```rust,editable,compile_fail
// Complete the `add` function by converting from ParseIntError to AddError using `map_err` combinator.

use std::num::ParseIntError;

#[derive(Debug)]
enum AddError {
    ParseError(ParseIntError),
    Overflow,
}

fn add(num1: &str, num2: &str) -> Result<u8, AddError> {
    let num1 = num1.parse::<u8>()?;
    let num2 = num2.parse::<u8>()?;
    num1.checked_add(num2).ok_or(AddError::Overflow)
}

fn main() {
    let (input1, input2) = ("23", "50");
    match add(input1, input2) {
        Ok(res) => println!("{input1} + {input2} = {res}"),
        Err(e) => println!("Error: {e:?}"),
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
use std::num::ParseIntError;

#[derive(Debug)]
enum AddError {
    ParseError(ParseIntError),
    Overflow,
}

fn add(num1: &str, num2: &str) -> Result<u8, AddError> {
    let num1 = num1.parse::<u8>().map_err(|e| AddError::ParseError(e))?;
    let num2 = num2.parse::<u8>().map_err(|e| AddError::ParseError(e))?;
    num1.checked_add(num2).ok_or(AddError::Overflow)
}

fn main() {
    let (input1, input2) = ("23", "50");
    match add(input1, input2) {
        Ok(res) => println!("{input1} + {input2} = {res}"),
        Err(e) => println!("Error: {e:?}"),
    }
}
  ```
</details>

#### Error trait 1

```rust,editable,compile_fail
// Complete the code and make the tests pass by implementing std::error::Error for CalculationError

pub enum CalculationError {
    InvalidOperation(char),
    InvalidOperand(String),
    DivideByZero { dividend: i8 },
    Overflow,
}

pub fn calculate(num1: &str, num2: &str, operation: char) -> Result<i8, CalculationError> {
    let num1 = num1
        .parse::<i8>()
        .map_err(|_| CalculationError::InvalidOperand(num1.to_owned()))?;
    let num2 = num2
        .parse::<i8>()
        .map_err(|_| CalculationError::InvalidOperand(num2.to_owned()))?;
    match operation {
        '+' => num1.checked_add(num2).ok_or(CalculationError::Overflow),
        '-' => num1.checked_sub(num2).ok_or(CalculationError::Overflow),
        '*' => num1.checked_mul(num2).ok_or(CalculationError::Overflow),
        '/' => {
            if num2 == 0 {
                return Err(CalculationError::DivideByZero { dividend: num1 });
            }
            num1.checked_div(num2).ok_or(CalculationError::Overflow)
        }
        _ => Err(CalculationError::InvalidOperation(operation)),
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    struct Error {
        e: Box<dyn std::error::Error>,
    }
    impl Error {
        // presence of this method ensures that std::error::Error is satisfied for CalculationError
        fn new(err: CalculationError) -> Self {
            Self { e: Box::new(err) }
        }
    }

    #[test]
    fn invalid_operation() {
        let res1 = calculate("12", "20", '$');
        let res2 = calculate("45", "43", '^');
        match (res1, res2) {
            (Err(e1), Err(e2)) => {
                assert_eq!(
                    "$ is not a valid operation. Allowed: +,-,/,*",
                    format!("{}", e1)
                );
                assert_eq!(
                    "^ is not a valid operation. Allowed: +,-,/,*",
                    format!("{}", e2)
                );
            }
            _ => panic!("Error expected!"),
        }
    }

    #[test]
    fn invalid_operand() {
        let res1 = calculate("ab", "3r", '+');
        let res2 = calculate("45", "4.23", '^');
        match (res1, res2) {
            (Err(e1), Err(e2)) => {
                assert_eq!(
                    "ab is not a valid integer in range [-128, 127]",
                    format!("{}", e1)
                );
                assert_eq!(
                    "4.23 is not a valid integer in range [-128, 127]",
                    format!("{}", e2)
                );
            }
            _ => panic!("Error expected!"),
        }
    }

    #[test]
    fn divide_by_zero() {
        let res1 = calculate("45", "0", '/');
        let res2 = calculate("70", "0", '/');
        match (res1, res2) {
            (Err(e1), Err(e2)) => {
                assert_eq!(
                    "Can not divide by zero. Attempting to divide 45 by 0",
                    format!("{}", e1)
                );
                assert_eq!(
                    "Can not divide by zero. Attempting to divide 70 by 0",
                    format!("{}", e2)
                );
            }
            _ => panic!("Error expected!"),
        }
    }

    #[test]
    fn overflow() {
        let res = calculate("120", "120", '+');
        match res {
            Err(e) => {
                assert_eq!("Overflow while performing the operation", format!("{}", e));
            }
            _ => panic!("Error expected!"),
        }
    }
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
use std::error::Error;
use std::fmt::Display;

#[derive(Debug)]
pub enum CalculationError {
    InvalidOperation(char),
    InvalidOperand(String),
    DivideByZero { dividend: i8 },
    Overflow,
}

impl Display for CalculationError {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        let msg = match self {
            Self::InvalidOperation(op) => format!("{op} is not a valid operation. Allowed: +,-,/,*"),
            Self::InvalidOperand(num) => format!("{num} is not a valid integer in range [-128, 127]"),
            Self::DivideByZero { dividend } => format!("Can not divide by zero. Attempting to divide {dividend} by 0"),
            Self::Overflow => format!("Overflow while performing the operation"),
        };
        f.write_str(&msg)
    }
}

impl Error for CalculationError {
    fn source(&self) -> Option<&(dyn Error + 'static)> {
        None
    }
}

pub fn calculate(num1: &str, num2: &str, operation: char) -> Result<i8, CalculationError> {
    let num1 = num1
        .parse::<i8>()
        .map_err(|_| CalculationError::InvalidOperand(num1.to_owned()))?;
    let num2 = num2
        .parse::<i8>()
        .map_err(|_| CalculationError::InvalidOperand(num2.to_owned()))?;
    match operation {
        '+' => num1.checked_add(num2).ok_or(CalculationError::Overflow),
        '-' => num1.checked_sub(num2).ok_or(CalculationError::Overflow),
        '*' => num1.checked_mul(num2).ok_or(CalculationError::Overflow),
        '/' => {
            if num2 == 0 {
                return Err(CalculationError::DivideByZero { dividend: num1 });
            }
            num1.checked_div(num2).ok_or(CalculationError::Overflow)
        }
        _ => Err(CalculationError::InvalidOperation(operation)),
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    struct Error {
        e: Box<dyn std::error::Error>,
    }
    impl Error {
        // presence of this method ensures that std::error::Error is satisfied for CalculationError
        fn new(err: CalculationError) -> Self {
            Self { e: Box::new(err) }
        }
    }

    #[test]
    fn invalid_operation() {
        let res1 = calculate("12", "20", '$');
        let res2 = calculate("45", "43", '^');
        match (res1, res2) {
            (Err(e1), Err(e2)) => {
                assert_eq!(
                    "$ is not a valid operation. Allowed: +,-,/,*",
                    format!("{}", e1)
                );
                assert_eq!(
                    "^ is not a valid operation. Allowed: +,-,/,*",
                    format!("{}", e2)
                );
            }
            _ => panic!("Error expected!"),
        }
    }

    #[test]
    fn invalid_operand() {
        let res1 = calculate("ab", "3r", '+');
        let res2 = calculate("45", "4.23", '^');
        match (res1, res2) {
            (Err(e1), Err(e2)) => {
                assert_eq!(
                    "ab is not a valid integer in range [-128, 127]",
                    format!("{}", e1)
                );
                assert_eq!(
                    "4.23 is not a valid integer in range [-128, 127]",
                    format!("{}", e2)
                );
            }
            _ => panic!("Error expected!"),
        }
    }

    #[test]
    fn divide_by_zero() {
        let res1 = calculate("45", "0", '/');
        let res2 = calculate("70", "0", '/');
        match (res1, res2) {
            (Err(e1), Err(e2)) => {
                assert_eq!(
                    "Can not divide by zero. Attempting to divide 45 by 0",
                    format!("{}", e1)
                );
                assert_eq!(
                    "Can not divide by zero. Attempting to divide 70 by 0",
                    format!("{}", e2)
                );
            }
            _ => panic!("Error expected!"),
        }
    }

    #[test]
    fn overflow() {
        let res = calculate("120", "120", '+');
        match res {
            Err(e) => {
                assert_eq!("Overflow while performing the operation", format!("{}", e));
            }
            _ => panic!("Error expected!"),
        }
    }
}
  ```
</details>

#### Error trait 2


```rust,editable,compile_fail
// Complete the implementation of `Display` & `Debug` for `MonthError`.

use std::{error::Error, fmt::{Display, Debug}};

enum Month {
    Jan, Feb, March, April, May, June, July, Aug, Sept, Oct, Nov, Dec,
}

impl TryFrom<u8> for Month {
    type Error = MonthError;
    fn try_from(value: u8) -> Result<Self, Self::Error> {
        match value {
            1 => Ok(Month::Jan),
            2 => Ok(Month::Feb),
            3 => Ok(Month::March),
            4 => Ok(Month::April),
            5 => Ok(Month::May),
            6 => Ok(Month::June),
            7 => Ok(Month::July),
            8 => Ok(Month::Aug),
            9 => Ok(Month::Sept),
            10 => Ok(Month::Oct),
            11 => Ok(Month::Nov),
            12 => Ok(Month::Dec),
            _ => Err(MonthError {
                        source: None,
                        msg: format!("{value} does not correspond to a month")
                    })
        }
    }
}

impl ToString for Month {
    fn to_string(&self) -> String {
        match self {
            Self::Jan => "Jan",
            Self::Feb => "Feb",
            Self::March => "March",
            Self::April => "April",
            Self::May => "May",
            Self::June => "June",
            Self::July => "July",
            Self::Aug => "Aug",
            Self::Sept => "Sept",
            Self::Oct => "Oct",
            Self::Nov => "Nov",
            Self::Dec => "Dec"
        }.to_string()
    }
}

struct MonthError {
    source: Option<Box<dyn Error>>,
    msg: String,
}

impl Error for MonthError {
    fn source(&self) -> Option<&(dyn Error + 'static)> {
        self.source.as_deref()
    }
}

impl Display for MonthError {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        // write "Error converting input to months, check your input"
    }
}

impl Debug for MonthError {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        // if self.source is Some, write "{a}\nCaused by: {b:?}", a=self.msg, b=error stored inside Some
        // else, write self.msg to the Formatter
    }
}

// function to convert a string to corresponding vector of owned month strings
// e.g. "1 2 3 4" -> ["Jan", "Feb", "March", "April"]
fn get_months(months: &str) -> Result<Vec<String>, MonthError> {
    let nums =
    months.split(' ')
          .into_iter()
          .map(|num_str| num_str.parse::<u8>()
                                      .map_err(|e| MonthError { source: Some(Box::new(e)),
                                                                                   msg: format!("Can not parse {num_str} to u8") 
                                                                                }))
          .collect::<Result<Vec<u8>, _>>()
          .map_err(|e| MonthError {
            source: Some(Box::new(e)),
            msg: format!("Could not convert string to numbers")
          })?;
    let month_strs =
    nums.into_iter()
        .map(|num| Month::try_from(num))
        .collect::<Result<Vec<Month>, _>>()
        .map_err(|e| MonthError {
            source: Some(Box::new(e)),
            msg: format!("Could not convert nums to months")
        })?
        .into_iter()
        .map(|month| month.to_string())
        .collect::<Vec<String>>();
    Ok(month_strs)

}

fn convert_and_print(nums: &str) {
    match get_months(nums) {
        Ok(months) => println!("Months: {months:?}\n"),
        Err(e) => println!("{e:?}\n"),
    }
}

fn main() {
    let input1 = "1 2 3 4 9 10";
    let input2 = "xyz 10 12";
    let input3 = "1 3 20";
    convert_and_print(input1);
    convert_and_print(input2);
    convert_and_print(input3);
}
```

<details>
  <summary>Solution</summary>
  
  ```rust
use std::{error::Error, fmt::{Display, Debug}};

enum Month {
    Jan, Feb, March, April, May, June, July, Aug, Sept, Oct, Nov, Dec,
}

impl TryFrom<u8> for Month {
    type Error = MonthError;
    fn try_from(value: u8) -> Result<Self, Self::Error> {
        match value {
            1 => Ok(Month::Jan),
            2 => Ok(Month::Feb),
            3 => Ok(Month::March),
            4 => Ok(Month::April),
            5 => Ok(Month::May),
            6 => Ok(Month::June),
            7 => Ok(Month::July),
            8 => Ok(Month::Aug),
            9 => Ok(Month::Sept),
            10 => Ok(Month::Oct),
            11 => Ok(Month::Nov),
            12 => Ok(Month::Dec),
            _ => Err(MonthError {
                        source: None,
                        msg: format!("{value} does not correspond to a month")
                    })
        }
    }
}

impl ToString for Month {
    fn to_string(&self) -> String {
        match self {
            Self::Jan => "Jan",
            Self::Feb => "Feb",
            Self::March => "March",
            Self::April => "April",
            Self::May => "May",
            Self::June => "June",
            Self::July => "July",
            Self::Aug => "Aug",
            Self::Sept => "Sept",
            Self::Oct => "Oct",
            Self::Nov => "Nov",
            Self::Dec => "Dec"
        }.to_string()
    }
}

struct MonthError {
    source: Option<Box<dyn Error>>,
    msg: String,
}

impl Error for MonthError {
    fn source(&self) -> Option<&(dyn Error + 'static)> {
        self.source.as_deref()
    }
}

impl Display for MonthError {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        f.write_str("Error converting input to months, check your input")
    }
}

impl Debug for MonthError {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        if let Some(err) = &self.source {
            f.write_fmt(format_args!("{}\nCaused by: {:?}", self.msg, err))
        } else {
            f.write_str(&self.msg)
        }
    }
}

// function to convert a string to corresponding vector of owned month strings
// e.g. "1 2 3 4" -> ["Jan", "Feb", "March", "April"]
fn get_months(months: &str) -> Result<Vec<String>, MonthError> {
    let nums =
    months.split(' ')
          .into_iter()
          .map(|num_str| num_str.parse::<u8>()
                                      .map_err(|e| MonthError { source: Some(Box::new(e)),
                                                                                   msg: format!("Can not parse {num_str} to u8") 
                                                                                }))
          .collect::<Result<Vec<u8>, _>>()
          .map_err(|e| MonthError {
            source: Some(Box::new(e)),
            msg: format!("Could not convert string to numbers")
          })?;
    let month_strs =
    nums.into_iter()
        .map(|num| Month::try_from(num))
        .collect::<Result<Vec<Month>, _>>()
        .map_err(|e| MonthError {
            source: Some(Box::new(e)),
            msg: format!("Could not convert nums to months")
        })?
        .into_iter()
        .map(|month| month.to_string())
        .collect::<Vec<String>>();
    Ok(month_strs)

}

fn convert_and_print(nums: &str) {
    match get_months(nums) {
        Ok(months) => println!("Months: {months:?}\n"),
        Err(e) => println!("{e:?}\n"),
    }
}

fn main() {
    let input1 = "1 2 3 4 9 10";
    let input2 = "xyz 10 12";
    let input3 = "1 3 20";
    convert_and_print(input1);
    convert_and_print(input2);
    convert_and_print(input3);
}
  ```
</details>
