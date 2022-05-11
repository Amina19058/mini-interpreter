# Lambda programming language

Lambda is imperative interpreted programming C++ like language.
Extension of the Lambda programming language id `.mini`.


The full documentation is here: https://www.notion.so/Lambda-Programming-Language-c557298411894b74ac4fa3dd8466b922


## Language specification

### Program

A program is a sequence of definitions. It may also contain comments and preprocessor directives, which are just ignored by the parser. 

An interpretable program must have a function `main` of type `int` that takes no arguments. 

It may or may not have a `return` statement.

```cpp
    int main () {
      ...
    }
```
---

### Comments

There are three kinds of comments.

- `/*` ... `*/` - multiline comments
- `//` - single-line comments
- `#` - preprocessor directive

Comments cannot be nested.

---

### Functions

Function definition has a type, name, an argument list, and body:

```cpp
    int foo(double x, int y)
    {
      return y + 9;
    }
```

A **function body** is either a list of statements enclosed in curly brackets `{` `}` , or an empty body consisting of a semicolon `;`.

The **function evaluation** starts by evaluating the arguments. The statements in the body are then executed in the order that they were written.

The function returns a value, which is obtained from the `return` statement. If the return type is `void`, no return statement is required.

**Typing rules:**

1. The same function name may be used in at most one function definition.
2. All `return` statements in a function body must return an expression whose type is the return type of the function. 

Lambda language has two **built-in functions** dealing with input and output:

```cpp
    void print(T x);   // prints an argument of type T and a newline in standard output
    T    read();       // reads an argument of type T from standard input
```

An argument list consist of argument declarations separated by comma and is enclosed in parentheses `(` `)`. 

An argument declaration has a type and an identifier: `int x`

Argument declarations with multiple variables (`int x, y`) are not included.

**Typing rule:** An argument list may only use each variable once.

---

### Statements

Any expression or declaration followed by a semicolon `;` can be used as a statement.

- **Expression**
    
    For example, statement returning an expression: `return i + 9;`
    
    Lower block is full description of expression.
    
- **Declaration**
    
    Declaration adds a variable to the current environment. 
    
    type and one variable:
    
    ```cpp
    int i;
    ```
    
    type and multiple variables:
    
    ```cpp
    int i, j;
    ```
    
    type and one initialised variable:
    
    ```cpp
    int i = 6;
    ```
    
    **Typing rule:** The initializing expression must have the declared type.
    
- **While loop**
    
    While loop consist of an expression in parentheses followed by a statement:
    
    ```cpp
    while (i < 10){
    	++i;
    }
    ```
    
    The condition expression is evaluated first. If the value is `true`, the body is interpreted in the resulting environment, and the `while`statement is executed again. Otherwise, exit the loop.
    
    **Typing rule:** The expression in parentheses must have type **bool**.
    
- **Conditional statement**
    
    ```cpp
    if (x > 0){
    	return x;
    } 
    else return y;
    ```
    
    The condition expression is evaluated first. If the value is `true`, the statement after `if` is interpreted. Otherwise, the statement after `else` is interpreted.
    
    **Typing rules:** 
    
    1. The expression in parentheses must have type **bool**.
    2. The expression in parentheses must be followed by a statement.
    3. `else` must be followed by another statement. 
    4. No `else`-less `if`
- **Block**
    
    Block is any list of statements (including empty list) between curly brackets:
    
    ```cpp
        {
          int i = 2;
          {
          }
          i++;
        }
    ```
    
    **Typing rules:**
    
    1. A variable may only be declared once on the same block level.
    2. The parameters of a function have the same level as the top-level block in the body.

---

### Expressions

The interpretation of an expression, also called evaluation, returns a value whose type is determined by the type of the expression. Evaluation of an expression in an environment should always result in a value.

The following table gives the expressions, their precedence and associativity:

| Precedence | Expression forms | Associativity | Explanation | Type |
| --- | --- | --- | --- | --- |
| 15 | literal | - | literal | literal type |
| 15 | identifier | - | variable | declared type |
| 15 | f(e,...,e) | none | function call | return type |
| 14 | v++, v-- | none | in/decrement | (sugar) |
| 13 | ++v, --v | none | in/decrement | (sugar) |
| 12 | e*e, e/e | left | mult, div | operand type (int or double) |
| 11 | e+e, e-e | left | add, sub | operand type (int or double) |
| 9 | e<e, e>e, e>=e, e<=e | none | comparison | bool |
| 8 | e==e, e!=e | none | (in)equality | bool |
| 4 | e&&e | left | conjunction | bool |
| 3 | e||e | left | disjunction | bool |
| 2 | v=e | right | assignment | type of Left Hand Side |

**Typing rules:**

1. Variables have the type declared in the nearest enclosing block. A variable must be declared before usage.
2. The arguments of a function call must have types corresponding to the argument types of the called function. The number of arguments must be the same as in the function declaration. 
3. Increments and decrements only apply to variables. 
4. Comparison and in/equality apply to two operands of the same type, which is `int`, `double`, or `bool`.
5. Conjunction and disjunction apply to operands of type `bool` only.
6. Assignments have the same type as the left hand side variable. 
7. Only variables are used as left-hand-sides.

**List of possible expressions:**

- **Literals**
    
    Literal is not is not evaluated further, it is just converted to the corresponding value. There are integer, floating point and true, false literals.
    
- **Identifiers**
    
    An identifier is a letter followed by a list of letters, digits, and underscores. It is basically a ‘name’, e.g. x, temp_2, and so on.  Identifier is evaluated by looking up its value in the innermost context where it occurs. If the variable is not in the context, or has no value there, the interpreter terminates with an error message. For instance, if there is no identifier x, the error message would be: `Unknown variable x`.
    
- **Function call**
    
    ```cpp
        foo(8,9,true);
    ```
    
    It is interpreted by first evaluating its arguments from left to right. The environment is then looked up to find out how the function is interpreted on the resulting values.
    
- **Postincrement and postdecrement, eg. i++ and i--**
    
    It has the same numeric value as its body initially has (here `i`). The value of the variable `i` in the expression  `i++` is then incremented by 1. `i--` correspondingly decrements `i` by 1. If `i` is of type `double`, 1.0 is used instead.
    
- **Preincrement and predecrement, eg. ++i and --i**
    
    `++i` has the same value as `i` plus 1. This incremented value replaces the old value of `i`. The decrement and double variants are analogous.
    
- **The arithmetic operation**
    
    The arithmetic operations are addition, subtraction, multiplication, and division,
    
    ```cpp
        a + b
        a - b
        a * b
        a / b
    ```
    
    are interpreted by evaluating their operands from left to right. The resulting values are then evaluated by using appropriate operations of the implementation language. For simplicity  `int` should result in `int` and `double` should result in `double`.
    
- **Comparisons**
    
    Comparisons,
    
    ```cpp
        a < b # less than
        a > b # more than
        a >= b # more or equal than
        a <= b # less or equal than
        a == b # equal to
        a != b # not equal to
    ```
    
    are treated similarly to the arithmetic operations, using comparisons of the implementation language. The returned value has a bool type.
    
- **Conjunction, e.g. a && b**
    
    it is evaluated *lazily*: first `a` is evaluated. If the result is true (i.e. 1), also `b` is evaluated, and the value of `b` is returned. However, if `a` evaluates to false (i.e. 0), then false is returned without evaluating `b`.
    
- **Disjunction, e.g. a || b**
    
    It is also evaluated lazily: first `a` is evaluated. If the result is false (0), also `b` is evaluated, and the value of `b` is returned. However, if `a` evaluates to true (1), then true is returned without evaluating `b`.
    
- **Assignment, e.g. x = a**
    
    It is evaluated by first evaluating `a`. The resulting value is returned, but also the context is changed by assigning this value to the innermost occurrence of `x`.
    

---

## Nested definitions

TODO

---

TODO:

Sequencing

Type Ascription

Class fields/methods accessibility level

General Recursion

Let-bindings

Tuples

Records

Built-in Lists

Built-in Arrays

Built-in Dict

Subtyping

---

## Types

### Base types

- int: integer numbers, e.g. `-47` is int value
- double: numbers with remainder, e.g. `3.14159`
- bool: boolean values, e.g. `true` and `false`
- string: string/text values, e.g. `‘Hello, world!’` (use single and double quotes)
- void: can be perceived as an empty or undefined value, which need never be shown

### User-defined types

TODO

---

## Simple Constraint-Based Type Inference (e.g. support auto types)

TODO

---

## **Standard Library**

TODO

---

## Exceptions

TODO

---

# **Implementation**

### Environment

| Name | Version | Link |
| --- | --- | --- |
| BNFC | 2.9.4 | http://bnfc.digitalgrammars.com/ |
| CUP | 0.11b | http://www2.cs.tum.edu/projects/cup/ |
| Jlex | 1.2.6 | https://www.cs.princeton.edu/~appel/modern/java/JLex/ |
| Java | 12.0.2 | https://www.oracle.com/java/technologies/javase/jdk12-archive-downloads.html |

## **How To Build**

Make sure you installed java, Jlex, CUP and BNFC (you can follow tutorial [https://bnfc.digitalgrammars.com/tutorial/bnfc-tutorial.html](https://bnfc.digitalgrammars.com/tutorial/bnfc-tutorial.html)).

You probably want to clean up previous build files first:

```bash
make clean
```

Run make file for generating parser and lexer. This should generate `Mini/Test` files that you can now use to test parsing of the source code in the target language.

```bash
make
```

Compile `eval` folder

```bash
javac eval/Interpreter.java eval/TypeException.java eval/TypeChecker.java
```

Compile `runmini.java`

```bash
javac -Xlint runmini.java
```

---

## **How To Use**

The interpreter reads standard input and files, parses a series of expressions separated by a semicolon (;), typechecks and evaluates each expression and prints out the results.

Input can be read from user typing on the terminal and standard input redirected from a file or by `echo`

```
>>> java runmini example.mini                 # Typing on the terminal
		3
>>> java runmini example.mini <test-input     # Standard input redirected from a file
>>> echo 3 | java runmini example.mini        # Standard input redirected from a echo
```

Consider next Lambda language program at file `example.lama`:

```cpp
int x;
int y;
x = 6;
y = x + 7;
print y;
```

Run program and see the result:

```bash
>>> java runmini ./examples/example.mini
13
```

### Valid program

```cpp
int main ()
  {
    int i = read();
		int factorial(int num) /* Let's create the factorial function */
    {
			if (5!=7) && (num > 0) 
			{ 
				int result = 1
				while (num > 0)
				{
					result = result * num;
					--num;
				};
				return result;
			};
			else return 1;
    }
		factorial(7) // function call
  }
```

### Invalid Program - Type error

```cpp
int y;
y = x + 3;
print(y);
```

```cpp
TYPE ERROR
eval.TypeException: Unknown variable x.
```

### Invalid Program - Interpreter error

```
int i;
printInt(i);
```

```cpp
INTERPRETER ERROR
Uninitialized variable i.
```

---

# Additional Information

### Notion link

https://www.notion.so/Lambda-Programming-Language-c557298411894b74ac4fa3dd8466b922

### Team members

- **Polina Minina** - implements type checker, implements compiler
- **Amina Khusnutdinova** - implements syntax, implements compiler
- **Kseniya Evdokimova** - implements syntax, implements tests
