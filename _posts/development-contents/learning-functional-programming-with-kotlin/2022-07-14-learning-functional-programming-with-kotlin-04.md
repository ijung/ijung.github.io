---
title: 코틀린으로 배우는 함수형 프로그래밍 - 4장 고차 함수
author: June
date: 2022-07-14 22:44:00 +0900
categories: [개발 책, 코틀린으로 배우는 함수형 프로그래밍]
tags: [공부, 책, 코틀린으로 배우는 함수형 프로그래밍, 코틀린, kotlin, 함수형 프로그래밍, funtional programming]
toc: true
math: true
mermaid: true
comments: true
---
# 범위
- 4장 고차 함수

# 요약
- 고차 함수, 부분 함수, 부분 적용 함수, 커링, 합성 함수는 함수형 프로그래밍을 하는데 기본적인 개념들이다.

# 개념 정리
- 고차 함수(higher order function): 함수형 프로그래밍에서 다음 두 가지 조건 중 하나 이상을 만족하는 함수.
    - 함수를 매개변수로 받는 함수
    - 함수를 반환하는 함수.
- 부분 함수: 모든 가능한 입력 중, 일부 입력에 대한 결과만 정의한 함수를 의미한다.
- 커링(currying): 여러 개의 매개변수를 받는 함수를 분리하여, 단일 매개변수를 받는 부분 적용 함수의 체인으로 만드는 방법이다.
- 합성 함수:  함수를 매개변수로 받고, 함수를 반환할 수 있는 고차 함수를 이용해서 두 개의 함수를 결합하는 것.
- 포인트 프리 스타일(point free style) 프로그래밍: 함수 합성을 사용해서 매개변수나 타입 선언 없이 함수를 만드는 방식.

# 책에서 기억하고 싶은 내용
- 고차 함수를 이용하면 기능을 확장하기도 쉽다.
- 코틀린은 매개변수로 받는 값이 하나인 경우에 it으로 생략이 가능하다.
- 커링으 장점 중 하나는 이런 부분 적용 함수를 다양하게 재사용할 수 있다는 점이다. 또한 마지막 매개변수가 입력될 때까지 함수를 실행을 늦출 수 있다.
- 고차 함수, 부분 함수, 부분 적용 함수, 커링, 합성 함수는 함수형 프로그래밍을 하는데 기본적인 개념들이다.

# 연습 문제
## 4-1

```kotlin
fun invokeOrElse(p: P, default: R): R = when {
    condition(p) -> f(p)
    else -> default
}

fun orElse(that: PartialFunction<P, R>): PartialFunction<P, R> =
    PartialFunction({ if(this.condition(it)) this.condition(it) else that.condition(it) } , {
        if(this.condition(it)) this.f(it) else that.f(it)
    })
    
fun main() {
    val condition: (Int) -> Boolean = { 0 == it.rem(2) }
    val body: (Int) -> String = { "$it is even" }

    val isEven = body.toPartialFunction(condition)
    val isOdd = { i: Int -> "$i is odd" }.toPartialFunction{ !condition(it) }
    
    println(listOf(1, 2, 3).map { isEven.invokeOrElse(it, "$it is odd") })    // [1 is odd, 2 is even, 3 is odd]
    println(listOf(1, 2, 3).map { isEven.orElse(isOdd).invoke(it) })    // [1 is odd, 2 is even, 3 is odd]
}
```

## 4-2

```kotlin
fun <P1, P2, P3, R>((P1, P2, P3) -> R).partial1(p1: P1): (P2, P3) -> R {
    return { p2, p3 -> this(p1, p2, p3) }
}

fun <P1, P2, P3, R>((P1, P2, P3) -> R).partial2(p2: P2): (P1, P3) -> R {
    return { p1, p3 -> this(p1, p2, p3) }
}

fun <P1, P2, P3, R>((P1, P2, P3) -> R).partial3(p3: P3): (P1, P2) -> R {
    return { p1, p2 -> this(p1, p2, p3) }
}

fun main() {
    val func = { a: Int, b: Int, c: Int -> a + b + c }

    val partiallyAppliedFunc1 = func.partial1(1)
    require(6 == partiallyAppliedFunc1(2, 3))

    val partiallyAppliedFunc2 = func.partial2(2)
    require(6 == partiallyAppliedFunc2(1, 3))

    val partiallyAppliedFunc3 = func.partial3(3)
    require(6 == partiallyAppliedFunc3(1, 2)) is odd]
}
```

## 4-3

```kotlin
fun max(a: Int) = { b: Int -> if(a>=b) a else b }

fun main() {
    require(30 == max(10)(30))
}
```

## 4-4

```korlin
val min = { a: Int, b: Int -> if(a <= b) a else b }
val curriedMin = min.curried()

fun main() {
    val min = { a: Int, b: Int -> if(a <= b) a else b }
    val curriedMin = min.curried()
    println(curriedMin(10)(30))    // 10
}
```

## 4-5

```kotlin
fun power(a: Int): Int = a * a

tailrec fun max(inputs: List<Int>, acc: Int = Int.MIN_VALUE): Int = when {
    inputs.isEmpty() -> acc
    else -> max(inputs.drop(1), max(inputs.first(), acc))
}

fun max(a: Int, b: Int) = if(a>=b) a else b

fun powerWithMax(list: List<Int>) = power(max(list))

fun main() {
    val list = listOf(1, 2, 3, 4, 5, 6, 7)
    val list2 = listOf(10, 2, 13, 4, 0, 6, 1)

    require(powerWithMax(list) == 49)
    require(powerWithMax(list2) == 169)
}
```

## 4-6

```kotlin
fun power(a: Int): Int = a * a

tailrec fun max(inputs: List<Int>, acc: Int = Int.MIN_VALUE): Int = when {
    inputs.isEmpty() -> acc
    else -> max(inputs.drop(1), max(inputs.first(), acc))
}

fun max(a: Int, b: Int) = if(a>=b) a else b

fun powerWithMax(list: List<Int>) = power(max(list))

infix fun <P1, P2, R> ((P2) -> R).compose(g: (P1) -> P2): (P1) -> R {
    return { gInput: P1 -> this(g(gInput)) }
}

val power = { i: Int -> power(i) }
val max = { list: List<Int> -> max(list) }
val powerComposedMax = power compose max

fun main() {
    val list = listOf(1, 2, 3, 4, 5, 6, 7)
    val list2 = listOf(10, 2, 13, 4, 0, 6, 1)

    val power = { i: Int -> power(i) }
    val max = { list: List<Int> -> max(list) }
    val powerComposedMax = power compose max

    require(powerComposedMax(list) == 49)
    require(powerComposedMax(list2) == 169)
}
```

## 4-7

```kotlin
tailrec fun <P> takeWhile(func: (P) -> Boolean, list: List<P>, acc: List<P> = listOf()): List<P> = when {
    list.isEmpty() -> acc
    else -> {
        val newAcc = if(func(list.first())) acc + list.first() else acc
        takeWhile(func, list.drop(1), newAcc)
    }
}

fun main() {
    require(listOf(1, 2) == takeWhile({ p -> p < 3 }, listOf(1, 2, 3, 4, 5)))
}
```

## 4-8

```kotlin
tailrec fun <P> takeWhile2(func: (P) -> Boolean, sequence: Sequence<P>, acc: List<P> = listOf()): List<P> = when {
    !func(sequence.first()) -> acc
    else -> takeWhile2(func, sequence.drop(1), acc + sequence.first())
}

fun main() {
    require(listOf(1, 2, 3, 4, 5, 6, 7, 8, 9) == takeWhile2({ p -> p < 10 }, generateSequence(1) { it + 1 }))
}
```

```kotlin
// 인터넷에서 주워온 코드를 수정한 것.
// 가장 심플하고, 입력값을 수정하는 것이 아닌 신규 Array를 생성하여 리턴한다.
fun getSortedArrayByQuickSort(array: IntArray): IntArray {
    if (array.size < 2) {
        return array
    }

    val pivot = array.first()
    val left = array.filter { it < pivot }.toIntArray()
    val right = array.filter { it > pivot }.toIntArray()

    return getSortedArrayByQuickSort(left) + pivot + getSortedArrayByQuickSort(right)rkqt
}
```