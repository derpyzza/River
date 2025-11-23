Language Grammar
===
the grammar of river. currently a mess but will get better with time.

# Notation:

rules are in snake_case, and are defined with the following pattern:

```
rule -> production | ( production | production );
```

the `|` character means `or`, and rules can be grouped via parenthesis for clarity.
rules are terminated with the `;` character.

terminals are wrapped in quotes `"import"`.

production rules can be followed up with any of the following operators:
```
*: Needs zero or more of
+: Needs one or more of
?: Either one or zero of
```

```

iden -> [a-zA-Z_]+ ;
number -> int | float ;


program -> item* eof;

item ->
      | import_decl
      | const_decl
      | var_decl
      | type_decl
      | struct_decl
      | enum_decl
      | union_decl
      | fn_decl
      ;

import_decl ->
      "import" iden ( "." iden )* ( "as" iden )? ";" ;

func_decl ->
      | "pub"? "fun" iden "(" params_list? ")" ( ":" type )? "{" ( statement* expr? ) "}"
      | "pub"? "fun" iden "(" params_list? ")" ( ":" type )? "=" expr ";"
      ;

a: x,
b: y,
c, d, e: z = 1, 2, 3
c = 1, d = 2, e = 3: z = 1, 2, 3

params_list -> param_item ( "," param_item )*;
param_item -> iden ( "," iden )* ":" type ( "=" lit )?

data_type -> <primary_type> ( "," <primary_type> )* ;
primary_type -> <type_header> iden ;
type_header -> "^"* type_arr* ;
type_arr -> ("[" %number? "]")* "^"* ;

var_decl ->
          | ("let" | "var") iden ":" data_type ";"
          | ("let" | "var") iden ( ":" data_type )? "=" LITERAL ";"
          | ("let" | "var") iden ( "," iden )+ ":" data_type ";"
          | ("let" | "var") iden ( "," iden )+ ":" data_type  "=" LITERAL ( "," LITERAL )+ ";"
          ;

const_decl -> "const" iden "=" <expr>;

type_def ->
      | "type" iden "=" data_type ";"
      | "type" iden "=" (struct_decl | union_decl | enum_decl)
      ;

struct_decl -> "struct" "{" ( struct_field ";" )+ "}";
struct_field -> iden ("," iden)* ":" type string? ;

union_decl -> "raw"? "union" "{" ( union_field "," )+ "}";
union_field -> iden ":" type string? ;
enum_decl -> "enum" ( ":" (iden ',')* iden ) "{" (iden ",")* "}";

type -> ptr* arr iden
      | iden ( ',' iden )*
      ;

statement -> expr ";";

stmt -> expr_stmt
      | decl_stmt;

expr_stmt -> expr ";" ;
decl_stmt -> ( 'let' | TYPE ) ID "=" expr;

block_expr -> "{" (( stmt* expr? ) | expr) "}" ; 

expr -> "{" ( assignment ";"? | assignment )+ "}" | assignment;
assignment -> ID "=" assignment | lor ;

lor -> land ( "||" land )* ;
land -> eq ( "&&" eq )* ;
eq -> comp ( ( "!=" | "==" ) comp )* ;
comp -> shift ( ( ">" | ">=" | "<" | "<=" ) shift )* ;
shift -> add ( (">>" | "<<") add)*

add -> mult ( ('+'|'-') mult )*;
mult -> unary ( ('*' | '/' | '%') unary)*;
unary -> primary | ( ('-' | '!') primary);

primary 
      -> "(" expr ")"
      | INT 
      | FLOAT
      | STRING
      | CHAR
      | ARRAY_LIT
      | "true" 
      | "false" 
      | TUPLE_LIT 
      | STRUCT_LIT;

```
