# hw2 report

|Field|Value|
|-:|:-|
|Name|Chih-Yu Kuo|
|ID|111550155|

## How much time did you spend on this project

About 7 hours.

## Project overview

This project implements a syntax parser for the `P` language using `yacc`. Additionally, it modifies the lexical scanner to return tokens.

### Project Structure

- /src
  - Makefile
  - **`scanner.l`**
  - **`parser.y`**
- /report
  - **`README.md`**

### Scanner (**`scanner.l`**)

Modify the lexical scanner to enable it to pass token information to the syntax parser.

##### For Example

```lex
%%

[a-zA-Z][a-zA-Z0-9]* { listLiteral("id", yytext); return IDENTIFIER;}

%%
```

### Parser (parser.y)

#### Token Definitions

Define the tokens that will be passed from the scanner.

##### For Example

```yacc
/* Identifiers */
%token IDENTIFIER

/* Integer Constants */
%token OCTAL_INTEGER DECIMAL_INTEGER
```
##### Delimiters

|lexeme|token||lexeme|token|
|:-:|:-:|:-:|:-:|:-:|
|`,`|','||`(`|'('|
|`;`|';'||`)`|')'|
|`:`|':'||`[`|'['|
||||`]`|']'|

##### Operators

|lexeme|token||lexeme|token||lexeme|token|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|`+`|'+'||`<`|LT||`and`|AND|
|`-`|'-'||`<=`|LE||`or`|OR|
|`*`|'*'||`<>`|NE||`not`|NOT|
|`/`|'/'||`>=`|GE||
|`mod`|MOD||`>`|GT||
|`=`|ASSIGN||`=`|EQ||

##### Reserved Words

|lexeme|token||lexeme|token||lexeme|token|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|`var`|VAR||`true`|TRUE||`for`|FOR|
|`array`|ARRAY||`false`|FALSE||`to`|TO|
|`of`|OF||`while`|WHILE||`begin`|BEGIN_BLOCK|
|`boolean`|BOOLEAN||`do`|DO||`end`|END_BLOCK|
|`integer`|INTEGER||`if`|IF||`print`|PRINT|
|`real`|REAL||`then`|THEN||`read`|READ|
|`string`|STRING||`else`|ELSE||`return`|RETURN|

##### Identifiers

|lexeme|token|
|:-:|:-:|
|`<id>`|IDENTIFIER|

##### Integer Constants

|lexeme|token|
|:-:|:-:|
|`<oct_integer>`|OCTAL_INTEGER|
|`<integer>`|DECIMAL_INTEGER|

##### Floating-Point Constants

|lexeme|token|
|:-:|:-:|
|`<float>`|FLOATING_POINT_NUMBER|

##### Scientific Notations

|lexeme|token|
|:-:|:-:|
|`<scientific>`|SCIENTIFIC_NOTATION|

#### Syntactic Definitions

Complete the following structure according to the description in the `spec`.

##### For Example

```yacc
%%

/* Program Units */
program_unit: program | function;

    /* Program */
program: IDENTIFIER ';' declaration_list function_list compound_statement END_BLOCK;

declaration_list: /* empty */ | sub_declaration_list;
sub_declaration_list: declaration | sub_declaration_list declaration;

function_list: /* empty */ | sub_function_list;
sub_function_list: function | sub_function_list function;

    /* Function */
function: function_declaration | function_definition;

function_declaration: function_header ';';
function_definition: function_header compound_statement END_BLOCK;

function_header: IDENTIFIER '(' formal_argument_list ')' ':' scalar_type | IDENTIFIER '(' formal_argument_list ')';

formal_argument_list: /* empty */ | sub_formal_argument_list;
sub_formal_argument_list: formal_argument | sub_formal_argument_list ';' formal_argument;

formal_argument: identifier_list ':' type;

%%
```

##### Structure

- _Program Units_
  - _Program_
  - _Function_
    - _function_declaration_
    - _function_definition_
- _Declarations_
  - _variable_declaration_
  - _constant_declaration_
- _Types_
  - _scalar_type_
  - _array_type_
- _Statements_
  - _simple_statement_
    - _assignment_
    - _print_statement_
    - _read_statement_
  - _conditional_statement_
  - _function_call_statement_
  - _loop_statement_
    - _while_statement_
    - _for_statement_
  - _return_statement_
  - _compound_statement_
- _Expressions_
  - _literal_constant_
    - _integer_literal_
    - _real_literal_
    - _string_literal_
    - _boolean_literal_
  - _variable_reference_
  - _function_call_
  - _binary_operation_
  - _unary_operation_
  - _( expression )_

## What is the hardest you think in this project

I think the most difficult part is error checking, as there are many complex syntactic definitions. After completing the work, I spent a lot of time identifying errors. I not only needed to check the syntax parser but also had to revisit the lexical scanner for potential mistakes. I encountered a particularly frustrating issue where only the type `string` was unmatched. After a long search, I realized that I had defined one of the conditions in the lexical scanner as "**STRING**," which conflicted with the `string` type in the syntax parser, which was also named "**STRING**." This naming conflict caused the matching issues I was experiencing. Overall, this was the most challenging part for me.

## Feedback to T.A.s

The assignment is designed in a way that makes it relatively easy to debug, which I think is great. =D
