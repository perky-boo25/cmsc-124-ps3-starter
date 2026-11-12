# Environments and the Toy Evaluator

**Form traced:** `(let ((y 10)) ((lambda (x) y) 5))` → `10`

## Trace

1. **`eval-expr` on the whole form, env `()`.** It is not a number or symbol, so it is a cons. `(car expr)` is `let`, so it calls `eval-let`.
2. **`eval-let`.** The init `10` evaluates to `10`. `extend-env` builds `((y . 10))`, and the body is evaluated in it.
3. **`eval-expr` on the body.** It is a cons, but `(car expr)` is a list, so `case` falls to `otherwise`. That branch evaluates the operator and the operands, then calls `apply-closure`.
4. **`eval-expr` on `(lambda (x) y)`.** The `car` is `lambda`, so `make-closure` runs with env `((y . 10))` and returns `(:closure (x) y ((y . 10)))`. The operand `5` evaluates to `5`.
5. **`apply-closure`.** It extends the saved env to `((x . 5) (y . 10))` and evaluates `y` in it.
6. **`eval-expr` on `y`.** It is a symbol, so `lookup` finds `(y . 10)` and returns `10`.

## Answer

**Capture line** (the `lambda` branch of `eval-expr`):

```lisp
(lambda (make-closure (second expr) (third expr) env))
```

`env` is where the `lambda` was written, and `make-closure` stores it in the closure.

**Contents at capture:** `((y . 10))`. It holds only `y` from the `let`. It does not hold `x`, which is added at call time, or anything from where the closure is called.