---
layout: overview-large
title: 변경 가능한 컬렉션에 속하는 구체적인 클래스들

disqus: true

partof: collections
num: 9
language: ko
---

스칼라 표준 라이브러리가 제공하는 가장 일반적으로 사용되는 변경 불가능한 컬렉션 클래스에 대해 살펴보았다. 이제 변경가능한 컬렉션 클래스를 살펴보자.

## 배열 버퍼

[ArrayBuffer(배열버퍼)](http://www.scala-lang.org/api/{{ site.scala-version }}/scala/collection/mutable/ArrayBuffer.html)는 배열과 크기를 저장한다. 배열 버퍼에 대한 대부분의 연산은 배열에 대한 연산과 같은 복잡도이다. 왜냐하면 이러한 연산들은 단지 하부의 배열을 억세스하거나 변경하기 때문이다. 추가로 배열 버퍼에서는 데이터를 효율적으로 맨 마지막에 추가할 수 있다. 배열 버퍼에 데이터를 추가하는 작업의 상환 시간 복잡도(amortized time complexity)도 상수 시간이다. 따라서 배열 버퍼는 새 데이터가 맨 마지막에만 추가되는 큰 컬렉션을 효율적으로 구축할 때 유용하다.

    scala> val buf = scala.collection.mutable.ArrayBuffer.empty[Int]
    buf: scala.collection.mutable.ArrayBuffer[Int] = ArrayBuffer()
    scala> buf += 1
    res32: buf.type = ArrayBuffer(1)
    scala> buf += 10
    res33: buf.type = ArrayBuffer(1, 10)
    scala> buf.toArray
    res34: Array[Int] = Array(1, 10)

## 리스트 버퍼

[ListBuffer(리스트버퍼)](http://www.scala-lang.org/api/{{ site.scala-version }}/scala/collection/mutable/ListBuffer.html)는 배열 대신 내부적으로 연결된 리스트를 사용한다는 점만 다르고 배열 버퍼와 비슷하다. 만약 컬렉션을 구축한 다음 리스트로 변환할 생각이라면, 배열 버퍼 대신 리스트 버퍼를 사용하도록 하라.

    scala> val buf = scala.collection.mutable.ListBuffer.empty[Int]
    buf: scala.collection.mutable.ListBuffer[Int] = ListBuffer()
    scala> buf += 1
    res35: buf.type = ListBuffer(1)
    scala> buf += 10
    res36: buf.type = ListBuffer(1, 10)
    scala> buf.toList
    res37: List[Int] = List(1, 10)

## 스트링빌더

배열 버퍼가 배열을 만들 때 유용하고, 리스트 버퍼는 리스트를 만들 때 유용한 것과 같이, [StringBuilder(스트링빌더)](http://www.scala-lang.org/api/{{ site.scala-version }}/scala/collection/mutable/StringBuilder.html)는 스트링을 만들 때 유용하다. 스트링빌더는 자주 사용되기 때문에 기본 네임스페이스에 임포트되어 있다. 단지 아래와 같이 `new StringBuilder`라고 하면 만들 수 있다.

    scala> val buf = new StringBuilder
    buf: StringBuilder = 
    scala> buf += 'a'
    res38: buf.type = a
    scala> buf ++= "bcdef"
    res39: buf.type = abcdef
    scala> buf.toString
    res41: String = abcdef

## 연결된 리스트

연결된 리스트는 변경 가능한 열로 각 노드는 다음번 노드를 가리키는 포인터로 서로 연결되어 있다. 이들은 [LinkedList(연결리스트)](http://www.scala-lang.org/api/{{ site.scala-version }}/scala/collection/mutable/LinkedList.html) 클래스로 지원된다. 대부분의 언어에서 `null`이 빈 연결 리스트로 사용된다. 스칼라 컬렉션에서는 그렇게 할 수 없는데 왜냐하면 빈 열도 열이 제공하는 모든 메소드를 지원해야 하기 때문이다. 특히 `LinkedList.empty.isEmpty`가 `true`를 반환해야지 `NullPointerException`을 발생시키면 안된다. 따라서 빈 연결 리스트는 특별한 방식으로 처리된다. 빈 연결리스트의 `next` 필드는 자기 자신을 가리킨다. 변경 불가능한 리스트와 마찬가지로 연결리스트도 순차적으로 순회할 때 가장 성능이 좋다. 추가로 연결리스트를 사용하면 원소나 연결 리스트를 다른 연결 리스트에 추가하는 것이 쉽다.

## Double Linked Lists

Double linked lists are like single linked lists, except that they have besides `next` another mutable field `prev` that points to the element preceding the current node. The main benefit of that additional link is that it makes element removal very fast. Double linked lists are supported by class [DoubleLinkedList](http://www.scala-lang.org/api/{{ site.scala-version }}/scala/collection/mutable/DoubleLinkedList.html).

## Mutable Lists

A [MutableList](http://www.scala-lang.org/api/{{ site.scala-version }}/scala/collection/mutable/MutableList.html) consists of a single linked list together with a pointer that refers to the terminal empty node of that list. This makes list append a constant time operation because it avoids having to traverse the list in search for its terminal node. [MutableList](http://www.scala-lang.org/api/{{ site.scala-version }}/scala/collection/mutable/MutableList.html) is currently the standard implementation of [mutable.LinearSeq](http://www.scala-lang.org/api/{{ site.scala-version }}/scala/collection/LinearSeq.html) in Scala.

## Queues

Scala provides mutable queues in addition to immutable ones. You use a `mQueue` similarly to how you use an immutable one, but instead of `enqueue`, you use the `+=` and `++=` operators to append. Also, on a mutable queue, the `dequeue` method will just remove the head element from the queue and return it. Here's an example:

    scala> val queue = new scala.collection.mutable.Queue[String]
    queue: scala.collection.mutable.Queue[String] = Queue()
    scala> queue += "a"
    res10: queue.type = Queue(a)
    scala> queue ++= List("b", "c")
    res11: queue.type = Queue(a, b, c)
    scala> queue
    res12: scala.collection.mutable.Queue[String] = Queue(a, b, c)
    scala> queue.dequeue
    res13: String = a
    scala> queue
    res14: scala.collection.mutable.Queue[String] = Queue(b, c)

## Array Sequences

Array sequences are mutable sequences of fixed size which store their elements internally in an `Array[Object]`. They are implemented in Scala by class [ArraySeq](http://www.scala-lang.org/api/{{ site.scala-version }}/scala/collection/mutable/ArraySeq.html).

You would typically use an `ArraySeq` if you want an array for its performance characteristics, but you also want to create generic instances of the sequence where you do not know the type of the elements and you do not have a `ClassManifest` to provide it at run-time. These issues are explained in the section on [arrays]({{ site.baseurl }}/overviews/collections/arrays.html).

## Stacks

You saw immutable stacks earlier. There is also a mutable version, supported by class [mutable.Stack](http://www.scala-lang.org/api/{{ site.scala-version }}/scala/collection/mutable/Stack.html). It works exactly the same as the immutable version except that modifications happen in place.

    scala> val stack = new scala.collection.mutable.Stack[Int]           
    stack: scala.collection.mutable.Stack[Int] = Stack()
    scala> stack.push(1)
    res0: stack.type = Stack(1)
    scala> stack
    res1: scala.collection.mutable.Stack[Int] = Stack(1)
    scala> stack.push(2)
    res0: stack.type = Stack(1, 2)
    scala> stack
    res3: scala.collection.mutable.Stack[Int] = Stack(1, 2)
    scala> stack.top
    res8: Int = 2
    scala> stack
    res9: scala.collection.mutable.Stack[Int] = Stack(1, 2)
    scala> stack.pop    
    res10: Int = 2
    scala> stack    
    res11: scala.collection.mutable.Stack[Int] = Stack(1)

## Array Stacks

[ArrayStack](http://www.scala-lang.org/api/{{ site.scala-version }}/scala/collection/mutable/ArrayStack.html) is an alternative implementation of a mutable stack which is backed by an Array that gets re-sized as needed. It provides fast indexing and is generally slightly more efficient for most operations than a normal mutable stack.

## Hash Tables

A hash table stores its elements in an underlying array, placing each item at a position in the array determined by the hash code of that item. Adding an element to a hash table takes only constant time, so long as there isn't already another element in the array that has the same hash code. Hash tables are thus very fast so long as the objects placed in them have a good distribution of hash codes. As a result, the default mutable map and set types in Scala are based on hash tables. You can access them also directly under the names [mutable.HashSet](http://www.scala-lang.org/api/{{ site.scala-version }}/scala/collection/mutable/HashSet.html) and [mutable.HashMap](http://www.scala-lang.org/api/{{ site.scala-version }}/scala/collection/mutable/HashMap.html).

Hash sets and maps are used just like any other set or map. Here are some simple examples:

    scala> val map = scala.collection.mutable.HashMap.empty[Int,String]
    map: scala.collection.mutable.HashMap[Int,String] = Map()
    scala> map += (1 -> "make a web site")
    res42: map.type = Map(1 -> make a web site)
    scala> map += (3 -> "profit!")
    res43: map.type = Map(1 -> make a web site, 3 -> profit!)
    scala> map(1)
    res44: String = make a web site
    scala> map contains 2
    res46: Boolean = false

Iteration over a hash table is not guaranteed to occur in any particular order. Iteration simply proceeds through the underlying array in whichever order it happens to be in. To get a guaranteed iteration order, use a _linked_ hash map or set instead of a regular one. A linked hash map or set is just like a regular hash map or set except that it also includes a linked list of the elements in the order they were added. Iteration over such a collection is always in the same order that the elements were initially added.

## Weak Hash Maps

A weak hash map is a special kind of hash map where the garbage collector does not follow links from the map to the keys stored in it. This means that a key and its associated value will disappear from the map if there is no other reference to that key. Weak hash maps are useful for tasks such as caching, where you want to re-use an expensive function's result if the function is called again on the same key. If keys and function results are stored in a regular hash map, the map could grow without bounds, and no key would ever become garbage. Using a weak hash map avoids this problem. As soon as a key object becomes unreachable, it's entry is removed from the weak hashmap. Weak hash maps in Scala are implemented by class [WeakHashMap](http://www.scala-lang.org/api/{{ site.scala-version }}/scala/collection/mutable/WeakHashMap.html) as a wrapper of an underlying Java implementation `java.util.WeakHashMap`.

## Concurrent Maps

A concurrent map can be accessed by several threads at once. In addition to the usual [Map](http://www.scala-lang.org/api/{{ site.scala-version }}/scala/collection/Map.html) operations, it provides the following atomic operations:

### Operations in class ConcurrentMap

| WHAT IT IS  	  	            | WHAT IT DOES				     |
| ------       	       	        | ------					     |
|  `m putIfAbsent(k, v)`  	    |Adds key/value binding `k -> m` unless `k` is already defined in `m`         |
|  `m remove (k, v)`  	        |Removes entry for `k` if it is currently mapped to `v`.                      |
|  `m replace (k, old, new)`  	|Replaces value associated with key `k` to `new`, if it was previously bound to `old`. |
|  `m replace (k, v)`  	        |Replaces value associated with key `k` to `v`, if it was previously bound to some value.|

`ConcurrentMap` is a trait in the Scala collections library. Currently, its only implementation is Java's `java.util.concurrent.ConcurrentMap`, which can be converted automatically into a Scala map using the [standard Java/Scala collection conversions]({{ site.baseurl }}/overviews/collections/conversions-between-java-and-scala-collections.html).

## Mutable Bitsets

A mutable bit of type [mutable.BitSet](http://www.scala-lang.org/api/{{ site.scala-version }}/scala/collection/mutable/BitSet.html) set is just like an immutable one, except that it is modified in place. Mutable bit sets are slightly more efficient at updating than immutable ones, because they don't have to copy around `Long`s that haven't changed.

    scala> val bits = scala.collection.mutable.BitSet.empty
    bits: scala.collection.mutable.BitSet = BitSet()
    scala> bits += 1
    res49: bits.type = BitSet(1)
    scala> bits += 3
    res50: bits.type = BitSet(1, 3)
    scala> bits
    res51: scala.collection.mutable.BitSet = BitSet(1, 3)


















