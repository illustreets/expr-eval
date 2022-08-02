
Operators & Functions
===============================

### Expression Syntax ###

The parser accepts a pretty basic grammar. It's similar to normal JavaScript
expressions, but is more math-oriented. For example, the `^` operator is
exponentiation, not xor.

#### Operator Precedence

Operator                 | Associativity | Description
:----------------------- | :------------ | :----------
(...)                    | None          | Grouping
f(), x.y, a[i]           | Left          | Function call, property access, array indexing
!                        | Left          | Factorial
^                        | Right         | Exponentiation
+, -, not, sqrt, etc.    | Right         | Unary prefix operators (see below for the full list)
\*, /, %                 | Left          | Multiplication, division, remainder
+, -, \|\|               | Left          | Addition, subtraction, array/list concatenation
==, !=, >=, <=, >, <, in | Left          | Equals, not equals, etc. "in" means "is the left operand included in the right array operand?"
and                      | Left          | Logical AND
or                       | Left          | Logical OR
x ? y : z                | Right         | Ternary conditional (if x then y else z)
=                        | Right         | Variable assignment
;                        | Left          | Expression separator

#### Unary operators

The parser has several built-in "functions" that are actually unary operators.
The primary difference between these and functions are that they can only accept
exactly one argument, and parentheses are optional. With parentheses, they have
the same precedence as function calls, but without parentheses, they keep their
normal precedence (just below `^`). For example, `sin(x)^2` is equivalent to
`(sin x)^2`, and `sin x^2` is equivalent to `sin(x^2)`.

The unary `+` and `-` operators are an exception, and always have their normal
precedence.

Operator | Description
:------- | :----------
-x       | Negation
+x       | Unary plus. This converts it's operand to a number, but has no other effect.
x!       | Factorial (x * (x-1) * (x-2) * … * 2 * 1). gamma(x + 1) for non-integers.
abs x    | Absolute value (magnitude) of x
acos x   | Arc cosine of x (in radians)
acosh x  | Hyperbolic arc cosine of x (in radians)
asin x   | Arc sine of x (in radians)
asinh x  | Hyperbolic arc sine of x (in radians)
atan x   | Arc tangent of x (in radians)
atanh x  | Hyperbolic arc tangent of x (in radians)
cbrt x   | Cube root of x
ceil x   | Ceiling of x — the smallest integer that’s >= x
cos x    | Cosine of x (x is in radians)
cosh x   | Hyperbolic cosine of x (x is in radians)
exp x    | e^x (exponential/antilogarithm function with base e)
expm1 x  | e^x - 1
floor x  | Floor of x — the largest integer that’s <= x
length x | String or array length of x
ln x     | Natural logarithm of x
log x    | Natural logarithm of x (synonym for ln, not base-10)
log10 x  | Base-10 logarithm of x
log2 x   | Base-2 logarithm of x
log1p x  | Natural logarithm of (1 + x)
not x    | Logical NOT operator
round x  | X, rounded to the nearest integer, using "grade-school rounding"
sign x   | Sign of x (-1, 0, or 1 for negative, zero, or positive respectively)
sin x    | Sine of x (x is in radians)
sinh x   | Hyperbolic sine of x (x is in radians)
sqrt x   | Square root of x. Result is NaN (Not a Number) if x is negative.
tan x    | Tangent of x (x is in radians)
tanh x   | Hyperbolic tangent of x (x is in radians)
trunc x  | Integral part of a X, looks like floor(x) unless for negative number

#### Pre-defined functions

Besides the "operator" functions, there are several pre-defined functions. You
can provide your own, by binding variables to normal JavaScript functions.
These are not evaluated by simplify.

Function      | Description
:------------ | :----------
random(n)     | Get a random number in the range [0, n). If n is zero, or not provided, it defaults to 1.
fac(n)        | n! (factorial of n: "n * (n-1) * (n-2) * … * 2 * 1") Deprecated. Use the ! operator instead.
min(a,b,…)    | Get the smallest (minimum) number in the list.
max(a,b,…)    | Get the largest (maximum) number in the list.
hypot(a,b)    | Hypotenuse, i.e. the square root of the sum of squares of its arguments.
pyt(a, b)     | Alias for hypot.
pow(x, y)     | Equivalent to x^y. For consistency with JavaScript's Math object.
atan2(y, x)   | Arc tangent of x/y. i.e. the angle between (0, 0) and (x, y) in radians.
roundTo(x, n) | Rounds x to n places after the decimal point.
map(f, a)     | Array map: Pass each element of `a` the function `f`, and return an array of the results.
fold(f, y, a) | Array fold: Fold/reduce array `a` into a single value, `y` by setting `y = f(y, x, index)` for each element `x` of the array.
filter(f, a)  | Array filter: Return an array containing only the values from `a` where `f(x, index)` is `true`.
indexOf(x, a) | Return the first index of string or array `a` matching the value `x`, or `-1` if not found.
join(sep, a)  | Concatenate the elements of `a`, separated by `sep`.
if(c, a, b)   | Function form of c ? a : b. Note: This always evaluates both `a` and `b`, regardless of whether `c` is `true` or not. Use `c ? a : b` instead if there are side effects, or if evaluating the branches could be expensive.

#### Array literals

Arrays can be created by including the elements inside square `[]` brackets, separated by commas. For example:

    [ 1, 2, 3, 2+2, 10/2, 3! ]
