ASImptota

Functional Language with new syntax for creating ASI Artificial Super Intelligence

ASImptota directly transpiles into TYpeScript 7

Since TS 7 based on Go it's several times faster than previuos versions

Lists:  `<a b c d>`

Vectors: `|1 2 3 4|`

First element:   `<head <a b c d>>` - returns a

Rest:   `<body <a b c d>>` - returns `<b c d>`

`'< >'` braces and `'head'` and `'body'` keywords taken from HTML

Add element at the end of list:  `<add <a b c d> e>` - returns `<a b c d e>`

Merge several lists:  `<merge <a b> <c d>>` - returns `<a b c d>`

Define named function:  `<fun square<x><* x x>>`

Create variable:  `<let <a 5> <b 10>><+ a b>` - returns 15

Conditionals:  `<if < '>' x 11> "large" "small">`

Loop while:  `<repeat <!= b 0> <if < '>' a b> <- a b> <- b a>>>` - Euclidean GCD algorithm

Loop for:   vector with one element '0', limit 'n' and iterator starting with 1 because first element is already in

 ```ASImptota
 <let <vec |0|><n 100> <i 1>>
<iterate < '<' i n> <add <vec i>> <+ i 1>>
```

I/O:  

```ASImptota
<fun euclid<a b><repeat <'!=' b 0> <if < '>' a b  > <- a b> <- b a>>>>
<in a b> //input
<out euclid<a b>> //output
```
`<in 15 25>`
 `<out euclid<15 25>>` //result 5

Example: polynomial   x^2 + 2 * x + 1   ( (x + 1)^2 )

```ASImptota
<let <polynom || 2 1| | 1 2 | | 0 1||> < n 2 > < sum 0 > < x 5>> -//polynomial ( 5^2 + 2 * 5 + 1 ) = 36
<fun power<x n> <<let < multi 1 >> <iterate < '>' n 0 > < * multi n > < - n 1 > >>> //x in power of n
<fun calculate<polynom> <let < i 0 >> <iterate < '=<' i n> < + < * < power<x polynom|i||0| >> polynom|i||1|> sum > <+ i 1>>>>
```
        













