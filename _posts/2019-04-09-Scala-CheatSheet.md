---
layout: post
title: "Scala Cheatsheet"
categories:
  - guide
tags:
  - guide
---
As I learn scala, I am going to update these notes for my own References

### Lists
Lists are immutable

#### Empty List
val list1 = List.empty[Int]

#### Append
val list1 = List(10, 20, 30)
val list2 = 40 :: list1

### Arrays
```scala
val array = Array(1, 2, 3)
// append
val array1 = array :+ 4
// prepend
val array2 = 4 +: array
```

### Sets
Sets come in two flavors: Mutable and immutable.

```scala
val fruit = Set("Apple", "Orange", "Peach", "Banana")
//adding elements
fruit += "Pineapple"
//Deleting elements
fruit -= "Orange"
```
There is `SortedSet` in scala as well

### HashMaps
Scala has two types of HashMaps: Mutable and Immutable.
`Map()` creates an immutable Map. In order to create mutable Map use `scala.collection.mutable.Map()`
```scala
var states = scala.collection.mutable.Map[String, String]()
// Adding elements
states += ("AZ" -> "Arizona", "KY" -> "Kentucky")
// removing elements
states -= "AZ"
// Update
states("KY") = "Kentucky, Chicken"
```


### Iterators
Using iterators
```scala
  while(it.hasNext){
    val = it.next()
  }
```

### TypeTags
TypeTags help solve the type erasure problem.

```scala
import scala.reflect.runtime.universe._

def meth[A : TypeTag](xs: List[A]) = typeOf[A] match {
  case t if t =:= typeOf[String] => "list of strings"
  case t if t <:< typeOf[Foo] => "list of foos"
}
```
Here `=:=` is to check for type equality while `<:<` checks for subtype relation. More information can be found [here](https://stackoverflow.com/questions/12218641/scala-what-is-a-typetag-and-how-do-i-use-it)

**References:**