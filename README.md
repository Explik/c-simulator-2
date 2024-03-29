# C simulator (v2)

## Example program
This program prints out a pyramid of numbers from 0 to the value of n (in this case 7) and then back down to 0. Each row of the pyramid corresponds to a loop iteration of the outer for loop, and each number on a row corresponds to a loop iteration of the inner for loop. The variable step is used to control the direction in which the inner loop iterates. The printf function is used to print out the numbers.

```C
#include <stdio.h>

int main() {
    int n = 7;
    int step = 1 & 1;

    for (int i = 0; 0 <= i && i <= n; i += step) {
        for (int j = 0; j <= i; ++j) {
            printf(" %d", j);
        }
        printf("\n");
        if (i == n) {
            step = -1;
        }
    }

    return 0;
}
```

This program can be converted to JS-based syntax tree using the functions located in tree module. 

```js
const i = identifier('i');
const j = identifier('j');
const n = identifier('n');
const printf = identifier('printf');
const step = identifier('step');
const includeStdio = includeDirective('stdio.h');

const root = [
  includeStdio,
  functionDeclaration('int', 'main', [], block(
    intDeclaration(n, intConstant(7)),
    intDeclaration(step, intConstant(1)),
    forLoop(
      intDeclaration(i, intConstant(0)),
      expressionStatement(and(lessThanOrEqual(intConstant(0), i), lessThanOrEqual(i, n))),
      expressionStatement(addAssign(i, step)),
      block(
        forLoop(
          intDeclaration(j, intConstant(0)),
          expressionStatement(lessThanOrEqual(j, i)),
          expressionStatement(increment(j)),
          block(
            expressionStatement(invoke(printf, stringConstant(" %d"), j))
          )
        ),
        expressionStatement(invoke(printf, stringConstant("\n"))),
        iff(equal(i, n), expressionStatement(assign(step, intConstant(-1))))
      )
    )
  ))
];
```

## Node types
Identifier: ``{ type: 'label', name: string }`` 
Constant: ``{ type: 'constant', value: string, datatype: string }``

Binary expression: ``{ type: 'binary-expression', operator: string, left: node, right: node }``
Unary expression: ``{ type: 'unary-expression', operator: string, argument: node, prefix: boolean }``
Call expression: ``{ type: 'call-expression', callee: node, arguments: node[] }``
Conditional expression: ``{ type: 'conditional-expression', test: node, consequent: node, alternate: node }``
Assignment-expression: ``{ type: 'assignment-expression', operator: string, left: node, right: node }``
Block statement: ``{ type: 'block-statement', body: node[] }``

Variable declaration: ``{ type: 'variable-declaration', kind: string, declarations: node[] }``
Function declaration: ``{ type: 'function-declaration', return-type: string, name: node, params: node[], body: node }``
Return statement: ``{ type: 'return-statement', argument: node }``
If statement: ``{ type: 'if-statement', test: node, consequent: node, alternate: node }``
For statement: ``{ type: 'for-statement', init: node, test: node, update: node, body: node }``
