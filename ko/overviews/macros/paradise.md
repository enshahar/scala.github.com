---
layout: overview-large
title: 매크로 낙원

disqus: true

partof: macros
num: 2
outof: 7
language: ko
---
<span class="label important" style="float: right;">매크로 낙원</span>

**유진 부르마코(Eugene Burmako)**

## 스칼라 2.11.x를 위한 매크로 낙원

매크로 낙원은 공식 스칼라 리포지터리의 실험용 브랜치인 `paradise/macros`의 별명이다. 이 브랜치는 스칼라의 안전성을 헤치지 않으면서 빠르게 매크로를 개발할 수 있도록 고안되었다. [나의 설명](http://scalamacros.org/news/2012/12/18/macro-paradise.html)을 참고하라.

소나타입(Sonatype)에 스냅샷 아티펙트를 보내는 야간 빌드 시스템을 만들었다. [https://github.com/scalamacros/sbt-example-paradise](https://github.com/scalamacros/sbt-example-paradise)를 보면 이 야간빌드를 SBT에서 사용하는 방법에 대한 완전한 설명을 볼 수 있다. 간략히 말하면, 매크로 낙원에서 놀기 위해서는 단지 빌드 스크립트에 다음 세 줄을 추가하면 된다(물론 이미 매크로 사용을 위해 [SBT를 설정](/overviews/macros/overview.html#using_macros_with_maven_or_sbt)해 두었어야 한다).

    scalaVersion := "2.11.0-SNAPSHOT"
    scalaOrganization := "org.scala-lang.macro-paradise"
    resolvers += Resolver.sonatypeRepo("snapshots")

현재 SBT가 변경된 `scala-compiler.jar`를 새로운 스냅샷 버전으로 업데이트하는데 문제가 있다. 증상은 다음과 같다. 매크로 낙원을 사용하는 프로젝트를 처음 컴파일할 때는 모든 것이 잘 동작한다. 하지만, 며칠 지나 `sbt update`를 하면 SBT가 `scala-library.jar`과 `scala-reflect.jar`는 야간빌드버전으로 업데이트하지만, `scala-compiler.jar`는 업데이트하지 않는다. 해결책은 `sbt reboot full`를 사용해 SBT 자체와 하부의 스칼라 컴파일러 인스턴스를 다시 다운로드하는 것이다. 이 심각한 문제를 해결하기 위해 계속해서 노력중이다. 원한다면 [메일링 리스트에서](https://groups.google.com/forum/?fromgroups=#!topic/simple-build-tool/UalhhX4lKmw/discussion) 이 문제에 대한 논의에 참여할 수도 있다.

매크로 낙원 야간빌드에 대한 스칼라독(scaladoc) 문서는 [젠킨스 서버](https://scala-webapps.epfl.ch/jenkins/view/misc/job/macro-paradise-nightly-main/ws/dists/latest/doc/scala-devel-docs/api/index.html)에 있다. 예를 들어 최상위 정의를 다루기 위한 새로운 API에 대한 문서가 [scala.reflect.macros.Synthetics](https://scala-webapps.epfl.ch/jenkins/view/misc/job/macro-paradise-nightly-main/ws/dists/latest/doc/scala-devel-docs/api/index.html#scala.reflect.macros.Synthetics)에 있다.

## 2.10.x를 위한 매크로 낙원

스칼라 2.10.x <span class="label success">NEW</span>를 대상으로 하는 매크로 낙원의 특별 빌드도 있다. 매크로 낙원 2.10을 가지고 **스칼라 2.10.x의 프로덕션 버전에서도 유사인용(quasi-quotes)을 사용** 가능하다. 매크로를 `2.10.2-SNAPSHOT` 매크로 낙원 빌드로 컴파일하면 이 매크로는 바닐라 2.10.x 컴파일러에서 아무 문제 없이 사용 가능할 것이다. 이렇게 깔끔하게 동작하는 이유는 유사인용 자체가 매크로이기 때문에 일단 확장되고 나면 매크로 낙원에 대한 아무런(사실은 거의) 의존성이 남지 않기 때문이다. [https://github.com/scalamacros/sbt-example-paradise210](https://github.com/scalamacros/sbt-example-paradise210)에서 이 방법에 대한 완전한 설명을 볼 수 있다.

바이너리 호환성 문제로 인해 2.10.x를 위한 매크로 낙원에는 유사인용을 제외한 나머지 매크로 낙원 2.11.x 기능이 포함되어 있지 않다. 현재의 호환성 정책 하에서는 새로운 메소드가 포함된 새 공개 API를 2.10.x 버전들의 마이너 릴리즈에 업데이트할 수가 없다. 그리고 전체 낙원 기능을 사용하려면 이 새 API가 꼭 필요하다. 따라서 스칼라 컴파일러 2.10.2와 2.10.x의 매크로 낙원의 차이는 유사인용이 추가되었다는 점 뿐이다.