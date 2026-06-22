# ASImptota II: Language Specification & Design

Welcome to the evolution of **ASI** development. While **ASImptota** laid the foundation with its HTML-inspired bracket syntax and blistering TypeScript 7 compilation speed, it still leaned on imperative loops (`repeat`, `iterate`) and basic data handling.

**ASImptota II** marries the distinctive HTML-bracket syntax of the original language with the powerful, immutable, and homoiconic semantics of **Clojure**. Because **TypeScript 7** compiles directly to optimized Go binaries, ASImptota II implements persistent, thread-safe data structures with near-zero performance overhead, making it the premier language for architecting **Artificial Super Intelligence**.

---

## 1. Core Semantics & Data Structures

ASImptota II abandons mutable state. Every data structure is persistent and immutable. When you modify a structure, a new version is returned, sharing structural history behind the scenes to maximize efficiency.

### Data Literal Syntax

| Structure | ASImptota II Syntax | Clojure Equivalent | Description |
| --- | --- | --- | --- |
| **List** | `<a b c d>` | `'(a b c d)` | Singly-linked list. Growth happens at the **head**. |
| **Vector** |  pipe 1 2 3 4 pipe | `[1 2 3 4]` | Vector of values, not necessary numeral | 
| **Map** | `/~name "alice" ~age 30/` | `{:name "alice" :age 30}` | Key-value pairs. Keywords start with `~`. |
| **Set** | `(~apple ~orange)` | `#{:apple :orange}` | Unique collections of elements. |

### The Sequence Abstraction

Instead of specialized list-only operations, ASImptota II adopts Clojure’s sequence abstraction. Functions like `head` and `body` work universally across all collections.

* `<head coll>`: Returns the first element.
* `<body coll>`: Returns a sequence of the remaining elements.
* `<conj coll element>`: Conses/conjoins an element to the collection based on its structural efficiency (at the front for Lists, at the end for Vectors).

```ASImptota
// Keywords act as functions when applied to maps
<let user /~name "Alice" ~role "ASI"/
  <~name user>> 
// Returns "Alice"

// Universal sequence handling
<head |1 2 3|> ; Returns 1
<body |1 2 3|> ; Returns |2 3|

```

---

## 2. Functional Control Flow & Local Bindings

Gone are the imperative `<let <a 5>>` block chains. ASImptota II utilizes Clojure-style vector blocks for local variable definitions, allowing clean, parallel block evaluation.

### Functions and Anonymous Lambdas

* `fun`: Defines a named, globally accessible function.
* `lambda`: Creates an anonymous lambda function.

```ASImptota
// Named function
<fun square |x| <* x x>>

// Anonymous function mapped over a vector
<map <lambda |x| <* x 2>> |1 2 3|>
// Returns |2 4 6|

```

### Tail-Call Optimization: `loop` and `recur`

To prevent stack overflows in TS7, ASImptota II replaces the original `repeat` and `iterate` loops with functional tail-recursion via `loop` and `recur`.

#### Example: Euclidean GCD Algorithm

```ASImptota
<fun euclid |a b|
  <loop |x a y b|
    <if <= y 0>
      x
      <recur y <rem x y>>>>>

```

---

## 3. Advanced Clojure Power Features

### Vector & Map Destructuring

Binding variables out of deeply nested data is declarative and simple.

```ASImptota
// Destructuring a vector
<let <first-item second-item> <body |1 2 3 4|>
  <+ first-item second-item>>
// Evaluates to: 2 + 3 = 5

// Destructuring a map
<let </~name user-name ~age user-age/ /~name "Bob" ~age 42/>
  <str user-name " is " user-age " cycles old.">>
// Returns "Bob is 42 cycles old."

```

### State Management via Atoms

Because ASImptota II is strictly immutable, shared mutable state is handled explicitly using `atom` models, matching Clojure's software transactional memory ideals.

```ASImptota
<let <global-counter <atom 0>>
  <swap! global-counter inc>
  <@ global-counter>> 
// Returns 1 (@ is the dereference macro)

```

### Macro System (Homoiconicity)

Because code is written using lists `<...>` and vectors `|...|`, code can manipulate code before compilation.

```ASImptota
<macro unless <pred form>
  <if pred nil form>>

```

---

## 4. Comprehensive Code Showcase

Let's rewrite the polynomial computation ($x^2 + 2x + 1$) from the original language. Instead of imperative matrix index tracking (`polynom|i||0|`), we use elegant functional reduction, map destructuring, and sequence transformations.

```ASImptota
// Pure functional power computation using tail recursion
<fun power <base exp>
  <loop |acc 1 e exp|
    <if <<= e 0>
      acc
      <recur <* acc base> <- e 1>>>>>

// Computes a single polynomial term: coef * (x ^ exp)
<fun calc-term <x term>
  <let |/~coef c ~exp e/ term|
    <* c <power x e>>>>

// Evaluates the entire polynomial sequence for a given x
<fun calculate |poly x|
  <reduce 
    <lambda |sum term| <+ sum <calc-term x term>>> 
    0 
    poly>>

// --- Execution Execution Context ---

// Define the polynomial: x^2 + 2x + 1
<let |my-poly |/~coef 1 ~exp 2/ 
               /~coef 2 ~exp 1/ 
               /~coef 1 ~exp 0/|
      x-val 5|
  <out <calculate my-poly x-val>>>
// Outputs: 36 (5^2 + 2*5 + 1)

```

---

## 5. Eratosthenes Sieve

To maintain the performance and the exact optimization logic of original code (which uses a packed array representing only odd numbers and an optimized index-matching formula), this translation utilizes **ASImptota II**'s high-performance boolean-array and standard loop/recur structures for the sifting phase, combined with idiomatic sequence operations for the formatting phase.

```ASImptota
<ns eratosthenes-sieve.core
  <~require |asi.string ~as str|>>

//Eratosthenes Sieve
<fun eratosthenes ||
  <let |n 1000
        primes <boolean-array n true>|
    
    // --- Sifting Process <proc sift> ---
    <loop |i 0
           index-square 3|
      <when <'<' index-square n>
        <when <aget primes i>
          <let |factor <+ i i 3>|
            // Mark composites <proc mark sieve>
            <loop |first-idx index-square|
              <when <'<' first-idx n>
                <aset-boolean primes first-idx false>
                <recur <+ first-idx factor>>>>>>
        // Calculate next index using the formula: 2 * i * (i + 3) + 3
        <let |next-i <inc i>
              next-index-square <+ <* 2 next-i <+ next-i 3>> 3>|
          <recur next-i next-index-square>>>>

    // --- Output Phase <print out> ---
    
    // Generate odd primes from the sieve array where (2i + 3) <= n
    <let |odd-primes <for |i <range n>
                           ~let |num <+ <* 2 i> 3>|
                           ~when <and <aget primes i> <'<=' num n>>|
                       num>          
          all-primes <cons 2 odd-primes>| // Prepend 2 to the list of primes
      
      // Print primes grouped into chunks of 10 per line
      <doseq |chunk <partition-all 10 all-primes>|
        <doseq |p chunk|
          <print <str " " p>>>
        <println>>
      
      // Print total count matching the original format
      <println <str "\n number : " <count all-primes>>>>>>

// To run the function:
// <eratosthenes>
```
---
## 6. Compilation Blueprint to TypeScript 7

When transpiled, ASImptota II leverages the native Go-runtime constructs available in TS7:

1. **Immutability Integration**: Persistent vectors translate directly to Go slice pointers with structural sharing, bypassing the old V8 garbage collection overhead entirely.
2. **Tail Call Erasure**: The `loop`/`recur` framework is compiled into a high-performance, native Go `for` loop block inside the TS7 final output, ensuring zero call-stack overhead.
3. **Keywords**: `~keyword` forms transpile to unique, deeply-cached integer hashes within the TS7 engine, making map key lookups look like structural pointer matching.

