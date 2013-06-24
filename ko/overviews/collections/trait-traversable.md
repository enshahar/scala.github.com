---
layout: overview-large
title: Traversable 트레잇

disqus: true

partof: collections
num: 3
language: ko
---

컬렉션 계층의 최상위에는 트레잇 `Traversable(방문 가능)`이 있다. 이 트레잇에 
있는 유일한 추상적인 연산이 바로 `foreach`이다.

    def foreach[U](f: Elem => U) 

`Traversable`을 구현하는 컬렉션 클래스는 단지 이 메소드만 정의하면 된다. 
다른 메소드는 자동으로 `Traversable`에서 상속된다.

`foreach` 메소드는 컬렉션의 모든 원소를 방문하면서 주어진 연산 f를 각 원소에 
적용한다. 이 연산 f의 타입은 `Elem => U`로 `Elem`은 컬렉션의 원소의 
타입이며, `U`는 임의의 결과 타입이다. `f`는 부작용을 위해 호출된다. 따라서 
f가 내놓는 결과값은 `foreach`가 무시한다.

`Traversable`에는 여러 구체적 메소드가 정의되어 있다. 이들은 아래 표에 나열되어 있다. 각 메소드들은 다음과 같은 분류에 속한다.

* **합치기**, `++`는 두 방문가능한 객체를 함께 이어붙이거나, 한 반복자의 모든 원소를 다른 방문가능 객체에 추가한다.
* **맵** 연산인 `map`, `flatMap`, `collect`는 인자로 넘겨진 함수를 컬렉션 원소에 적용한 결과로 이루어진 새 컬렉션을 만들어낸다.
* **Conversions** `toArray`, `toList`, `toIterable`, `toSeq`, `toIndexedSeq`, `toStream`, `toSet`, `toMap`, which turn a `Traversable` collection into something more specific. All these conversions return their receiver argument unchanged if the run-time type of the collection already matches the demanded collection type. For instance, applying `toList` to a list will yield the list itself.
* **Copying operations** `copyToBuffer` and `copyToArray`. As their names imply, these copy collection elements to a buffer or array, respectively.
* **Size info** operations `isEmpty`, `nonEmpty`, `size`, and `hasDefiniteSize`: Traversable collections can be finite or infinite. An example of an infinite traversable collection is the stream of natural numbers `Stream.from(0)`. The method `hasDefiniteSize` indicates whether a collection is possibly infinite. If `hasDefiniteSize` returns true, the collection is certainly finite. If it returns false, the collection has not been not fully elaborated yet, so it might be infinite or finite.
* **Element retrieval** operations `head`, `last`, `headOption`, `lastOption`, and `find`. These select the first or last element of a collection, or else the first element matching a condition. Note, however, that not all collections have a well-defined meaning of what "first" and "last" means. For instance, a hash set might store elements according to their hash keys, which might change from run to run. In that case, the "first" element of a hash set could also be different for every run of a program. A collection is _ordered_ if it always yields its elements in the same order. Most collections are ordered, but some (_e.g._ hash sets) are not-- dropping the ordering gives a little bit of extra efficiency. Ordering is often essential to give reproducible tests and to help in debugging. That's why Scala collections give ordered alternatives for all collection types. For instance, the ordered alternative for `HashSet` is `LinkedHashSet`.
* **Sub-collection retrieval operations** `tail`, `init`, `slice`, `take`, `drop`, `takeWhile`, `dropWhile`, `filter`, `filterNot`, `withFilter`. These all return some sub-collection identified by an index range or some predicate.
* **Subdivision operations** `splitAt`, `span`, `partition`, `groupBy`, which split the elements of this collection into several sub-collections.
* **Element tests** `exists`, `forall`, `count` which test collection elements with a given predicate.
* **Folds** `foldLeft`, `foldRight`, `/:`, `:\`, `reduceLeft`, `reduceRight` which apply a binary operation to successive elements.
* **Specific folds** `sum`, `product`, `min`, `max`, which work on collections of specific types (numeric or comparable).
* **String** operations `mkString`, `addString`, `stringPrefix`, which give alternative ways of converting a collection to a string.
* **View** operations, consisting of two overloaded variants of the `view` method. A view is a collection that's evaluated lazily. You'll learn more about views in [later](#Views).

### Operations in Class Traversable ###

| WHAT IT IS  	  	    | WHAT IT DOES				     |
| ------       	       	    | ------					     |
|  **Abstract Method:**     |						     |
|  `xs foreach f`	    |Executes function `f` for every element of `xs`.|
|  **Addition:**     	    |						     |
|  `xs ++ ys`	    	    |A collection consisting of the elements of both `xs` and `ys`. `ys` is a [TraversableOnce](http://www.scala-lang.org/api/current/scala/collection/TraversableOnce.html) collection, i.e., either a [Traversable](http://www.scala-lang.org/api/current/scala/collection/Traversable.html) or an [Iterator](http://www.scala-lang.org/api/current/scala/collection/Iterator.html).|
|  **Maps:**     	    |						     |
|  `xs map f`		    |The collection obtained from applying the function f to every element in `xs`.|
|  `xs flatMap f`	    |The collection obtained from applying the collection-valued function `f` to every element in `xs` and concatenating the results.|
|  `xs collect f`	    |The collection obtained from applying the partial function `f` to every element in `xs` for which it is defined and collecting the results.|
|  **Conversions:**         |						     |
|  `xs.toArray`	    	    |Converts the collection to an array.	     |
|  `xs.toList`	    	    |Converts the collection to a list.		     |
|  `xs.toIterable`    	    |Converts the collection to an iterable.	     |
|  `xs.toSeq`	    	    |Converts the collection to a sequence.	     |
|  `xs.toIndexedSeq`   	    |Converts the collection to an indexed sequence. |
|  `xs.toStream`    	    |Converts the collection to a lazily computed stream.|
|  `xs.toSet`	    	    |Converts the collection to a set.		     |
|  `xs.toMap`	    	    |Converts the collection of key/value pairs to a map. If the collection does not have pairs as elements, calling this operation results in a static type error.|
|  **Copying:**             |						     |
|  `xs copyToBuffer buf`    |Copies all elements of the collection to buffer `buf`.|
|  `xs copyToArray(arr, s, n)`|Copies at most `n` elements of the collection to array `arr` starting at index `s`. The last two arguments are optional.|
|  **Size info:**           |						     |
|  `xs.isEmpty`	    	    |Tests whether the collection is empty.	     |
|  `xs.nonEmpty`    	    |Tests whether the collection contains elements. |
|  `xs.size`	    	    |The number of elements in the collection.	     |
|  `xs.hasDefiniteSize`	    |True if `xs` is known to have finite size.	     |
|  **Element Retrieval:**   |						     |
|  `xs.head`	    	    |The first element of the collection (or, some element, if no order is defined).|
|  `xs.headOption`	    |The first element of `xs` in an option value, or None if `xs` is empty.|
|  `xs.last`	    	    |The last element of the collection (or, some element, if no order is defined).|
|  `xs.lastOption`	    |The last element of `xs` in an option value, or None if `xs` is empty.|
|  `xs find p`	    	    |An option containing the first element in `xs` that satisfies `p`, or `None` is no element qualifies.|
|  **Subcollections:**      |						     |
|  `xs.tail`	    	    |The rest of the collection except `xs.head`.    |
|  `xs.init`	    	    |The rest of the collection except `xs.last`.    |
|  `xs slice (from, to)`    |A collection consisting of elements in some index range of `xs` (from `from` up to, and excluding `to`).|
|  `xs take n`	    	    |A collection consisting of the first `n` elements of `xs` (or, some arbitrary `n` elements, if no order is defined).|
|  `xs drop n`	    	    |The rest of the collection except `xs take n`.|
|  `xs takeWhile p`	    |The longest prefix of elements in the collection that all satisfy `p`.|
|  `xs dropWhile p`	    |The collection without the longest prefix of elements that all satisfy `p`.|
|  `xs filter p`	    |The collection consisting of those elements of xs that satisfy the predicate `p`.|
|  `xs withFilter p`	    |A non-strict filter of this collection. Subsequent calls to `map`, `flatMap`, `foreach`, and `withFilter` will only apply to those elements of `xs` for which the condition `p` is true.|
|  `xs filterNot p`	    |The collection consisting of those elements of `xs` that do not satisfy the predicate `p`.|
|  **Subdivisions:**        |						     |
|  `xs splitAt n`	    |Split `xs` at a position, giving the pair of collections `(xs take n, xs drop n)`.|
|  `xs span p`	    	    |Split `xs` according to a predicate, giving the pair of collections `(xs takeWhile p, xs.dropWhile p)`.|
|  `xs partition p`	    |Split `xs` into a pair of two collections; one with elements that satisfy the predicate `p`, the other with elements that do not, giving the pair of collections `(xs filter p, xs.filterNot p)`|
|  `xs groupBy f`	    |Partition `xs` into a map of collections according to a discriminator function `f`.|
|  **Element Conditions:**  |						     |
|  `xs forall p`	    |A boolean indicating whether the predicate `p` holds for all elements of `xs`.|
|  `xs exists p`	    |A boolean indicating whether the predicate `p` holds for some element in `xs`.|
|  `xs count p`	    	    |The number of elements in `xs` that satisfy the predicate `p`.|
|  **Folds:** 		    |						     |
|  `(z /: xs)(op)`	    |Apply binary operation `op` between successive elements of `xs`, going left to right and starting with `z`.|
|  `(xs :\ z)(op)`	    |Apply binary operation `op` between successive elements of `xs`, going right to left and starting with `z`.|
|  `xs.foldLeft(z)(op)`	    |Same as `(z /: xs)(op)`.|
|  `xs.foldRight(z)(op)`    |Same as `(xs :\ z)(op)`.|
|  `xs reduceLeft op`	    |Apply binary operation `op` between successive elements of non-empty collection `xs`, going left to right.|
|  `xs reduceRight op`	    |Apply binary operation `op` between successive elements of non-empty collection `xs`, going right to left.|
|  **Specific Folds:**      |						     |
|  `xs.sum`	    	    |The sum of the numeric element values of collection `xs`.|
|  `xs.product`	    	    |The product of the numeric element values of collection `xs`.|
|  `xs.min`	    	    |The minimum of the ordered element values of collection `xs`.|
|  `xs.max`	    	    |The maximum of the ordered element values of collection `xs`.|
|  **Strings:**             |						     |
|  `xs addString (b, start, sep, end)`|Adds a string to `StringBuilder` `b` that shows all elements of `xs` between separators `sep` enclosed in strings `start` and `end`. `start`, `sep`, `end` are all optional.|
|  `xs mkString (start, sep, end)`|Converts the collection to a string that shows all elements of `xs` between separators `sep` enclosed in strings `start` and `end`. `start`, `sep`, `end` are all optional.|
|  `xs.stringPrefix`	    |The collection name at the beginning of the string returned from `xs.toString`.|
|  **Views:**               |						     |
|  `xs.view`	    	    |Produces a view over `xs`.|
|  `xs view (from, to)`     |Produces a view that represents the elements in some index range of `xs`.|
