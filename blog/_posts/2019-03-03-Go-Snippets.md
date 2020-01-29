---
layout: post
title: "Golang snippets"
categories:
  - cheatsheet
tags:
  - cheatsheet
---
# Go code snippets and libraries

Updated on: 03-03-2019
## Deleting an element from a slice
Let's say that the element you want to delete is on index 'i' then

```go
a = append(a[:i], a[i+1:]...)
```

## Sorting in Golang
This is sorting an array of structure `planets` according to the axis value of the struct

```go
slice.Sort(planets[:], func(i, j int) bool {
    return planets[i].Axis < planets[j].Axis
})```

### Serialization library
If you want to send data from one node to another you will likely need some serialization/deserialization. Either use the [encoding/gob](https://golang.org/pkg/encoding/gob/) library or [struc](https://github.com/lunixbochs/struc) library. For more information take a look at [this tutorial for gob](https://medium.com/@kpbird/golang-serialize-struct-using-gob-part-1-e927a6547c00).

Here's a snippet of code for gob:
```go

package main
import (
"fmt"
"encoding/gob"
"bytes"
)
var m = map[string]int{"one":1, "two":2, "three":3}
func main() {
b := new(bytes.Buffer)
e := gob.NewEncoder(b)
// Encoding the map
err := e.Encode(m)
if err != nil {
    panic(err)
}
var decodedMap map[string]int
d := gob.NewDecoder(b)
// Decoding the serialized data
err = d.Decode(&decodedMap)
if err != nil {
    panic(err)
}
// Ta da! It is a map!
fmt.Printf("%#v\n", decodedMap)
}
```

## Short intro to Interfaces
Checkout [this page](https://gobyexample.com/interfaces) from go by example to get a basic idea about how to work with Interfaces in go.  

## Number parsing examples
Here's a snippet from go by example. Here's the Go playground [link](https://play.golang.org/p/NZh4LjhguvN)
```go
package main
// The built-in package `strconv` provides the number
// parsing.
import "strconv"
import "fmt"
func main() {
    // With `ParseFloat`, this `64` tells how many bits of
    // precision to parse.
    f, _ := strconv.ParseFloat("1.234", 64)
    fmt.Println(f)
    // For `ParseInt`, the `0` means infer the base from
    // the string. `64` requires that the result fit in 64
    // bits.
    i, _ := strconv.ParseInt("123", 0, 64)
    fmt.Println(i)
    // `ParseInt` will recognize hex-formatted numbers.
    d, _ := strconv.ParseInt("0x1c8", 0, 64)
    fmt.Println(d)
    // A `ParseUint` is also available.
    u, _ := strconv.ParseUint("789", 0, 64)
    fmt.Println(u)
    // `Atoi` is a convenience function for basic base-10
    // `int` parsing.
    k, _ := strconv.Atoi("135")
    fmt.Println(k)
    // Parse functions return an error on bad input.
    _, e := strconv.Atoi("wat")
    fmt.Println(e)
}
```

## Generating a slice with sequence of numbers:
Here's a function to generate sequence of numbers:

```go
func makeRange(min, max int) []int {
    a := make([]int, max-min+1)
    for i := range a {
        a[i] = min + i
    }
    return a
}
```
[Link to Go playground](https://play.golang.org/p/rOCmHbT9Xvu)

## Useful Libraries:
1. Numpy-like functionality: [gonum](https://www.gonum.org/)
2. Working with and modifying HTML files? Take a look at [template package](https://golang.org/pkg/html/template/)

**References**

1. https://stackoverflow.com/questions/25025409/delete-element-in-a-slice
2. https://stackoverflow.com/questions/28999735/what-is-the-shortest-way-to-simply-sort-an-array-of-structs-by-arbitrary-field
3. https://stackoverflow.com/questions/39868029/how-to-generate-a-sequence-of-numbers-in-golang
