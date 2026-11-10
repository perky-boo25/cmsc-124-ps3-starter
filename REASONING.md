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
Replace this line with the rule behind this case's four fields.

## P02
Replace this line with the rule behind this case's four fields.

## P03
Replace this line with the rule behind this case's four fields.

## P04
Replace this line with the rule behind this case's four fields.

## P05
Replace this line with the rule behind this case's four fields.

## P06
Replace this line with the rule behind this case's four fields.

## P07
Replace this line with the rule behind this case's four fields.

## P08
Replace this line with the rule behind this case's four fields.

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
