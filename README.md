# Lambda programming language

Lambda is imperative interpreted programming C++ like language.
Extension of the Lambda programming language id `.mini`.


The full documentation is here: https://www.notion.so/Lambda-Programming-Language-c557298411894b74ac4fa3dd8466b922


## Language specification

### Program

A program is a sequence of definitions. It may also contain comments and preprocessor directives, which are just ignored by the parser. 

An interpretable program must have a function `main` of type `int` that takes no arguments. 

It may or may not have a `return` statement.

## Features

- variables
- operations on base types
- lists
- user-defined terms and types (user-defined types are structured algebraic data types)
- auto types
- first-class functions, functions with multiple arguments
- nested definitions
- subtyping exceptions
- standard library


## Project Structure

`examples` - folder with program examples

`eval` - folder with typechecker and interpreter

`MakeFile` - file to compile classes

`runmini.java` - evaluation file

`Mini.cf` - normal syntax

## Types
- int
- bool
- string
- void

## How To Build
Make sure you installed java, Jlex, CUP and BNFC (you can follow tutorial https://bnfc.digitalgrammars.com/tutorial/bnfc-tutorial.html).

### Building the interpreter
You probably want to clean up previous build files first:
```
make clean
```

Run make file for generating parser and lexer (TODO: add everything to the Makefile)
This should generate Mini/Test files that you can now use to test parsing of the source code in the target language.
```
make
```

Compile `eval` folder
```
javac eval/Interpreter.java eval/TypeException.java eval/TypeChecker.java
```
Compile `runmini.java`
```
javac -Xlint runmini.java 
```

## How To Use
The interpreter reads standard input and files, parses a series of expressions separated by a semicolon (;), typechecks and evaluates each expression and prints out the results.

```bash
java runmini ./examples/example.mini
```
```
13
4
4
4
13
```

## Valid Program

```
int x ;
x = 6 ;
int y ;
y = x + 7 ;
print y ;
```

Output:
```
13
```

## Invalid program

```
int i ;
printInt(i) ;
```
Output:
```
uninitialized variable i
```



