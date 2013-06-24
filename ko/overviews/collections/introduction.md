---
layout: overview-large
title: 컬렉션 소개

disqus: true

partof: collections
num: 1
language: ko
---

**Martin Odersky와 Lex Spoon**
  
대부분의 사람들에게 있어 스칼라 2.8의 가장 중요한 변화는 새 컬렉션 프레임워크일 것이다. 
스칼라에는 예전부터 컬렉션이 포함되어 있었다(실제 새 프레임워크도 이전 컬렉션과 상당부분 호환성이 있다). 
하지만 다양한 컬렉션 유형을 포괄하는 일반적이면서 균일한 프레임워크를 제공하는 것은 스칼라 2.8 부터이다.

처음 봤을 땐 컬렉션에 추가된 내용이 어떤 것인지 잘 알기 어려울 수 있다. 하지만, 그 변화가 여러분의 프로그래밍 
스타일에 미칠 영향은 심오하다. 실제로, 컬렉션의 개별 원소 보다는 전체 컬렉션을 프로그램의 기본 구성 요소로 
사용해 더 상위 레벨에서 작업하는 것 처럼 느끼게 될 것이다. 이런 프로그래밍 스타일에 익숙해지려면 적응이 필요하다. 
다행히 새 스칼라 컬렉션이 제공하는 몇몇 특징으로 인해 쉽게 적응할 수 있다. 새 스칼라 컬렉션은 쓰기 쉽고, 
간결하며, 안전하고, 일반적이다.

**사용 용이성:** 대부분의 경우 컬렉션과 관련된 문제를 몇 연산을 거쳐 해결하는데 필요한 메소드는 20-50개 정도이다. 복잡하게 루프를 돌거나 재귀 호출을 하기 위해 끙끙거릴 필요가 없다. 
영속적인 컬렉션과 부작용이 없는 연산을 사용하면 실수로 기존 컬렉션을 오염시킬까 염려할 필요가 없다. 이터레이터와 컬렉션 업데이트간의 간섭도 없다. 

**간결성:** 하나 이상의 루프를 통해 작업해야 했던 것을 한 단어로 수행할 수 있다. 간결한 문법으로 연산을 표현할 수 있고, 각 연산을 힘들이지 않고 조합할 수 있다. 따라서 
컬렉션 전용 대수식을 사용하는 것 같은 느낌을 받게될 것이다.

**안전성:** This one has to be experienced to sink in. The
statically typed and functional nature of Scala's collections means
that the overwhelming majority of errors you might make are caught at
compile-time. The reason is that (1) the collection operations
themselves are heavily used and therefore well
tested. (2) the usages of the collection operation make inputs and
output explicit as function parameters and results. (3) These explicit
inputs and outputs are subject to static type checking. The bottom line
is that the large majority of misuses will manifest themselves as type
errors. It's not at all uncommon to have programs of several hundred
lines run at first try.

**Fast:** Collection operations are tuned and optimized in the
libraries. As a result, using collections is typically quite
efficient. You might be able to do a little bit better with carefully
hand-tuned data structures and operations, but you might also do a lot
worse by making some suboptimal implementation decisions along the
way.  What's more, collections have been recently adapted to parallel
execution on multi-cores. Parallel collections support the same
operations as sequential ones, so no new operations need to be learned
and no code needs to be rewritten. You can turn a sequential collection into a
parallel one simply by invoking the `par` method.

**Universal:** Collections provide the same operations on
any type where it makes sense to do so. So you can achieve a lot with
a fairly small vocabulary of operations. For instance, a string is
conceptually a sequence of characters. Consequently, in Scala
collections, strings support all sequence operations. The same holds
for arrays.

**Example:** Here's one line of code that demonstrates many of the 
advantages of Scala's collections.

    val (minors, adults) = people partition (_.age < 18)

It's immediately clear what this operation does: It partitions a
collection of `people` into `minors` and `adults` depending on
their age. Because the `partition` method is defined in the root
collection type `TraversableLike`, this code works for any kind of
collection, including arrays. The resulting `minors` and `adults`
collections will be of the same type as the `people` collection.

This code is much more concise than the one to three loops required for
traditional collection processing (three loops for an array, because
the intermediate results need to be buffered somewhere else).  Once
you have learned the basic collection vocabulary you will also find
writing this code is much easier and safer than writing explicit
loops. Furthermore, the `partition` operation is quite fast, and will
get even faster on parallel collections on multi-cores.  (Parallel
collections have been released
as part of Scala 2.9.)

This document provides an in depth discussion of the APIs of the
Scala collections classes from a user perspective.  They take you on
a tour of all the fundamental classes and the methods they define.