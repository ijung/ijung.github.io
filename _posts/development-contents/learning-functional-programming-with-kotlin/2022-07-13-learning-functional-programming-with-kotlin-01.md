---
title: 코틀린으로 배우는 함수형 프로그래밍 - 1장 함수형 프로그래밍이란?
author: June
date: 2022-07-13 17:59:00 +0900
categories: [개발 책, 코틀린으로 배우는 함수형 프로그래밍]
tags: [공부, 책, 코틀린으로 배우는 함수형 프로그래밍, 코틀린, kotlin, 함수형 프로그래밍, funtional programming]
toc: true
math: true
mermaid: true
comments: true
---
# 범위
- 1장 함수형 프로그래밍이란?

# 요약
- 함수형 프로그래밍의 특징은 다음과 같다.
    - 불변성(immutable)
    - 참조 투명성(referential transparency)
    - 일급 함수(first-class function)
    - 게으른 평가(lazy evaluation)

# 개념 정리
- 함수형 프로그래밍(functional programming, FP): 함수를 사용해서 데이터 처리의 참조 투명성을 보장하고 상태와 가변 데이터 생성을 피하는 프로그래밍 패러다임.
- 순수한 함수(pure function):  부수 효과가 없는 함수, 즉, 함수의 실행이 외부에 영향을 끼치지 않는 함수를 뜻한다. 
- 부수효과(side-effect): 함수가 실행되는 과정에서 외부의 상태(데이터)를 사용 또는 수정하는 것. 함수의 반환값이 아닌, 외부의 상태에 영향을 미치는 것.
- 참조 투명성(referential transparency): 프로그램의 변경 없이 어떤 표현식을 값으로 대체할 수 있다는 뜻.
- 일급 객체(first-class object): 다음 세가지 조건을 만족시키는 객체.
    - 객체를 함수의 매개변수로 넘길 수 있다.
    - 객체를 함수의 반환값으로 돌려 줄 수 있다.
    - 객체를 변수나 자료구조에 감을 수 있다.
- 일급 함수(first-class funtion): 다음 조건들을 만족하는 함수.
    - 함수를 함수의 매개변수로 넘길 수 있다.
    - 함수를 함수의 반환값으로 돌려줄 수 있다.
    - 함수를 변수나 자료구조에 담을 수 있다.
- 게으른 평가(lazy evaluation): 계산의 결과값이 필요할 때까지 계산을 늦추는 기법.

# 책에서 기억하고 싶은 내용
- 1.1 함수형 프로그래밍의 특징
    - 함수형 프로그래밍(functional programming, FP)은 함수를 사용해서 데이터 처리의 참조 투명성을 보장하고 상태와 가변 데이터 생성을 피하는 프로그래밍 패러다임이다.
    - 함수형 프로그래밍의 특징은 다음과 같다.
        - 불변성(immutable)
        - 참조 투명성(referential transparency)
        - 일급 함수(first-class function)
        - 게으른 평가(lazy evaluation)
    - 함수형 프로그래밍으로 프로그램을 개발하면 다음과 같이 여러 이점이 있다.
        - 부수효과가 없는 프로그램을 만들 수 있어 동시성 프로그래밍에 적합하다.
        - 코드의 복잡도가 낮아 간결한 코드를 만들 수 있고, 모듈성이 높아져 유지보수하기 쉽다.
        - 프로그램의 예측성을 높여 컴파일러가 효율적으로 실행되는 코드를 만들어준다.
- 1.2 순수한 함수란 무엇인가?
    - 순수한 함수(pure function)란, 부작용(side-effect)이 없는 함수, 즉, 함수의 실행이 외부에 영향을 끼치지 않는 함수를 뜻한다.  따라서 순수한 함수는 스레드 안전하고, 병렬적인 계산이 가능하다.[[함수형 프로그래밍](https://ko.wikipedia.org/wiki/%ED%95%A8%EC%88%98%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)]
    - 순수한 함수에는 다름과 같은 특징이 있다.
        - 동일한 입력으로 실행하면 동일한 결과가 나온다.
        - 부수효과가 없다.
            - 부수효과란 함수가 실행되는 과정에서 외부의 상태(데이터)를 사용 또는 수정하는 걸 말한다.
    - 순수한 함수의 특징 덕에 참조 투명성(referential transparency)을 만족하게 된다.
- 1.3 부수효과 없는 프로그램 작성하기
    - 부수효과는 함수의 반환값이 아닌, 외부의 상태에 영향을 미치는 것을 말한다.
- 1.4 참조 투명성으로 프로그램을 더 안전하게 만들기
    - 참조 투명성이란, 프로그램의 변경 없이 어떤 표현식을 값으로 대체할 수 있다는 뜻이다.
    - 참조 투명성은 프로그래머나 컴파일러가 평가 결과를 추론할 수 있게 한다. 그래서 프로그램이 실행되기 전에 컴파일러가 코드를 최적화하거나 코드가 평가되는 시점을 늦출 수 있다.
- 1.5 일급 함수란?
    - 일급 객체(first-class object)
        - 일급 객체는 다음 세가지 조건을 만족시키는 객체를 의미한다.
            - 객체를 함수의 매개변수로 넘길 수 있다.
            - 객체를 함수의 반환값으로 돌려 줄 수 있다.
            - 객체를 변수나 자료구조에 감을 수 있다.
    - 일급 함수(first-class funtion)
        - 다음 조건들을 만족하는 함수는 일급 함수라고 할 수 있다.
            - 함수를 함수의 매개변수로 넘길 수 있다.
            - 함수를 함수의 반환값으로 돌려줄 수 있다.
            - 함수를 변수나 자료구조에 담을 수 있다.
        - 일급 함수를 통해서 더 높은 추상화가 가능하고, 코드의 재사용성을 높일 수 있다.
- 1.6 일급 함수를 이용한 추상화와 재사용성 높이기
- 1.7 게으른 평가로 무한 자료구조 만들기
    - 느긋한 계산법(Lazy evaluation)은 계산의 결과값이 필요할 때까지 계산을 늦추는 기법이다.[[느긋한 계산법](https://ko.wikipedia.org/wiki/%EB%8A%90%EA%B8%8B%ED%95%9C_%EA%B3%84%EC%82%B0%EB%B2%95)]
    - 함수형 언어는 기본적으로 값이 필요한 시점에 평가(lazy evaluation)되고 프로그래머가 평가 시점을 지정할 수도 있다.
    - by lazy는 여러 번 호출되더라도 최초 에 한 번만 평가를 실행한다. 그리고 내부적으로 결괏값을 저장해 두고 필요할 때 가져온다.
    - 함수형 언어에서는 게으른 평가라는 특성을 활용하여 무한대 값을 자료구조에 저장할 수 있다.

<table data-layout="default">
    <colgroup>
        <col style="width: 71.0px;" />
        <col style="width: 344.5px;" />
        <col style="width: 344.5px;" />
    </colgroup>
    <tbody>
        <tr>
            <th>
            </th>
            <th>
                <p style="text-align: center;"><strong>lazy evaluation</strong></p>
            </th>
            <th>
                <p style="text-align: center;"><strong>eager evaluation</strong></p>
            </th>
        </tr>
        <tr>
            <th>
                <p><strong>code</strong></p>
            </th>
<td markdown="block">

```kotlin
fun main() {
    for(i in 0.. 2) {
        val elapsed: Long = kotlin.system.measureTimeMillis {
            println(lazyValue)
        }
        println("$elapsed ms")
    }
}

val lazyValue: String by lazy {
    Thread.sleep(500)
    println("lazy evaluation")
    "hello"
}
```

</td>
<td markdown="block">

```kotlin
fun main() {
    for(i in 0.. 2) {
        val elapsed: Long = kotlin.system.measureTimeMillis {
            println(eagerValue())
        }
        println("$elapsed ms")
    }
}

var eagerValue: () -> String = {
    Thread.sleep(500)
    println("eager evaluation")
    "hello"
}
```

</td>
        </tr>
        <tr>
            <th>
                <p><strong>output</strong></p>
            </th>
<td markdown="block">

```
lazy evaluation
hello
512 ms
hello
0 ms
hello
0 ms
```

</td>
<td markdown="block">

```
eager evaluation
hello
507 ms
eager evaluation
hello
503 ms
eager evaluation
hello
515 ms
```

</td>
        </tr>
    </tbody>
</table>


# Reference
- [함수형 프로그래밍](https://ko.wikipedia.org/wiki/%ED%95%A8%EC%88%98%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)
- [느긋한 계산법](https://ko.wikipedia.org/wiki/%EB%8A%90%EA%B8%8B%ED%95%9C_%EA%B3%84%EC%82%B0%EB%B2%95)