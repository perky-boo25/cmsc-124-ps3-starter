# Reasoning for the Predictions Table

Write one entry under every heading below, then commit this file together with
`predictions.tsv`, before you run a case or open `tests/expected.tsv`.

Each entry names the rule that produces the four predicted fields for that
case. One to three sentences is enough. The grader checks structure only. It
does not judge quality, and it sets no word minimum.

After you run the cases, add a short correction under each case you predicted
wrongly. Say what you predicted, what CLISP printed, and which rule you
misread. Those entries belong in the correction commits.

## P01
`(+ 1 (* 2 3))` heads with the function `+`, so its arguments are evaluated before the call. The inner `(* 2 3)` is also a function call that returns the integer 6, so `+` receives 1 and 6 and returns 7. Seven is an integer, so the length is none.

## P02
`'(+ 1 2)` is `(quote (+ 1 2))`, so its head is the special operator `quote`, which returns its argument unevaluated. The value is the list `(+,1,2)`, which is a nonempty list, so the kind is cons and the length is 3.

## P03
`nil` is an atom, so it evaluates to itself and has no operator. Its kind is null (the course reserves null for nil only), and since nil is the empty sequence, its length is 0.

## P04
`t` is an atom, so it evaluates to itself and has no operator. It is a symbol that is not nil, so its kind is symbol, and a symbol is not a sequence, so the length is none.

## P05
`(symbolp nil)` heads with the function `symbolp`, which checks whether its argument is a symbol. `nil` is a symbol (as well as the empty list and false), so the result is `t`. `t` is a symbol, so the kind is symbol and the length is none.

## P06
`(car '())` heads with the function `car`, which returns the first element of a list. The argument `'()` is the empty list, which is the same object as nil, and `car` of the empty list is nil. The value is `nil`, the kind is null, and the length is 0.

## P07
`(cons 1 '(2 3))` heads with the function `cons`, which builds a new list with 1 in front. Its arguments are evaluated left to right: `1` is an integer and `'(2 3)` is a plain list. The result is `(1,2,3)`, a nonempty list, so the kind is cons and the length is 3.

## P08
`(if (null '()) 'empty 'full)` heads with the special operator `if`, which evaluates the condition first and then only one branch. `'()` is nil, so `(null '())` is true and `if` evaluates `'empty`, which returns the symbol `empty`. The kind is symbol and the length is none.

## P09
`(and 1 nil 3)` heads with `and`. CLISP implements `and` as both a special operator and a macro, and the course checks special operator first, so the operator is special-operator. It evaluates left to right and stops at the first nil, so the value is `nil`, the kind is null, and the length is 0.

## P10
`(progn ...)` heads with the special operator `progn`, which evaluates its subforms in order and returns the value of the last one. `defvar` makes `*ps3-rate*` a special variable, so the `let` rebinds it to 10 while `(ps3-scale 3)` runs. The function body `(* n *ps3-rate*)` then becomes `(* 3 10)`, which is 30. Thirty is an integer, so the length is none.

## P11
`(defun ps3-twice (n) (* n 2))` heads with `defun`, which is a macro. Nothing is called, because `defun` only defines the function, and a definition returns the function's symbol. The value is `ps3-twice`, the kind is symbol and the length is none.

## P12
`(labels ...)` heads with the special operator `labels`, which defines `down` and then evaluates `(down 3)`. Each call conses `n` onto the result of `down` with `n - 1`, until `n` is 0 and the base case returns `'()`, which is nil. That nil ends the chain without adding an element, so the value is `(3,2,1)`, a nonempty list. The kind is cons and the length is 3.

## P13
`(mapcar #'length '((1 2) () (3)))` heads with the function `mapcar`, which applies `length` to each element of the list and collects the results. The elements have lengths 2, 0, and 1, so the value is `(2,0,1)`. That is a nonempty list, so the kind is cons and its length is 3.

## P14
`(mapcar (lambda (n) (* n n)) '(1 2 3))` heads with the function `mapcar`, which applies the lambda to each element and collects the results. Squaring 1, 2, and 3 gives the value `(1,4,9)`. That is a nonempty list, so the kind is cons and the length is 3.

## P15
`(lambda (n) (* n 2))` heads with `lambda`, which is a macro and not one of the six CLISP also treats as special operators. Nothing calls it, so evaluating it just creates a function object. The value is `#<function>`, the kind is function, and the length is none.

## P16
`(let ((factor 2)) ...)` heads with the special operator `let`. `factor` is a lexical variable, so the lambda captures the `factor` bound to 2 where it was written. The inner `let` binds a different `factor` as 10, but `f` never sees it. `(funcall f 3)` therefore computes `(* 3 2)`, so the value is 6. Six is an integer, so the length is none.