# GlichTutorial

Tutorial Steps - Glich

### 1 Glich Basic Script Language

#### 1.1 Hello World

```
// Hello World Script

write "Hello World!" nl;
```
##### 1.1.1 Running Scripts

##### 1.1.2 Glich Command Line glcs.exe

##### 1.1.3 Glich IDE Gliched gliched.exe

##### 1.1.3 Basic syntax

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

###### 1.1.3.1 Simple Statement

This takes the form `name ... ;`.
It starts with the statement name and ends with a **;** (semi-colon).
Examples:-
```
mark test1;
let name = 200;
name += 10;
file note write "notes.txt";
```

###### 1.1.3.2 Compound Statement
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
for the moment we are just showing their construction.

###### 1.1.3.1 Construction Statement
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

#### 1.2 Input and Output

Glich is designed to be embedded in a controlling program.
The host program is responsible for how the input and output
is presented to the user.

##### 1.2.1 Write Statement, Expressions and Literals

##### 1.2.2 Let Statement and Variables

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
