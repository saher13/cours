# Constraint programming

A constraint is a logic formula on a host language, a propertie of solution 
you are looking for.

Useful for optimal resolution of combinatory problems, schedulers, placement.

Program use constraints to model a problem. Interpreter use solver 
to find a solution, but most of solver are incomplete.

Coder has to choose a good constraints system, implement input and output, 
and understand consequences of *solvers* lacks.

## Arithmetical constraints on R

Let host language be First Order Logic (F: functions, constants, P: predicate).
We define:
* A simple constraint: predicate applied to Termes.
* A constraint: conjonction of simples constraints (i.e. a logical fomula) 
  without negation, disjunction or quantifier.
* Special constraints: true (empty conjunction), false

Let D be the constraint domain.    

A **system of constraints** is given by F, P, D and an interpretation of symbols 
in F and P.

An **affectation** is a partial function from variables to constraint domain.
An affectation &T violate a simple constraint if it falsify it.
&T violate a constraint if it violate one of these simple constraints at least.
&T is called **consistent** for a contraint if it does not violate it.
A solution is an **total** affectation which is **consistent**.

A constraint is satisfiable if it has one solution at least.

Simple constraints order is important, since algorithms may vary depending on it.

For C = c_1 &and; c_2 &and; ... &and; c_k, we define set(C) = {c_1, ..., c_n}.
A constraint c_i implicate c_j if any solution of c_i is a solution for c_j as 
well.

Two constraints are logically equivalent if they share the same solution set.

A constraints satisfaction problem (CSP) if define as:

* data: problem variables, with a domain and a constraint *c* for each of them.
* questions: is *c* satisfiable? If so, give a solution.

We use a solver to answer these questions.

A formula is in resolved form if:
x_1 = t_1 &and; ... &and; x_n = t_n 
| &forall; i, j (i &ne; j &rArr;  x_i &nequiv; x_j &and; x_i &notin; V(t_j)

Any resolved form is satisfiable in R. We call x_1, ..., x_n determined 
variables, and we can choose any undetermined variable.

Resolution algorithm is given by transformation rules. You apply them while 
we can, without any special order. We return the resulting constraint.

Rules are:
* Choose a not normalized equation and normalize it.
* If an equation is c_i = c_y, where c_i and c_j are different, replace by 
  &perp;.
* Remove c = c form equations.
* If a normalized equation is x = t with x &notin; V(t), replace x by t in 
  every equation.

Apply a rule product an equivalent constraint. A rule suite always end and 
product &perp; or a resolved form.

## Constraints on finite domain

A constraint c, dealing with variables x_1, ..., x_n . For each variable x_i, 
we define a domain D(x_i). The implicit constraint is 
c &and; x_1 &isin; D(x_1) &and; ... &and; x_n &isin; D(x_n).
A constraint with simple constraints containing 2 variable at most is called 
binary constrainte. With it, we can draw a constraint tree.

### Solver generates and tries

* For each variable, enumerate each value one by one.
* When each variable has a value, test if constraint is satisfied

Very inefficient. Can be improved by testing each time if a partial 
affectation make a constraint false.

### Simple solver using backtracking

* For each variable, enumerate each value one by one.
* For each, check that no simple constraint is false.
* When each variable has a value, test if constraint is satisfied

parsat(C) = &forall closed simple constraint c, c = true

backsolve(C, D) =
* if var(C) = &empty; then return parsat(C)
* pick x in D(x)
* for each d in D(x)
  * &foreach; c = constraint where x & larr; d:
    if parsat(c) return backsolve(c)

### Constraint simplification

The idea is to find a CSP equivalent to the original CSP with smaller variable 
domains.

Node consistency: (var(c) = {x}): remove any value from D(x) which 
make simple constraint c not satisfiable.

Edge consistency: (var(c) = {x, y}): remove each value from D(x) for which 
there is no value in D(x) satisfying the simple constraint c and vice-versa.

#### Node constistancy

A simple constraint is node consitant according to domain D, if:
* |var(c)| &neq; 1
* var(c) = {x} &and; &forall; d &isin; D(x), {x <- d} is solution of c

A CSP is node-consistent if every simple constraint is node-consistent.

Obtaining a node-consistent CSP:
```
nodecons(C, D) =
    &forall; simple constrainte c &isin; C:
        D <- simpnodecons(c, D)
    return D

simpnodecons(c, D):
    if |variable(c)| = 1 then
        let {x} = var(c) in
        D(x) <- {d &isin; D(x) | {x <- d} is solution of c}
    return D
```

#### Edge consistency

A simple constraint is edge-consistent if:
* |var(c)| &neq; 2
* var(c) = {x, y}
  &and; (&forall; d &isin; D(x),
         &exist; e &isin; D(y) | {x <- d, y <- e} is solution of c)
  &and; (&forall; d &isin; D(y),
         &exist; e &isin; D(x) | {x <- d, y <- e} is solution of c)

A CSP is edge-consistent is each simple constraint it contains is edge-consistent.

Obtaining a node-consistent CSP:
```
edgecons(c, D) =
    do
        W <- D
        &forall; simple constraint c &isin; C
            D <- simpedgecons(c, D)
    until W = D
    return D

simpedgecons(c, D) =
    if |var(c)| = 2 then
        D(x) <- {d &isin; D(x)
                 | &exist; e &isin; D(y) &and; {x <- d, y <- e} is solution of c}
        D(y) <- {d &isin; D(y)
                 | &exist; e &isin; D(x) &and; {x <- d, y <- e} is solution of c}
    return D

simpnodecons(c, D):
    if |variable(c)| = 1 then
        let {x} = var(c) in
        D(x) <- {d &isin; D(x) | {x <- d} is solution of c}
    return D
```

#### Using node and edge-consistency

Two types of important domains:
* *false* domain: a variable with empty domain
* *simple* domain: each variable has a domain whose cardinal is 1

We suppose to have a test `satisfiable(C, D)` to test CSP with simple domains.

```
(incomplete) solver:
    D <- nodecons(C, D)
    D <- edgecons(C, D)
    if D is false domain then return false
    else if D is simple domain then return satisfiable(C, D)
    else return dunno 
```
To complete this solver, we need to combine node-edge-consistency and 
backtracking: apply node-edge-consistency before running backtracking solver
**AND** each time a variable is affected by solver.

### Heuristic

We can choose different strategy to select which variable/value to affect.

* Variable:
  * Pick the variable with less legal values.
  * In case of equalty, choose the variable wich has more constraints
    with remaining variables.

* Value:
  Pick the value imposing less constraints on remaining variables.

### Bound consistency

Arithmetic CSP: constraints applied to integers. The idea is to use consistency 
to study bounds of each variable's domain.

A simple constraint c is **bound-consistent** if:
&forall; x &isin; var(c): 
* &exist; d_1, ..., d_k for other variables x_1, ... , x_k |
  * &forall; j min(D(x_j)) <= d_j <= max(D(x_j))
  * {x <- min(D(x)), x_1 <- d_1, ..., x_k <- d_k} is solution of c
* &exist; d_1', ..., d_k' for other variables x_1, ... , x_k |
  * &forall; j min(D(x_j)) <= d_j' <= max(D(x_j))
  * {x <- max(D(x)), x_1 <- d_1', ..., x_k <- d_k'} is solution of c

A CSP is bound-consistent if all of its simple constraints are.

#### Get a bound-consitent CSP

With a given domain D, use propagation rules to modify bounds. Example:
* X = Y + Z
  * X = Y + Z &and; Y = X - Z &and; Z = X - Y
  * X >= min(D(Y)) + min(D(Z)) &and; X <= max(D(Y)) + max(D(Z)) 
  * Y >= min(D(X)) - max(D(Z)) &and; Y <= max(D(X)) + max(D(Z)) 
  * Z >= min(D(X)) - max(D(Z)) &and; Z <= max(D(X)) + max(D(Y)) 

##### Inequalities aX + bY <= c

aX + bY <= c &lArr; X <= (c/a) - (b/a) * min(D(Y)) (using floor function)

##### Inequalities X &neq; Y

Inequalities produce few propagation rules: only if one side has a fixed value,
wich is equal to max or min of the other side.

##### Multiplication X = Y * Z

If every variable is positive:
X >= min(D(Y)) * min(D(Z)) &and; X <= max(D(Y)) * max(D(Z))

Else:
let S = {min(D(Y)) * min(D(Z)); min(D(Y)) * max(D(Z)); max(D(Y)) * min(D(Z)); 
max(D(Y)) * max(D(Y)) * max(D(Z))} in
X >= min(S) &and; X <= max(S)

About Y and Z:
if min(D(Z)) < 0 &and; max(D(Z)) > 0, no restriction for Y
You "wait" until D(Z) become non-negative or non-positive and then use:
Y >= min{min(D(X)) / min(D(Z)); min(D(X)) / max(D(Z)); max(D(X)) / min(D(Z));
max(D(X)) / max(D(Z))}
(Be careful about division by zero)

### Bound-consistent solver

boundcons(C, D): apply propagation rules for each simple constraint in C, until 
there is no change anymore in domains.
Do not re-examinate a simple constraint if none of its variables domain's 
changed.
```
D <- bornecons(C, D)
if D is false domain, then return false
else if D is simple domain, then return satisfiable(C, D)
else return uknown
```

With backtracking: apply bornecons before running the backtracking solver 
**AND** each time a variable is affected by backtracking solver.