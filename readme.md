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

| Category | Operators |
| --- | --- |
| Arithmetic | Plus, Minus (binary and unitary), Multiply, Divide (Float and integer) and Modulo |
| Text Strings | Concatenation |
| Comparison | Not, Equal, Not equal, Greater than, Greater than or equal, Less than, Less than or equal |
| Boolean | Not, And, Inclusive or |
| Set Operators | Range, Union, Intersection, Symmetric difference, Relative complement and Complement |
| Subscript | Index, Member |
| Function | Function (binary and unitary) |

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

###### 1.2.1.3.2 Text String Operations

The **+** operator is also used for **string Concatenation**.

 If both sides of the operator are string values,
 the right hand side will be appended to the left hand.
 If just one side is a string value
 then the other side is converted to a string first.

###### 1.2.1.3.3 Comparison Operations

The comparison operators must have operands of the same type,
after type allowing for type promotion.
The output is always a boolean type.

###### 1.2.1.3.4 Boolean Operations

The unary prefix operator 'not' and
the binary operators 'and' and 'or' can be used with boolean types only.
Any other type will produce an error

###### 1.2.1.3.5 Extended Integer Set Operations

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

The **Complement operator** **!** exclamation character.
This applies the **not** operator to every member of the
extended integer set.
```
set context glich;
write !0;
```
Output: `-infinity..-1 | 1..+infinity`.

The **Union operator** **|** vertical bar character.
This applies the **or** operator to every member pair of the
extended integer set.
```
write 20..75 | 50..100;
```
Will output: `20..100`.

The **Intersection operator** **&** ampersand character.
This applies the **and** operator to every member pair of the
extended integer set.
```
write 20..75 & 50..100;
```
Will output: `50..75`.

The **Symmetric Difference operator** **^** caret character.
This applies the **exclusive or** operator to every member pair of the
extended integer set.
```
write 20..75 ^ 50..100;
```
Will output: `20..49 | 76..100`.

The **Relative complement operator** **\** backslash character.
This applies the **& !** operator to every member pair of the
extended integer set.
This is the only set operation in which the order of the input is significant.
```
write 20..75 \ 50..100;
```
Will output: `20..49`. and
```
write 50..100 \ 20..75;
```
Will output: `76..100`.

###### 1.2.1.3.6 Subscript Operations

The **Subscript operators** uses the **[ ]** square brackets.
It is named subscript because usually in mathematics,
these would be written as a subscript to their main value.

There are two types of subscripts, index and property.

The **Index Subscript operator** is used with either rlist
or object types.
```
write (8..16 | 20..50 | 75..99)[1];
```
Will output: `20..50`.

```
write {: 2024, 5, 27}[0];
```
Will output: `2024`.

As we will see later, objects can have named members.
These can be accessed by using the name (either as a string or a name).
```
write {s:g 2024, 5, 27}[day];
// or
write {s:g 2024, 5, 27}["day"];
```
Will output: `27`.

###### 1.2.1.3.7 Functions

A function may be build-in
or user defined using the function definition statement.

Functions are introduced using the **Function operator** **@** at character.
The unitary function takes optional arguments and outputs a value
following the definition of the function.
```
let x = 5;
write @if( x < 10, "x is less than 10", "x is 10 or more" );
```
Will output `x is less than 10`.

The binary function has an object as the first operand
and a function declared as part of the object.
Some built-in functions can operate on any object.
```
write {: 1,,3,,,6} @mask( {: 10,20,,40} );
```
Will output `{: 1, 20, 3, 40, null, 6}`.

#### 1.2.2 Let Statement and Variables

The **let** is used to create and change named variables.

##### 1.2.2.1 Variable Assignment

The `let` statement
can be used with the basic assignment operator `=` equal sign.

```
let x = 123;
write x;
```
Will output `123`.

The first time a variable is named
the `let` statement name must be used,
but in subsequent uses the `let` may be omitted.

```
let x = 123;
write x,;
x = 456;
write x;
```
Will output `123, 456`.

Note: In the `write` statement,
following an expression with a `,` commer
will output a commer followed by a space.
This can be used to write a commer separated list to the output device.

##### 1.2.2.2 Variable Arithmetic Assignment

As well as simple assignment,
we can use the **Arithmetic Assignments** `+=`, `-=`, `*=` and `/=`
to modify an existing variable.

```
let count = 1;
write count,;
count += 1;
write count;
```
Will output `1, 2`.

#### 1.2.3 Mark Statement

Note that variables and definitions are remembered by the host program
even after the script has stopped running.
This can cause problems if you want to run the same script multiple times,
as definitions can only be created once.

The `mark` statement
can be used the reset the internal memory back to where it was
when the **mark** was first encountered.
So starting a script with a mark statement
will ensure every time it is run, the internal memory state
will be the same.

The mark statement is written as:-
```
mark tag;
```
Where `tag` is a *name or literal* value.
```
mark test;
let x = 10;
write x,;
mark test;
write x;
```
Will output `10, Error (5): Variable "x" not found.`

1.2.3 File Statement and Input, Output

To redirect output to a file, instead of the default output,
We need to define a file code using the **file** statement.
```
mark test;
let fname = "notes.txt";
file out write fname;
write.out "A line of text." nl;
```
Will create a text file named "notes.txt" in the current directory,
or if the file already exists, its contents will be deleted.
The write statement will enter the text "A line of text."
to the file.

```
file in read fname;
write @read.in nl;
```
Will write the first line (`A line of text.`) of the file to the standard output.

1.3 Script Construction

Glich has two statements to control the program flow,
the **do ... loop** and **if ... endif** statements.

It also has two subroutine definition statements:
- **function** *fcode* **{** *sub-statements* **}**
- **command** *ccode* **{** *sub-statements* **}**

With the function being called with the function operator **@**
as in **@fcode**
and the command with the **call ccode;** statement.

1.3.1 Do Loop Statement

The statements between the `do ... loop` will be executed
until either a **while** *condition* **;**
sub-statement is `false`
or an **until** *condition* **;** is `true`.

```
let i = 1;
do
    write i;
    until i = 10;
    write ", ";
    i += 1;
loop
````
Will output `1, 2, 3, 4, 5, 6, 7, 8, 9, 10`.

```
let a = {: "one", "two", "three"};
let i = 0;
do
    write a[(i)];
    i += 1;
    while i < @size(a);
    write ", ";
loop
```
will output `one, two, three`.

Note that the index `i` needs to be in parenthesis `a[(i)]`
to avoid it being mistaken for a value named `i`.

1.3.2 If Statement

the **if** statement with the optional *elseif** and **else**
sections will carry out selected statements
depending on the conditions set. 

```
let x = 10;
if x<>0
    write x;
endif
```
Will write `10` to the standard output.

```
let x = 0;
do
    x += 1;
    if x = 3
        write "x is 3";
    elseif x mod 2 = 0
        write "x is even";
    else
        write "x is odd but not three"; 
    endif
    until x = 5;
    write ", ";
loop
```
Will output `x is odd but not three, x is even, x is 3, x is even, x is odd but not three`

1.3.3 Function Definition Statement and Operator

Functions must be defined before they can be used.
They may include *arguments* in parenthesis,
which appear as local variables within the definition.
A local variable call **result** is also available,
and holds the return value of the function.
```
function fun( a, b ) {
    result = a + b;
}
write @fun( 7, 4 );
```
Will output `11`.

A function may also have a *qualifier*.
This is written after a dot **'.'** following the function name
and before any arguments.
```
function fun.qual( a, b ) {
    if qual = "mult"
        result = a * b;
    else
        result = a + b;
    endif
}
write @fun( 7, 4 ), @fun.mult( 7, 4 );
```
Will output `11, 28`.

1.3.4  Command Definition and Call Statements

1.3.5 End Statement

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
