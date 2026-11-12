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

**Dynamic Scope Alternative:**

In `apply-closure`, we evaluate the body using `(eval-expr body (extend-env params args saved-env))`, where `saved-env` is the environment captured when the closure was created.

If we extend the caller's environment instead, evaluating `(f 1)` inside the scope-discrimination case:

```lisp
(let ((x 2))
  (let ((f (lambda (y) (+ x y))))
    (let ((x 100))
      (f 1))))
```

This would put `(y . 1)` at the front of the caller's environment, where `(x . 100)` is already located. When `eval-expr` evaluates `(+ x y)`, `lookup` (`assoc`) would find `(x . 100)` before `(x . 2)`, returning `101` instead of `3`.

This would make our toy language dynamically scoped instead of lexically scoped.

**Why CLISP Needs `funcall`:**

Our toy evaluator is a Lisp-1, meaning it uses one shared environment `env` for both numbers and closures. In `eval-expr`, the `otherwise` branch evaluates `(eval-expr (car expr) env)`, so `(f 1)` directly looks up `f` in `env`.

However, CLISP is a Lisp-2, meaning it has separate namespaces for functions and values. Because of this, calling a closure stored in a variable requires `funcall`, while passing a named function requires `#'`.
