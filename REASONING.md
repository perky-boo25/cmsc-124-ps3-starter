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
`(and 1 nil 3)` evaluates everything from left to right but stops as soon as it encounters `nil`, so `3` never gets evaluated. The result is `nil` with kind `null` and length `0`, and its operator is `special-operator` because CLISP checks `special-operator-p` before `macro-function`.

## P10
`(progn ...)` uses the `progn` special operator to execute its expressions one after another and return the value of the last one. Since `defvar` makes `*ps3-rate*` a special, dynamically scoped variable, the `let` binding temporarily changes its value to `10`, making `(ps3-scale 3)` return `30` with kind `integer` and length `none`.

## P11
`(defun ps3-twice (n) (* n 2))` uses the `defun` macro to create a function named `ps3-twice`, but what it returns is the function's name as a symbol. Its kind is `symbol` and its length is `none`.

## P12
`(labels ((down (n) ...)) (down 3))` uses the `labels` special operator to create a local recursive function that keeps adding the current value of `n` to the list until it reaches `0`. This gives us `(3 2 1)`, with kind `cons` and length `3`.

## P13
`(mapcar #'length '((1 2) () (3)))` uses the `mapcar` function to apply the `length` function to each list and collect their results, while `#'length` gets the function from CLISP's function namespace. We get `(2 0 1)`, which has kind `cons` and length `3`.

## P14
`(mapcar (lambda (n) (* n n)) '(1 2 3))` uses the `mapcar` function to apply an anonymous function that multiplies each number by itself. The resulting list is `(1 4 9)`, with kind `cons` and length `3`.

## P15
`(lambda (n) (* n 2))` uses the `lambda` macro, which expands into a function form when evaluated. Instead of running the multiplication immediately, it creates an anonymous function object written as `#<function>` in grader notation, with kind `function`, length `none`, and operator `macro`.

## P16
`(let ((factor 2)) ...)` uses the `let` special operator to create a lexical binding for `factor`, which the function `f` captures when it is defined. Even if another `let` creates a separate `factor` with the value `10`, `f` still uses the original value of `2`, so calling `(funcall f 3)` gives us `6` with kind `integer` and length `none`.
