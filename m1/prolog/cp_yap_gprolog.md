# Constraint programming with gprolog

Incomplete solver based on edge-consistency and bound-consistency. 
Maximum (and default) domain is [O,fd_max_integer] 
Domain may be represented by intervals (default) or set 
(limited to vector_max).

A `@` in solver response means that you lost values.

## Basic predicats

* `fd_domain(?Vars, +Integer1, +Integer2)`: define domain of a variable (or 
a list of variables) Vars as [Integer1, Integer2]
* `fd_domain(?Vars, +IntList)`: same with a list of values
* `fd_all_different(?VarList)`: impose constraint that every value is 
different thant others.

## Arithmetic constraint

* `#= #\= #< #> #=< #>=`: use bound consistency
* `#=# #\=# #<# #># #=<# #>=#`: use edge consistency

## Labelling

`fd_labeling(?Vars, ?Options)`: search for solution in Vars with options
Example: `[variable_method(most_constrained)]` option -> choosen variable is 
the one the most constrained.

## Other

`fd_element(N, L, X)`: force X to be the Nth element of list L (start with 1)

`fd_cardinality(+L, ?N)`: inject into V the number of constraints in L 
satisfied.

Also exist `fd_at_least_one(L)`, `fd_at_most_one(L)` and `fd_only_one(L)`.

# YAP

Allow to use reals constraints with `clpr` librairie:
* F = {+, -, *, /, sin, cos, tan, ...}
* P = {=, <, >, =<, >=, =/=, ...}
* D = real

Do not forget `(use_module(library(clpr)))`.
Constraint are written between `{}`
Comma means logical conjunction.