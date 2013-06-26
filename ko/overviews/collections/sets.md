---
layout: overview-large
title: 집합(Set)

disqus: true

partof: collections
num: 6
language: ko
---

`Set(집합)`은 `Iterable(반복가능)` 중에서 중복 원소가 없는 것이다. 일반적인 집합의 연산은 다음 표에 정리되어 있고, 변경 가능한 집합의 연산은 그 다음에 오는 표에 정리되어 있다. 연산들은 다음과 같은 범주에 들어간다.

* **검사** 연산으로 `contains`, `apply`, `subsetOf`가 있다. `contains` 메소드는 집합에 원소가 속해 있는지를 검사한다. 집합에 있어 `apply` 메소드는 `contains`과 동일하다. 따라서 `set(elem)`과 `set contains elem`는 같다. 따라서 집합을 원소가 포함되어있는지를 검사하는 검사 함수로 사용 가능하다. 

예를 들면 다음과 같다.


    val fruit = Set("apple", "orange", "peach", "banana")
    fruit: scala.collection.immutable.Set[java.lang.String] = 
    Set(apple, orange, peach, banana)
    scala> fruit("peach")
    res0: Boolean = true
    scala> fruit("potato")
    res1: Boolean = false


* **추가** 연산에는 `+` and `++`가 있다. 이들은 집합에 하나 이상의 원소를 추가한 새 집합을 만들어 낸다.
* **제거** 연산인 `-`, `--`는 집합에서 하나 이상의 원소를 제거한 새 집합을 만들어 낸다.
* **집합 연산**으로 합집합, 교집합, 차집합이 있다. 각 연산은 두가지 버전이 존재한다. 하나는 영어문자를 사용한 것이고, 다른 하나는 기호로 된 이름을 사용한 것이다. 영문자 버전은 `intersect`, `union`, `diff`이며, 각각 순서대로 `&`, `|`, `&~`이란 기호 이름과 대응된다. 사실은 `Traversable`에서 상속받은 `++`도 `union` 또는 `|`에 대한 별칭이라 생각할수 있다. 다만 차이가 나는 것은 `++`는 `Traversable`을 매개변수로 받을 수 있지만, `union`과 `|`에는 집합만을 허용한다는 점이다.

### 집합(Set)의 연산 ###

| 사용법   	  	    | 하는일				     |
| ------       	       	    | ------					     |
|  **검사:**               |						     |
|  `xs contains x`  	    |`x`가 `xs`의 원소인지 여부를 반환한다.        |
|  `xs(x)`        	    |`xs contains x`와 같다.                        |
|  `xs subsetOf ys`  	    |`xs`가 `ys`의 부분집합인지 여부를 반환한다.         |
|  **추가:**           |						     |
|  `xs + x`                 |`xs`의 모든 원소와 `x`를 원소로 하는 새 집합을 반환한다.|
|  `xs + (x, y, z)`         |`xs`의 모든 원소와 덧붙인 모든 값들을 원소로 하는 새 집합을 반환한다.|
|  `xs ++ ys`  	            |`xs`의 모든 원소와 `ys`의 모든 원소를 원소로 하는 새 집합을 반환한다.|
|  **제거:**               |						     |
|  `xs - x`  	            |`xs`의 모든 원소 중 `x`를 제거한 나머지를 원소로 하는 새 집합을 반환한다.|
|  `xs - (x, y, z)`         |`xs`의 모든 원소 중 열거한 원소들을 제외한 나머지를 원소로 하는 새 집합을 반환한다.|
|  `xs -- ys`  	            |`xs`의 모든 원소 중 `ys`의 원소들을 제거한 나머지를 원소로 하는 새 집합을 반환한다.|
|  `xs.empty`  	            |`xs`와 동일한 타입의 빈 집합을 반환한다.         |
|  **이항 연산:**   |						     |
|  `xs & ys`  	            |`xs`와 `ys`의 교집합 연산이다.          |
|  `xs intersect ys`        |`xs & ys`와 같다.                              |
|  <code>xs &#124; ys</code>  	            |`xs`와 `ys`의 합집합 연산이다.          |
|  `xs union ys`  	    |<code>xs &#124; ys</code>와 같다..                              |
|  `xs &~ ys`  	            |`xs`와 `ys`의 차집합 연산이다.          |
|  `xs diff ys`  	    |`xs &~ ys`와 같다..                             |

변경 가능한 집합은 원소를 추가, 삭제, 변경하는 연산을 추가로 제공한다. 아래 표에 정리되어 있다.

### 변경 가능 집합(mutable.Set)의 연산 ###

| 사용법               | 하는일                    |
| ------       	       	    | ------					     |
|  **추가:**           |						     |
|  `xs += x`  	            |Adds element `x` to set `xs` as a side effect and returns `xs` itself.|
|  `xs += (x, y, z)`        |Adds the given elements to set `xs` as a side effect and returns `xs` itself.|
|  `xs ++= ys`  	    |Adds all elements in `ys` to set `xs` as a side effect and returns `xs` itself.|
|  `xs add x`  	            |Adds element `x` to `xs` and returns `true` if `x` was not previously contained in the set, `false` if it was.|
|  **제거:**            |						     |
|  `xs -= x`  	            |Removes element `x` from set `xs` as a side effect and returns `xs` itself.|
|  `xs -= (x, y, z)`  	    |Removes the given elements from set `xs` as a side effect and returns `xs` itself.|
|  `xs --= ys`  	    |Removes all elements in `ys` from set `xs` as a side effect and returns `xs` itself.|
|  `xs remove x`  	    |Removes element `x` from `xs` and returns `true` if `x` was previously contained in the set, `false` if it was not.|
|  `xs retain p`  	    |Keeps only those elements in `xs` that satisfy predicate `p`.|
|  `xs.clear()`  	    |Removes all elements from `xs`.|
|  **변경:**              |						     |
|  `xs(x) = b`  	    |(or, written out, `xs.update(x, b)`). If boolean argument `b` is `true`, adds `x` to `xs`, otherwise removes `x` from `xs`.|
|  **복제:**             |						     |
|  `xs.clone`  	            |A new mutable set with the same elements as `xs`.|

Just like an immutable set, a mutable set offers the `+` and `++` operations for element additions and the `-` and `--` operations for element removals. But these are less often used for mutable sets since they involve copying the set. As a more efficient alternative, mutable sets offer the update methods `+=` and `-=`. The operation `s += elem` adds `elem` to the set `s` as a side effect, and returns the mutated set as a result. Likewise, `s -= elem` removes `elem` from the set, and returns the mutated set as a result. Besides `+=` and `-=` there are also the bulk operations `++=` and `--=` which add or remove all elements of a traversable or an iterator.

The choice of the method names `+=` and `-=` means that very similar code can work with either mutable or immutable sets. Consider first the following REPL dialogue which uses an immutable set `s`:

    scala> var s = Set(1, 2, 3)
    s: scala.collection.immutable.Set[Int] = Set(1, 2, 3)
    scala> s += 4
    scala> s -= 2
    scala> s
    res2: scala.collection.immutable.Set[Int] = Set(1, 3, 4)

We used `+=` and `-=` on a `var` of type `immutable.Set`. A statement such as `s += 4` is an abbreviation for `s = s + 4`. So this invokes the addition method `+` on the set `s` and then assigns the result back to the `s` variable. Consider now an analogous interaction with a mutable set.


    scala> val s = collection.mutable.Set(1, 2, 3)
    s: scala.collection.mutable.Set[Int] = Set(1, 2, 3)
    scala> s += 4
    res3: s.type = Set(1, 4, 2, 3)
    scala> s -= 2
    res4: s.type = Set(1, 4, 3)

The end effect is very similar to the previous interaction; we start with a `Set(1, 2, 3)` end end up with a `Set(1, 3, 4)`. However, even though the statements look the same as before, they do something different. `s += 4` now invokes the `+=` method on the mutable set value `s`, changing the set in place. Likewise, `s -= 2` now invokes the `-=` method on the same set.

Comparing the two interactions shows an important principle. You often can replace a mutable collection stored in a `val` by an immutable collection stored in a `var`, and _vice versa_. This works at least as long as there are no alias references to the collection through which one can observe whether it was updated in place or whether a new collection was created.

Mutable sets also provide add and remove as variants of `+=` and `-=`. The difference is that `add` and `remove` return a Boolean result indicating whether the operation had an effect on the set.

The current default implementation of a mutable set uses a hashtable to store the set's elements. The default implementation of an immutable set uses a representation that adapts to the number of elements of the set. An empty set is represented by just a singleton object. Sets of sizes up to four are represented by a single object that stores all elements as fields. Beyond that size, immutable sets are implemented as [hash tries](#hash-tries).

A consequence of these representation choices is that, for sets of small sizes (say up to 4), immutable sets are usually more compact and also more efficient than mutable sets. So, if you expect the size of a set to be small, try making it immutable.

Two subtraits of sets are `SortedSet` and `BitSet`.

### Sorted Sets ###

A [SortedSet](http://www.scala-lang.org/api/current/scala/collection/SortedSet.html) is a set that produces its elements (using `iterator` or `foreach`) in a given ordering (which can be freely chosen at the time the set is created). The default representation of a [SortedSet](http://www.scala-lang.org/api/current/scala/collection/SortedSet.html) is an ordered binary tree which maintains the invariant that all elements in the left subtree of a node are smaller than all elements in the right subtree. That way, a simple in order traversal can return all tree elements in increasing order. Scala's class [immutable.TreeSet](http://www.scala-lang.org/api/current/scala/collection/immutable/TreeSet.html) uses a _red-black_ tree implementation to maintain this ordering invariant and at the same time keep the tree _balanced_-- meaning that all paths from the root of the tree to a leaf have lengths that differ only by at most one element.

To create an empty [TreeSet](http://www.scala-lang.org/api/current/scala/collection/immutable/TreeSet.html), you could first specify the desired ordering:

    scala> val myOrdering = Ordering.fromLessThan[String](_ > _)
    myOrdering: scala.math.Ordering[String] = ...

Then, to create an empty tree set with that ordering, use:

    scala> TreeSet.empty(myOrdering)
    res1: scala.collection.immutable.TreeSet[String] = TreeSet()

Or you can leave out the ordering argument but give an element type or the empty set. In that case, the default ordering on the element type will be used.

    scala> TreeSet.empty[String]
    res2: scala.collection.immutable.TreeSet[String] = TreeSet()

If you create new sets from a tree-set (for instance by concatenation or filtering) they will keep the same ordering as the original set. For instance,

scala> res2 + ("one", "two", "three", "four")
res3: scala.collection.immutable.TreeSet[String] = TreeSet(four, one, three, two)

Sorted sets also support ranges of elements. For instance, the `range` method returns all elements from a starting element up to, but excluding, and end element. Or, the `from` method returns all elements greater or equal than a starting element in the set's ordering. The result of calls to both methods is again a sorted set. Examples:

    scala> res3 range ("one", "two")
    res4: scala.collection.immutable.TreeSet[String] = TreeSet(one, three)
    scala> res3 from "three"
    res5: scala.collection.immutable.TreeSet[String] = TreeSet(three, two)


### Bitsets ###

Bitsets are sets of non-negative integer elements that are implemented in one or more words of packed bits. The internal representation of a [BitSet](http://www.scala-lang.org/api/current/scala/collection/BitSet.html) uses an array of `Long`s. The first `Long` covers elements from 0 to 63, the second from 64 to 127, and so on (Immutable bitsets of elements in the range of 0 to 127 optimize the array away and store the bits directly in a one or two `Long` fields.) For every  `Long`, each of its 64 bits is set to 1 if the corresponding element is contained in the set, and is unset otherwise. It follows that the size of a bitset depends on the largest integer that's stored in it. If `N` is that largest integer, then the size of the set is `N/64` `Long` words, or `N/8` bytes, plus a small number of extra bytes for status information.

Bitsets are hence more compact than other sets if they contain many small elements. Another advantage of bitsets is that operations such as membership test with `contains`, or element addition and removal with `+=` and `-=` are all extremely efficient.

