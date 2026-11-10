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
`(+ 1 (* 2 3))` is a compound form headed by the function `+`, so it evaluates its arguments first before performing the operation. The inner multiplication returns `6`, then `+` adds `1` to it, giving us `7`, which is an integer with length `none`.

## P02
`'(+ 1 2)` is shorthand for `(quote (+ 1 2))`, where `quote` is a `special-operator` that returns the expression as it is without evaluating it. We get the list `(+,1,2)`, with kind `cons` and length `3`.

## P03
`nil` is an atomic form that simply evaluates to itself, so it doesn't have an operator. It's also a special case in this language since `nil` represents both false and an empty list, giving it kind `null` and length `0`.

## P04
`t` is a self-evaluating symbol that represents true, so it also has no operator and just returns itself. Since it's a non-nil symbol, its kind is `symbol` and its length is `none`.

## P05
`(symbolp nil)` uses the `symbolp` function to check whether `nil` is considered a symbol, evaluating its argument first. Even though `nil` also represents an empty list, it's still a symbol in CLISP, so the result is `t` with kind `symbol` and length `none`.

## P06
`(car '())` uses the `car` function to get the first element of a quoted empty list, which evaluates to `nil`. Since getting the `car` of `nil` also returns `nil` in CLISP, the result has kind `null` and length `0`.

## P07
`(cons 1 '(2 3))` uses the `cons` function, which evaluates its arguments first before adding `1` to the beginning of the existing list `(2 3)`. This gives us `(1 2 3)`, with kind `cons` and length `3`.

## P08
`(if (null '()) 'empty 'full)` uses the `if` special operator to check the condition first and only evaluate the branch that matches the result. Since the condition returns `t`, only `'empty` gets evaluated, giving us the symbol `empty` with kind `symbol` and length `none`.

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
