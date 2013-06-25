---
layout: overview-large
title: Traversable() 트레잇

disqus: true

partof: collections
num: 3
language: ko
---

컬렉션 계층의 최상위에는 트레잇 `Traversable(순회가능)`이 있다. 이 트레잇에 
있는 유일한 추상적인 연산이 바로 `foreach`이다.

    def foreach[U](f: Elem => U) 

`Traversable`을 구현하는 컬렉션 클래스는 단지 이 메소드만 정의하면 된다. 
다른 메소드는 자동으로 `Traversable`에서 상속된다.

`foreach` 메소드는 컬렉션의 모든 원소를 방문하면서 주어진 연산 f를 각 원소에 
적용한다. 이 연산 f의 타입은 `Elem => U`로 `Elem`은 컬렉션의 원소의 
타입이며, `U`는 임의의 결과 타입이다. `f`는 부작용을 위해 호출된다. 따라서 
f가 내놓는 결과값은 `foreach`가 무시한다.

`Traversable`에는 여러 구체적 메소드가 정의되어 있다. 이들은 아래 표에 나열되어 있다. 
각 메소드들은 다음과 같은 분류에 속한다.

* **병합** 연산 `++`는 두 방문가능한 객체를 함께 이어붙이거나, 한 반복자의 모든 원소를 다른 방문가능 객체에 추가한다.
* **맵** 연산인 `map`, `flatMap`, `collect`는 인자로 넘겨진 함수를 컬렉션 원소에 적용한 결과로 이루어진 새 컬렉션을 만들어낸다.
* **변환** 연산인 `toArray`, `toList`, `toIterable`, `toSeq`, `toIndexedSeq`, `toStream`, `toSet`, `toMap` 등은 `Traversable` 
컬렉션을 더 구체적인 데이터 구조로 변환한다. 런타임에 수신자가 이미 변환 결과 컬렉션 타입이었다면, 각 변환 메소드는 수신자를 그대로 반환한다. 예를 들어 리스트에 `toList`를 적용하면 그 리스트 자신이 반환된다.
* **복사** 연산으로 `copyToBuffer`와 `copyToArray`가 있다. 이름이 말하는데로 각각 컬렉션 원소를 버퍼나 배열에 복사한다.
* **크기 정보** 연산 `isEmpty`, `nonEmpty`, `size`, `hasDefiniteSize`: 순회가능한 컬렉션은 유한할 수도 있고, 무한할 수도 있다. 
무한한 순회가능한 컬렉션의 예를 들자면 자연수의 스트림 `Stream.from(0)`이 있다. 메소드 `hasDefiniteSize`는 컬렉션이 무한 컬렉션일 가능성이 있는지를 알려준다. `hasDefiniteSize`가 참을 반환하면 컬렉션이 유한하다는 것이 확실하다. 하지만, 컬렉션이 내부 원소를 아직 완전히 계산해 채우지 않은 경우에는 거짓을 반환하기 때문에, 거짓을 반환한다 해도 유한할 수도 있고 무한할 수도 있다.
* **원소 가져오기** 연산 `head`, `last`, `headOption`, `lastOption`, `find`등은 컬렉션의 첫 원소나 마지막 원소를 선택하거나, 조건을 만족하는 첫 원소를 선택한다. 하지만 모든 컬렉션에서 "첫번째"나 "마지막"의 의미가 잘 정의되어 있는 것은 아니라는 점에 유의하라. 예를 들어 해시 집합은 해시값에 따라 원소를 저장하는데, 이 해시값은 매 실행시마다 변할 수 있다. 이런 경우 해시 집합의 
"첫번째" 원소는 프로그램이 실행될 때마다 바뀔 수 있다. 어떤 컬렉션이 항상 같은 순서로 원소를 표시한다면 이를 _순서있다_ 고 한다. 대부분의 컬렉션은 순서가 있으나, 일부(_e.g._ 해시 집합)는 그렇지 않다 -- 이들은 순서를 포기하는 대신 효율을 택한 것이다. 동일한 테스트를 반복하거나, 디버깅을 할 때 때로 순서가 꼭 필요하다. 이 때문에 모든 스칼라 컬렉션 타입에는 순서가 있는 대체물이 반드시 존재한다. 예를 들어 `HashSet`에 순서가 부여된 것은 `LinkedHashSet`이다.
* **부분 컬렉션 가져오기** 연산에는 `tail`, `init`, `slice`, `take`, `drop`, `takeWhile`, `dropWhile`, `filter`, `filterNot`, `withFilter` 등이 있다. 이들은 모두 어떤 첨자 범위나 술어 함수에 의해 식별되는 부분 컬렉션을 반환한다. 
* **분할** 연산인 `splitAt`, `span`, `partition`, `groupBy` 등은 대상 컬렉션의 원소를 구분해서 여러 부분 컬렉션으로 나눈다.
* **원소 테스트** 연산 `exists`, `forall`, `count`는 주어진 술어 함수를 가지고 컬렉션 원소들을 검사한다.
* **폴드** 연산 `foldLeft`, `foldRight`, `/:`, `:\`, `reduceLeft`, `reduceRight`는 인접한 두 원소에 대해 이항 연산자(binary operator)를 반복적용한다.
* **특정 폴드** `sum`, `product`, `min`, `max`는 특정 타입(비교가능하거나 수)의 컬렉션에만 작용한다.
* **문자열** 연산 `mkString`, `addString`, `stringPrefix`는 컬렉션을 문자열로 바꾸는 여러가지 방법을 제공한다.
* **뷰** 연산은 `view` 메소드를 오버로딩한 두 메소드이다. 뷰는 지연 계산될 수 있는 컬렉션이다. 뷰에 대해서는 [나중에](#Views) 	다룰 것이다.

### Traversable에 정의되어 있는 연산들 ###

| 쓰는법 	  	    | 하는일				     |
| ------       	       	    | ------					     |
|  **추상 메소드:**     |						     |
|  `xs foreach f`	    |함수 `f`를 `xs`의 모든 원소에 적용한다.|
|  **병합:**     	    |						     |
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
|  `xs mkString (start, sep, end)`|컬렉션 `xs`의 모든 원소를 `sep`로 구분해 문자열 `start`와 `end` 사이에 넣는다. `start`, `sep`, `end`를 지정하지 않으면 .|
|  `xs.stringPrefix`	    |`xs.toString`이 반환하는 문자열의 맨 앞에 표시될 컬렉션 이름.|
|  **뷰:**               |						     |
|  `xs.view`	    	    |`xs`에 대한 뷰를 만든다.|
|  `xs view (from, to)`     |`xs`에 대해 첨자 범위에 속하는 원소에 대한 뷰를 만든다.|
