## Function Calling

### Implicit left-associative function calls

```jison
line
  : callable eol
  ;

callable
  : S
  | call
  ;

call
  : callable arg
  | callable args
  ;

args
  : arg COMMA arg
  | args COMMA arg
  ;

arg
  : S
  ;

S: ID;
```

Examples

- `c d -> a(b)`
- `a b c -> a(b)(c)`
- `a b,c d,e,f -> a(b,c)(d,e,f)`

### Implicit right-associative function calls

```jison
line
  : O eol { console.log($1) }
  ;

O
  : S S
  | S L
  | S O
  ;

L : S COMMA O
  | S COMMA L
  | S COMMA S
  ;

S : ID;
```

Examples

- `a b -> a(b)`
- `a b,c -> a(b,c)`
- `a b c -> a(b(c))`
- `a b,c,d e,f,g h,i,j,k -> a(b,c,d(e,f,g(h,i,j,k)))`


Amendment to account for brackets

```jison
line
  : O eol
  ;

O
  : S S
  | S L
  | S O
  | S OP S CP
  | S OP L CP
  ;

L : S COMMA O
  | S COMMA L
  | S COMMA S
  ;

S : ID
  | OP O CP
  ;
```

## If-Then-Else

### Right Associative with Optional Else

```jison
line
  : IF eol
  ;

IF
  : C
  | O
  ;

S
  : ID
  | OP IF CP
  ;

O
  : S QUERY O
  | S QUERY C
  | S QUERY C COLON O
  ;

C
  : S QUERY C COLON C
  | S
  ;
```
