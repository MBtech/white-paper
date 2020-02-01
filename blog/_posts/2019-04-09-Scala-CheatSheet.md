---
layout: post
title: "Scala Cheatsheet"
categories:
  - guide
  - csblog
tags:
  - guide
---
As I learn scala, I am going to update these notes for my own References

### Lists
Lists are immutable

Empty List:
```scala
val list1 = List.empty[Int]
```

Append:
```scala
val list1 = List(10, 20, 30)
val list2 = 40 :: list1
```

Convert List[Any] to List[Int]:
```scala
# l is the list of Any datatype
l.map(_.toString.toInt)
```

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

### Gems
Ternary operator syntax:
`val a = if (i == 1) x else y`

### Recommendations
If an object doesn't need to be serialized then use `@transient lazy val`. `lazy val` are fields that are evaluated once they are access for the first time and `@transient` denotes that a field shouldn't be serialized.

### SBT
To define a location you have to add something like this to the resolvers `resolvers += "repository name" at "location"` such as:
```Scala
resolvers += "Java.net Maven2 Repository" at "http://download.java.net/maven/2/"
```

**Running a program:**

If there is no need for command line arguments:
`sbt run`

If specific class with command line parameters has to be executed then:

`sbt "runMain package.path.to.main.class param1 param2"`
or
`sbt "run-main package.path.to.main.class param1 param2"`

**References:**
