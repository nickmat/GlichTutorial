# GlichTutorial

Tutorial Steps - Glich

## 1 Glich Basic Script Language

### 1.1 Hello World

```
// Hello World Script

write "Hello World!" nl;
```
#### 1.1.1 Running Scripts

#### 1.1.2 Glich Command Line glcs.exe

#### 1.1.3 Glich IDE Gliched gliched.exe

#### 1.1.3 Basic syntax

Comments (treated as whitespace)
```
// Single line comment

/* Multi line comment
write "This is commented out" nl;
*/
```
All scripts consist of one or more statements.
Some statements can contain sub-statements and so on...

Statements can take one of 3 forms.

##### 1.1.3.1 Simple Statement

This takes the form `name ... ;`
It starts with the statement name and ends with a **;** (semi-colon).
Examples:-
```
mark test1;
let name = 200;
name += 10;
file note write "notes.txt";
```

##### 1.1.3.2 Compound Statement
A statement which contains one or more sub-statements.
this has the form `name ... { sub-statement; ... }`
The sub-statements may be normal statements
or the may be specialist statements depending on context.
Examples:-
```
function pluspos( a, b ) {
    let x = a + b;
    result = @if( x<0, -x, x );
}
object test {
    values a b;
    function sum {
        result = a + b;
    }
}
```
These statements details will be explained later,
for the moment we are just looking at their form.

##### 1.1.3.1 Construction Statement
These statements begin with a start name and end with an end name;
There are currently ony two such statements. `if ... endif` and `do ... loop`.
Example:-
```
do
    while i < 10;
    write value * i nl;
    i += 1;
loop
```

### 1.2 Input and Output

Glich is designed to be embedded in a controlling program.
The host program is responsible for how the input and output
is presented to the user.
The script uses the `write expression ... ;`for standard output
and the `@read(prompt)` expression for standard input.

The following example was entered into the glcs console program:-
```
glcs: let name = @read("Enter your name: ");
Enter your name: Nick
glcs: write "Hello " + name + ", how are you?";
Hello Nick, how are you?
glcs:
```

#### 1.2.1 Write Statement, Expressions and Literals

The minimum write statement is:-
```
write expression;
```
An expression in Glich (as all other programming languages)
is a value that executes in a way that ends up being another value.
When that value is in a write statement
it is converted to a text and sent to the program's output.

##### 1.2.1.1 Glich Value Types

Glich is an untyped language.
This doesn't mean there are no types,
just that the types do not need to be declared
and can change depending on context.
It is also possible to change the type using built-in functions.

In Glich there are ten possible types a value can take:-

| Type | Example | Note |
| ---  | --- | --- |
| Number | 123 or 123n | 64-bit integer (Range approx. +/- nine quintillion) |
| Field | 456 or 456f | 32-bit integer with +/-infinity and ? (Not a number) |
| Range | 5..10 | A range  of field values. |
| RList | 1..20 \| 30..35 | A list of ranges |
| Float | 10.0 | A floating point decimal number |
| Boolean | true | Holds one of the values: true or false |
| String | "Hello" | Text as a string of characters |
| Object | {x: 56, "Hi"} | A named collection of values |
| Error | Error (3): Must be integer. | Error in expression. |
| Null | null | Not assigned a value. |

Both number and field are integer values
but normally we do not need distinguish between them.
The script will convert them as required.

##### 1.2.1.2 Literal Values.

There are only 3 literal entries, Integer numbers,
floating point numbers and text characters strings 

###### 1.2.1.2.1 Integer Literal

The characters 0-9, no gaps or commers,
is read in as a positive integer.
Use the unitary '-' minus operator for negative integers.

Glich has two types of integers which are named as
**number** and **field**.
By default, a plain integer will default to a number type.

The field type also has the special value infinity
(entered as **infinity**) and not-a number or invalid
(entered as **?**).

Normally the script will convert between the two integer types
as required.

The default type can be changed using the **set** statement,
as we will see later.

If there is a reason to enter a specific type
the integer can followed by an **f** (123f) for field
and **n** (456n).

###### 1.2.1.2.2 Floating Point Literal

The characters 0-9 and a dot '.'
the first character must be a digit.
is read in as a positive floating point.
(A future version will allow floats to entered in exponent form.)

The float type also has the special values infinity
(entered as **inf**) and not-a-number (entered as **nan**).

###### 1.2.1.2.3 Text String Literal

A text string literal is a sequence of characters
enclosed between **"** quote characters.
To include the quote character itself,
use two quotes together.
```"He said ""Hi"""```

New line characters can also be included within the quotes.

##### 1.2.1.3 Expressions.

Expressions and their operators can be classified as follows:-

| Category | Operators |  |
| --- | --- | --- |
| Arithmetic | Plus, Minus (binary and unitary), Multiply, Divide (Float and integer) and Modulo | |
| Set Operators | Range, Union, Intersection, Symmetric difference, Relative complement and Complement | |
| Comparison | Not, Equal, Not equal, Greater than, Greater than or equal, Less than, Less than or equal | |
| Boolean | Not, And, Inclusive or | |
| Grouping | Parenthesis, Object literal | |
| Subscript | Member, Property | |
| Function | Function (binary and unitary) | |
| Text Strings | Concatenation | |

Note that other operations are implemented as built-in functions,
such as the @if(comparison, if_true, if_false) function.

###### 1.2.1.3.1 Arithmetic Operations

For numeric_types, the arithmetic operators '+' (addition), '-' (subtraction,
negation) and '*' (multiplication) retain their normal mathematical meanings.

There are two forms for division,
normal division '/' which always gives a floating point result.
Division **div** and **mod** provides integer division and modulus (remainder).
Note that the modulo operator always returns a positive value.
```
write 5 * 20 + 1;
```
Will output `101`.

###### 1.2.1.3.2 Extended Integer Set Operations

The set operations all operate on the **field**, **range** and **rlist**
value types.

The **range** value type represents an inclusive range range of field values.
A range may include the +/- infinity value.
If either value in the range is the '?' invalid value,
then it is an invalid range.
The range is always held with the lowest value first.

The **range operator** '**..**' double-dot is used to create a range value.
For example, `5..20`.

A field can be converted into a range, so `50` = `50..50`,
a range size of 1.
This is normally done automatically.
It can also be forced using the @rlist( field ) function.

The **rlist** value type is a list of ranges.
The list may contain zero or more ranges.
If the rlist has more than one range,
it is written using the union '|' operator between ranges.
An empty list is written using the **empty** keyword.

An rlist is always held and written as a **well ordered list**.
A well ordered list can be described as,
ranges are always shown with the lowest value first,
multiple ranges do not overlap or abut one another
and are in ascending order.

**Note:** The keywords used for the infinity values depend on the context.
The default Glich keywords are **+infinity** and **-infinity**
but if the hics extension library is loaded,
the keywords are changed to **past** and **future**.
This can always be changed using the **set context** statement.

The **Complement operator** **!** exclamation character 
```
set context glich;
write !0;
```
Output: `-infinity..-1 | 1..+infinity`



#### 1.2.2 Let Statement and Variables

1.2.3 Mark Statement

1.2.3 File Statement and Input, Output

1.3 Script Construction

1.3.1 If Statement

1.3.2 Do Loop Statement

1.3.3 Function and Command Statements

1.3.4 End Statement

1.3.5 Call Statement

1.4. Built-in Functions and Commands

1.4.1 General Purpose Functions

1.4.2 Type Conversion Functions

1.5 Object Definition

1.5.1 Built-in Object

1.6 Set Statement

1.6.1 Set Integer

1.6.2 Set Context

2 Hics Extension

2.1 Scheme Statement

2.2 Built-in Functions

2.3 Grammar and Format Statements

2.4 Lexicon Statement

3 Hics Library
