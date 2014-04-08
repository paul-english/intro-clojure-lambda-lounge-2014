---
author: Paul English
title: Introduction to Clojure
---

# Intro to Clojure*

## Getting started, foundational ideas, and the core library

---

# Topics to Cover

<br>

0. Clojure Semantics
1. Prefix Notation
2. The Positioning of Parenthesis
3. The Elimination of Parenthesis

---

# Topics to Cover

<br>

0. Clojure Semantics
1. ~~Prefix Notation~~
2. ~~The Positioning of Parenthesis~~
3. ~~The Elimination of Parenthesis~~

---

# So... What's cool about Clojure?

<br>

- It's a *Lisp*
- It's very *Functional*
- It Embraces *Concurrency*
- It's Built to run on *Java* and more recently *Javascript*

---

# How does it compare to Lisp?

<br>

- It's a Lisp-1
- The focus is on sequences, not necessarily lists
- There's some more syntax sugar, which actually leads to fewer
  parenthesis
- There's a whole list of comparisons here <http://clojure.org/lisps>,
  so we won't go into detail about those.

---

# How does it compare to Lisp?

<br>

## A note on some of the similarities

- Functions are first class objects
- Sequences really are lists and they include `lists`, `vectors`,
  `maps`, `arrays`, and many of the objects you work with in Clojure
  are `seq`'able
- We get macros!

---

# What does Clojure do for concurrency?

<br>

- Clojure offers a handful of primitives built-in for handling
  different concurrent situations.
- Java concurrency is a tough problem. Clojure solves this.
- Javascript doesn't really have concurrency, though it does have
  asynchronicity. This is getting better, but in general some of
  Clojure's concurrency ideas help make asynchronicity in JS much
  easier.
- These primitives include `atoms`, `refs`, `agents`, `promises`, and
  the `core.async` library.

---

# Clojure and Clojurescript

<br>

- Clojure is built to run on the JVM and compiles to Java bytecode.
- Clojurescript compiles to pure Javascript.
  -- You can use Clojurescript
  on it's own for a full Javascript app (frontends, Light table)
  -- You can use Clojurescript in combination with a Clojure backend
  and share quite a bit of code.

---

# Cool, let's get on to the basics

<br>

1. forms
2. reader macros
3. functions
4. vars & binding
5. flow

---

# Forms

1. Symbols
2. Literals
3. Lists, Vectors, Maps, Sets
4. Special forms

<br>

## Further Reading
- <http://clojure.org/reader>
- <http://clojure.org/special_forms>

---

# Reader Macros

<br>

- Reader macros give us a bunch of syntactic sugar
- They're completely optional, though they tend to improve the
  conciseness and readability of code

---

# Reader Macros

<br>

<!-- Not including table shorthand in markdown was a huge mistake -->
<style>
table {
font-size: 0.5em;
}
table th {
text-align: left;
font-weight: bold;
padding-right: 55px;
}
table td, th {
padding: 2px;
}
</style>
<table>
<tr>
<th> <b>Anonymous Function</b> </th>
<td> <code>#(str "Hello " %)</code> </td>
</tr>
<tr>
<th> <b>Comment</b> </th>
<td> <code>; inline comment</code> </td>
</tr>
<tr>
<th> <b>Deref</b> </th>
<td> <code>@form => (deref form)</code> </td>
</tr>
<tr>
<th> <b>Meta</b> </th>
<td> <code>^form => (meta form)</code> </td>
</tr>
<tr>
<th> <b>Metadata</b> </th>
<td> <code>#^metadata-form</code> </td>
</tr>
<tr>
<th> <b>Quote</b> </th>
<td> <code>'form => (quote form)</code> </td>
</tr>
<tr>
<th> <b>Regex pattern</b> </th>
<td> <code>#"foo" => java.util.regex.Pattern</code> </td>
</tr>
<tr>
<th> <b>Syntax-quote</b> </th>
<td> <code>`x</code> </td>
<!-- ` prevent my editor syntax highlighting -->
<!-- from being annoying -->
</tr>
<tr>
<th> <b>Unquote</b> </th>
<td> <code>~</code> </td>
</tr>
<tr>
<th> <b>Unquote-splicing</b> </th>
<td> <code>~@</code> </td>
</tr>
<tr>
<th> <b>Var-quote</b> </th>
<td> <code>#'x => (var x)</code> </td>
</tr>
</table>

---

# Functions

<br>

Functions are first class objects, and come in various forms

<br>


    (fn [name] (str "Hello " name))

    #(str "Hello " %)

    (defn greeter [name] (str "Hello " name))

---

# Functions

They can be passed around & made use of in other higher-order functions.

<br>


    ;; Use built-in functions with each other
    (map inc [1 2 3 4])
    => [2 3 4 5]

    ;; Make use of anonymous functions within higher order functions
    (map #(rem (* % 2) 10) [20 34 95 19])
    => [0 8 0 8]

    ;; Compose, complement, and more
    (comp not nil?)
    (complement nil?)


---

# Functions

<br>

But you guys already know about functional programming. This is
probably a given for the Lambda Lounge.

<br>

;)

---

# Vars, Binding, & Namespaces

<br>

## Vars

Vars are immutable (mostly) and local to a namespace.

<br>

    (def a-var 42)
    => #'user/a-var

    a-var
    => 42

    (var a-var)
    => #'user/a-var

---

# Vars, Binding, & Namespaces

<br>

## Binding

We can make user of local binding using `let`

<br>

    (let [a-var 42
         another "foo"]
      another)
    => "foo"

    another
    => java.lang.RuntimeException: Unable to resolve symbol: another in this context

---

# Vars, Binding, & Namespaces

<br>

## Destructured Binding

Anywhere binding occurs we can make use of destructuring.

<br>

    ;; Destructuring a vector that's passed in as an argument to a function
    (defn a-cool-fn [[first last]] ...)

    ;; Destructuring a map
    (def coll {:a 1 :b 2 :c 3})
    (let [{a :a b :b} coll] (println a b))

---

# Vars, Binding, & Namespaces

<br>

## Namespaces

Namespaces allow us to organize code, and prevent globally scoped
variables. Filenames and directory structure must match up correctly
to a namespace, otherwise there will be errors. For example the
following namespace would likely be found in `src/my_namespace/core.clj`

<br>

    (ns my-namespace.core
      (require ...)
      (use ...)
      (import ...))

---

# Flow Control

<br>

Control flow in Clojure is similar to most lisps though there may be
some differences.

- We can branch with `if` and it's variants.
- We try to avoid iterative steps (these imply side-effects) though if
  needed we can using `do`. (By convention any function with `do` in the
  name implies side-effects, e.g. `dosync`, `doall`, `dorun`, ...)
- Looping isn't the same as you might expect, rather the function `for` is a list comprehension.
- Recursion, though we'll talk about this in later section.

---

# Flow Control

<br>

    ;; If-Else Structure
    (if predicate-fn
      (left-branch-fn ...)
      (right-branch-fn ...))

    ;; Example
    (if (>= item 5)
      item
      (recur (inc item)))
    => item

---

# Flow Control

<br>

When we want more than one thing to happen.

<br>

    (if true
      (do
       (step-1 ...)
       (step-2 ...)))

    ;; The when macro encapsulates this one-armed if pattern
    (when true
      (step-1 ...)
      (step-2 ...))

---

# Flow Control

<br>

`for` is a list comprehension, and can be very powerful. The
assumption is that you make use of the result, rather than have
side-effects within the looping structure. In the case you have
side-effecty code `doseq` can work well for this.

<br>

    (for [x [0 1 2 3 4 5]
          :let [y (* x 3)]
          :when (even? y)]
       y)
    => [0 6 12]

---

# Sequences

<br>

As expressed earlier, A core concept of Clojure is the sequence. This
abstraction let's us make heavy reuse of the most important sequence
functions on a variety of data structures like `vectors`, `lists`,
`maps`, and even more abstract things like files, database results, or
custom defined objects.

<br>

In Clojure just about everything is a `seq`.

---

# Sequences

<br>

## Things which are Seq'able

- Clojure collections
- Java collections / Javascript collections
- Java arrays & strings
- Regular expression matches
- Directory structures
- IO Streams, XML, Database Results, Your own custom objects...

---

# Sequences

<br>

## Sequence Operations

- *Basics*: `first`, `rest`, `next`
- *Constructing*: `cons`, `conj`, `into`
- *Access*: `take`, `drop`, `nth`
- *Creating*: `range`, `repeat`, `iterate`, `cycle`, `interleave`, `interpose`
- *Creation of Specific Types*: `seq`, `list`, `vector`, `hash-set`, `hash-map`
- *Filtering*: `filter`, `take-while`, `drop-while`
- *Predicates*: `every?`, `some`, `not-every?`, `not-any?`
- *Map & Reduce*: `map`, `map-indexed`, `reduce`,
- *Forced Evaluation*: `doall`, `dorun`
- The list goes on...

---

# Sequences

<br>

We also have sequence operations specific to certain types.

## Vectors
- `peek`, `pop`, `get`, `assoc`, `subvec`

## Maps
- `keys`, `vals`, `get`
- `contains?`, `assoc`, `dissoc`, `select-keys`, `merge`, `merge-with`

## Sets
- `union`, `difference`, `intersection`, `select`
- `rename`, `project`, `join`


---

# Sequences

Using sequence operations on a custom object.

    (defrecord Person [fname lname address])

    (def me (Person. "Paul" "English" "1234 Street"))

    (first me) => [:fname "Paul"]
    (last me) => [:address "1234 Street"]

    (map identity me)
    => ([:fname "Paul"] [:lname "English"] [:address "1234 Street"])


*Note*: A record is close to the concept of an object. Interfaces can
 be defined on records that let a variety of functions perform on
 them. Records by default implement interfaces for working as a
 sequence.

---

# Concurrency

0. Atoms (Uncoordinated, Synchronous)
1. Refs (Coordinated, Synchronous)
2. Agents (Uncoodinated, Asynchronous)
3. Promises
4. Futures
5. core.async

---

# Concurrency

## Atoms

Atoms are uncoordinated synchronous objects. Like their name implies
they're atomic.

<br>

    (def counter (atom 0))

    counter => #<Atom@6ea406d6: 0>

    @counter => 0
    (deref counter) => 0

---

# Concurrency

## Atoms

We use atoms when we want to have some kind of shared state that
changes atomically. They're lightweight and tend to be more common
than some of the other concurrency primitives.

<br>

    (swap! inc counter) => 1
    (swap! inc counter) => 2

    (reset! counter 0) => 0

---

# Concurrency

## Refs

Similar to atoms, but bring in the concept of coordination. We use
transactions to work with them. Therefore anytime you have changes
that need to be done in one atomic transaction, you can use refs.

<br>

    ;; Let's try using a ref as a counter
    (def counter (ref 0))

    ;; Similar method of access `deref`
    @counter => 0

---

# Concurrency

## Refs

<br>

    ;; `ref-set` is like `reset!` but....
    (ref-set counter 1)
    => java.lang.IllegalStateException: No transaction running

    ;; We _must_ have a transaction
    (dosync
      (ref-set counter 1))

---

# Concurrency

## Refs

Just like we can update atoms using `swap!`, for refs we use `alter`
and `commute`. Of course these must happen in a transaction.

<br>

    (dosync (alter counter inc))
    (dosync (commute counter inc))

<br>

Note: `commute` is the same as `alter` only, like the name suggests,
it's for when you don't care about what order the operations may occur
in, i.e. the operations are _commutative_.

---

# Concurrency

## Refs

Because `dosync` creates a transaction, when you perform multiple ref
operations either everything will succeed, or none of it will.

<br>

    @counter => 1

    (dosync
      (alter counter inc)
      (throw (ex-info "Simulated Error" {})))

    ;; It's still the same value
    @counter => 1

---

# Concurrency

## Refs

Refs can optionally be validated.

<br>

    (def current-user (ref {:name "Paul"}
                           :validator #(contains? % :name)))

    (dosync
      (ref-set current-user {}))
    => java.lang.IllegalStateException: Invalid reference state

---

# Concurrency

## Agents

Agents are asynchronous & uncoordinated. Agent operations occur in a
seperate thread than the one you're on.

<br>

    (def counter (agent 0))

    (defn long-running-inc [a]
      (Thread/sleep 5000)
      (inc a))

    (send counter long-running-inc)

<br>

*Note*: Just like a `ref` we can optionally attach validator functions.

---

# Concurrency

## Agents

Because operations happen in a different thread errors are handled differently

<br>

- We use `agent-errors` to view if anything went wrong.
- We use `clear-agent-errors` to clear these problems.

---

# Concurrency

## Promises

<br>

A `promise` is an object you can pass around between threads or
functions. It's an agreement to perform an operation in some other
thread, that can block at some point until it's needed. In this ways
it's somewhat like an agent, though we can wait till it's realized.

---

# Concurrency

## Promises

<br>

    (def p (promise))

    ;; Threading a native Java Thread (-> is sugar)
    (-> (fn []
          (Thread/sleep 5000)
          (deliver p "Long operation finished"))
        (Thread.)
        (.start))

    ;; Try derefing the value before the timer finishes
    @p

---

# Concurrency

## Futures

A future is very similar to a promise (and sometimes easily confused).
The difference is we define the operation on it right away rather than
assuming some other piece of code will `deliver` to it.

<br>

    (def f (future (Thread/sleep 5000) "done"))

    @f

---

# Concurrency

## Core Async Library

The `core.async` library is an additional dependency, and not by
default included in a new project. It's a really great library, and I
think Ben will be covering it more in depth. Some of it's benefits,

- Communicate amongst threads with queue-like channels.
- Avoid callbacks.
- Write `golang` style concurrent code.
- Clojurescript let's you make use of channels as a better way of
  managing complex asynchronicity in a Javascript app.

---

# Macros

<br>

Macros are a way to dynamically write or expand code at runtime. It's
a very popular lisp concept, and covered in great depth in many
places. Here are the basic ideas,

<br>

0. Macros let you perform magic!
1. Macros can get unweildy and complex!
2. Don't abuse macros!

---

# Macros

<br>

There are lots of core components of clojure that are built using
macros. We can dive a bit into how those work with `macroexpand` and `macroexpand-1`.

---

# Macros

<br>

    ;; When is a macro for the one-armed-if, notice it expands to an
    ;; if statement
    (macroexpand '(when (>= 2 1) (println "Basic logic seems to hold")))
    => (if (>= 2 1) (do (println "Basic logic seems to hold")))

    ;; Threading is syntactic sugar that helps you avoid parens and
    ;; deeply nested calls. This threads through the end.
    (macroexpand-1 '(->> (rand)
                         (+ (rand))
                         inc
                         (Math/exp)))
    => (clojure.core/->> (clojure.core/->> (rand) (+ (rand))) inc (Math/exp))

---

# Macros

We write macros using special reader syntax to control what forms get evaluated.

<br>

    (defmacro unless [pred a b]
      `(if (not ~pred) ~a ~b))

    (unless true 1 2) => 2

    ;; But you don't always need fancy syntax
    (defmacro postfix-notation [expression]
      (conj (butlast expression) (last expression)))

---

# Multimethods

Multimethods allow us to dispatch differently based on what we're
calling on.

<br>

One example might be polymorphic style dispatching on an account.

<br>

    (defmulti service-charge (fn [acct] [(account-level acct)
                                         (:tag acct)]))
    (defmethod service-charge [::acc/Basic ::acc/Checking]   [_] 25)
    (defmethod service-charge [::acc/Basic ::acc/Savings]    [_] 10)
    (defmethod service-charge [::acc/Premium ::acc/Account] [_] 0)

---

# Multimethods

<br>

    (defmulti greeting (fn[x] (x "language")))

    ;params is not used, so we could have used [_]
    (defmethod greeting "English" [params] "Hello!")

    (defmethod greeting "French" [params] "Bonjour!")

    ;;default handling
    (defmethod greeting :default [params]
     (throw (IllegalArgumentException.
              (str "I don't know the " (params "language") " language"))))

---

# Multimethods

<br>

    (def english-map {"id" "1", "language" "English"})
    (def  french-map {"id" "2", "language" "French"})
    (def spanish-map {"id" "3", "language" "Spanish"})

    (greeting english-map)
    => "Hello!"
    (greeting french-map)
    => "Bounjour!"
    (greeting spanish-map)
    => java.lang.IllegalArgumentException: I don't know the Spanish language

---

# Recursion

Because Clojure is built on the JVM, there are some issues with
recursion. There is no tail call optimization, and it can be easy to
blow the stack with self-recursion. Clojure provides `loop` and
`recur` as a way to get around this.

    ;; We can still use self-recursion, but it's not too great
    (defn fib [n]
      (condp = n
        0 0
        1 1
        (+ (fib (- n 1))
           (fib (- n 2)))))

---

# Recursion

    ;; Recur is a natural self-recursion, but...
    (defn fib [n]
      (condp = n
        0 0
        1 1
        (+ (recur (- n 1))
           (recur (- n 2)))))

    => java.lang.UnsupportedOperationException: Can only recur from tail position
    ;; ... it's a bit limited

---

# Recursion

`recur` is good for simple recursive cases, as it can only be called
once and from the tail position.

    (defn factorial
      ([n] (factorial n 1))
      ([cnt acc] (if (zero? cnt)
                   acc
                   (recur (dec cnt) (* acc cnt)))))

    (factorial 10)
    => 3628800

---

# Recursion

`loop` is provided to augment recur. Here's factorial rewritten to
avoid explicit self-recursion.

    (defn factorial [n]
      (loop [cnt n acc 1]
        (if (zero? cnt)
          acc
          (recur (dec cnt) (* acc cnt)))))

    (factorial 10)
    => 3628800

---

# Recursion

Recursion is immensely  useful; however, there are usually ways around
it. If we look back to the fibbonaci example we can use sequences to
avoid recursion all together.

Here's the Fibbonaci sequence again, this time it's not a function
that generates each number. It's a function that generates an infinite
lazy sequence.

    (def fib-seq
      ((fn rfib [a b]
         (lazy-seq (cons a (rfib b (+ a b)))))
       0 1))

---

# Recursion

Now we've modified it further to avoid recursion altogether,

    (def fib-seq
      ((fn []
         (letfn [(fib-step [[a b]] [b (+ a b)])]
           (map first (iterate fib-step [0 1]))))))

    (take 10 fib-seq)
    => (0 1 1 2 3 5 8 13 21 34)

---

# Getting Started

* [Install the JVM](http://www.oracle.com/technetwork/java/javase/downloads/index-jsp-138363.html)
* [Install Leiningen](http://leiningen.org)

        cd ~/bin
        wget https://raw.github.com/technomancy/leiningen/stable/bin/lein
        chmod +ax lein
        lein # this will run a self-install

* Try out the lein commands
        new, repl, run, deps, ...

---

# Getting Started

* Useful Plugins
  * [codox](https://github.com/weavejester/codox)
  * [ancient](https://github.com/xsc/lein-ancient)
  * [cljsbuild](https://github.com/emezeske/lein-cljsbuild)
  * [cloverage](https://github.com/lshift/cloverage)
  * [kibit](https://github.com/jonase/kibit)
  * [midje](https://github.com/marick/lein-midje)

---

# REPL

## Documentation

    (doc name)
    (find-doc str)

## Past args

    *1, *2, *3

## Errors

    *e


---

# The End

## Thank You!

You can probably find this presentation at,

<http://log0ymxm.github.io/intro-clojure-lambda-lounge-2014>

<br>

- Paul English <paul@redbrainlabs.com>
- <http://twitter.com/log0ymxm>
- <http://github.com/log0ymxm>
