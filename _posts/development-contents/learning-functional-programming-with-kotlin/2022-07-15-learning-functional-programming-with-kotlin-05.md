---
title: 코틀린으로 배우는 함수형 프로그래밍 - 5장 컬렉션으로 데이터 다루기
author: June
date: 2022-07-15 00:40:00 +0900
categories: [개발 책, 코틀린으로 배우는 함수형 프로그래밍]
tags: [공부, 책, 코틀린으로 배우는 함수형 프로그래밍, 코틀린, kotlin, 함수형 프로그래밍, funtional programming]
toc: true
math: true
mermaid: true
comments: true
---
# 범위
- 5장 컬렉션으로 데이터 다루기

# 개념 정리
- 컬렉션 다루기에서 명령형 방식 대비 함수형 프로그래밍의 장점
    - 코드가 간결해져서 가독성이 좋다.
    - 결괏값을 저장하기 위해서 별도의 리스트를 생성할 필요가 없다.
    - 비지니스 로직에 집중할 수 있다.
    - 버그가 발생할 확률이 적다.
    - 테스트가 용이하다.
    - 유지보수가 용이하다.
- foldLeft: 컬렉션 값들을 왼쪽에서부터 오른쪽으로 줄여 나가는 함수
- 컬렉션을 사용하면 안 되는 상황
    - 성능에 민감한 프로그램을 작성할 때
    - 컬렉션의 크기가 고정되어 있지 않을 때
    - 고정된 컬렉션 크기가 매우 클 때

# 책에서 기억하고 싶은 내용
- 함수형 프로그래밍에서 사용되는 대표적인 컬렉션으로 리스트, 세트, 배열, 맵 등이 있다.
- 이러한 컬렉션들은 부수효과가 없는 맵, 필터 등 다양한 형태의 고차 함수를 제공한다.
- 이러한 함수들은 콤비네이터(combinator)라고 부르고, 컬렉션의 데이터를 여러 가지 형태로 조작하는 데 사용한다.
- 코틀린의 컬렉션은 기본적으로 값이 즉시 평가(eager evaluation)된다.

# 연습 문제
## 5-1

```kotlin
val intList:FunList<Int> = FunList.Cons(1, FunList.Cons(2, FunList.Cons(3, FunList.Cons(4, FunList.Cons(5, FunList.Nil)))))

fun main() {
    println(intList) // Cons(head=1, tail=Cons(head=2, tail=Cons(head=3, tail=Cons(head=4, tail=Cons(head=5, tail=FunList$Nil@6acbcfc0)))))
}
```

## 5-2

```kotlin
val doubleList:FunList<Double> = FunList.Cons(1.0, FunList.Cons(2.0, FunList.Cons(3.0, FunList.Cons(4.0, FunList.Cons(5.0, FunList.Nil)))))

fun main() {
    println(doubleList) // Cons(head=1.0, tail=Cons(head=2.0, tail=Cons(head=3.0, tail=Cons(head=4.0, tail=Cons(head=5.0, tail=FunList$Nil@6acbcfc0)))))
}
```

## 5-3

```kotlin
fun <T> FunList<T>.getHead(): T = when (this) {
    FunList.Nil -> throw NoSuchElementException()
    is FunList.Cons -> head
}

fun main() {
    println(intList.getHead()) // 1
    println(doubleList.getHead()) // 1.0
}
```

## 5-4

```korlin
tailrec fun <T> FunList<T>.drop(n: Int): FunList<T> = when(n) {
    0 -> this
    else -> {
        if(this == FunList.Nil) throw NoSuchElementException()
        this.getTail().drop(n - 1)
    }
}
fun main() {
    val doubleList:FunList<Double> = FunList.Cons(1.0, FunList.Cons(2.0, FunList.Cons(3.0, FunList.Cons(4.0, FunList.Cons(5.0, FunList.Nil)))))

    println(doubleList.drop(2)) // Cons(head=3.0, tail=Cons(head=4.0, tail=Cons(head=5.0, tail=FunList$Nil@5f184fc6)))
}
```

## 5-5

```kotlin
tailrec fun <T> FunList<T>.dropWhile(p: (T) -> Boolean): FunList<T> = when(this) {
    FunList.Nil -> this
    is FunList.Cons -> if (p(head)) {
        this
    } else {
        tail.dropWhile(p)
    }
}

fun main() {
    val intList:FunList<Int> = FunList.Cons(1, FunList.Cons(2, FunList.Cons(3, FunList.Cons(4, FunList.Cons(5, FunList.Nil)))))

    println(intList.dropWhile { it == 3 }) // Cons(head=3, tail=Cons(head=4, tail=Cons(head=5, tail=FunList$Nil@5f184fc6)))
}
```

## 5-6

```kotlin
tailrec fun <T> FunList<T>.take(n: Int, acc: FunList<T> = FunList.Nil): FunList<T> = when(n) {
    0 -> acc.reverse()
    else -> this.getTail().take(n-1, acc.addHead(this.getHead()))
}

fun main() {
    val intList:FunList<Int> = FunList.Cons(1, FunList.Cons(2, FunList.Cons(3, FunList.Cons(4, FunList.Cons(5, FunList.Nil)))))

    println(intList.take(3)) // Cons(head=1, tail=Cons(head=2, tail=Cons(head=3, tail=FunList$Nil@6acbcfc0)))
}
```

## 5-7

```kotlin
tailrec fun <T> FunList<T>.takeWhile(acc: FunList<T> = FunList.Nil, p:(T) -> Boolean): FunList<T> = when {
    this == FunList.Nil || !p(getHead()) -> acc.reverse()
    else -> getTail().takeWhile(acc.addHead(getHead()), p)
}

fun main() {
    val intList:FunList<Int> = FunList.Cons(1, FunList.Cons(2, FunList.Cons(3, FunList.Cons(4, FunList.Cons(5, FunList.Nil)))))

    println(intList.takeWhile { it < 4 }) // Cons(head=1, tail=Cons(head=2, tail=Cons(head=3, tail=FunList$Nil@6acbcfc0)))
    println(intList.takeWhile { it < 2 }) // Cons(head=1, tail=FunList$Nil@6acbcfc0)
    println(intList.takeWhile { it < 0 }) // FunList$Nil@6acbcfc0
}
```

## 5-8

```kotlin
tailrec fun <T, R> FunList<T>.indexedMap(index: Int = 0, acc: FunList<R> = FunList.Nil, f: (Int, T) -> R): FunList<R> = when(this) {
    FunList.Nil -> acc.reverse()
    is FunList.Cons -> tail.indexedMap(index + 1, acc.addHead(f(index, head)), f)
}

fun main() {
    val intList:FunList<Int> = FunList.Cons(1, FunList.Cons(2, FunList.Cons(3, FunList.Cons(4, FunList.Cons(5, FunList.Nil)))))

    require(intList.indexedMap { index, elm -> index * elm } == funListOf(0, 2, 6, 12, 20))
}
```

## 5-8

```kotlin
tailrec fun <T, R> FunList<T>.indexedMap(index: Int = 0, acc: FunList<R> = FunList.Nil, f: (Int, T) -> R): FunList<R> = when(this) {
    FunList.Nil -> acc.reverse()
    is FunList.Cons -> tail.indexedMap(index + 1, acc.addHead(f(index, head)), f)
}

fun main() {
    val intList:FunList<Int> = FunList.Cons(1, FunList.Cons(2, FunList.Cons(3, FunList.Cons(4, FunList.Cons(5, FunList.Nil)))))

    require(intList.indexedMap { index, elm -> index * elm } == funListOf(0, 2, 6, 12, 20))
}
```

## 5-9

```kotlin
tailrec fun <T, R> FunList<T>.foldLeft(acc: R, f: (R, T) -> R): R = when(this) {
    FunList.Nil -> acc
    is FunList.Cons -> tail.foldLeft(f(acc, head), f)
}

fun FunList<Int>.maximumByFoldLeft(): Int = this.foldLeft(0) {a, b -> if(a > b) a else b}

fun main() {
    val intList:FunList<Int> = FunList.Cons(1, FunList.Cons(2, FunList.Cons(3, FunList.Cons(4, FunList.Cons(5, FunList.Nil)))))

    require(intList.maximumByFoldLeft() == 5)
}
```

## 5-10

```kotlin
/**
 *
 * 연습문제 5-10
 *
 * filter 함수를 foldLeft 함수를 사용해서 재작성 해보자.
 *
 * 힌트: 함수의 선언 타입은 아래와 같다.
 *
 */

tailrec fun <T> FunList<T>.appendTail(value: T, acc: FunList<T> = FunList.Nil): FunList<T> = when(this) {
    FunList.Nil -> FunList.Cons(value, acc).reverse()
    is FunList.Cons -> tail.appendTail(value, acc.addHead(head))
}

fun <T> FunList<T>.filterByFoldLeft(p: (T) -> Boolean): FunList<T> = this.foldLeft(FunList.Nil) { a: FunList<T>, b -> if(p(b)) a.appendTail(b) else a }

fun main() {
    val intList:FunList<Int> = FunList.Cons(1, FunList.Cons(2, FunList.Cons(3, FunList.Cons(4, FunList.Cons(5, FunList.Nil)))))

    require(intList.filterByFoldLeft { it % 2 == 0 } == funListOf(2, 4))
}
```

## 5-11

```kotlin
/**
 *
 * 연습문제 5-11
 *
 * reverse 함수를 foldRight 함수를 사용해서 재작성 해보자.
 *
 * 힌트: 함수의 선언 타입은 아래와 같다.
 *
 *
 *
 * f(1, Cons(2, Cons(3, Cons(4, Cons(5, Nil)))))
 * f(1, f(2, Cons(3, Cons(4, Cons(5, Nil)))))
 * f(1, f(2, f(3, Cons(4, Cons(5, Nil)))))
 * f(1, f(2, f(3, f(4, Cons(5, Nil)))))
 * f(1, f(2, f(3, f(4, f(5, Nil)))))
 * f(1, f(2, f(3, f(4, Cons(5, Nil)))))
 * f(1, f(2, f(3, Cons(5, Cons(4, Nil)))))
 * f(1, f(2, Cons(5, Cons(4, Cons(3, Nil)))))
 * f(1, Cons(5, Cons(4, Cons(3, Cons(2, Nil)))))
 * Cons(5, Cons(4, Cons(3, Cons(2, Cons(1, Nil)))))
 */


fun <T> FunList<T>.reverseByFoldRight(): FunList<T> = this.foldRight(FunList.Nil) { head, acc:FunList<T> -> acc.appendTail(head) }

fun <T, R> FunList<T>.foldRight(acc: R, f: (T, R) -> R): R = when(this) {
    FunList.Nil -> acc
    is FunList.Cons -> f(head, tail.foldRight(acc, f))
}

tailrec fun <T> FunList<T>.appendTail(value: T, acc: FunList<T> = FunList.Nil): FunList<T> = when(this) {
    FunList.Nil -> FunList.Cons(value, acc).reverse()
    is FunList.Cons -> tail.appendTail(value, acc.addHead(head))
}

fun main() {
    val intList:FunList<Int> = FunList.Cons(1, FunList.Cons(2, FunList.Cons(3, FunList.Cons(4, FunList.Cons(5, FunList.Nil)))))

    require(intList.reverseByFoldRight() == funListOf(5, 4, 3, 2, 1))
}
```

## 5-12

```kotlin
/**
 *
 * 연습문제 5-12
 *
 * filter 함수를 foldRight 함수를 사용해서 재작성 해보자.
 *
 * 힌트: 함수의 선언 타입은 아래와 같다.
 *
 */

fun <T> FunList<T>.filterByFoldRight(p: (T) -> Boolean): FunList<T> = this.foldRight(FunList.Nil) { head, acc: FunList<T> -> if(p(head)) acc.addHead(head) else acc }

fun main() {
    val intList:FunList<Int> = FunList.Cons(1, FunList.Cons(2, FunList.Cons(3, FunList.Cons(4, FunList.Cons(5, FunList.Nil)))))
    
    require(intList.filterByFoldRight { it % 2 == 0 } == funListOf(2, 4))
    require(intList.filterByFoldRight { it < 1 } == FunList.Nil)
    require(intList.filterByFoldRight { it < 6 } == funListOf(1, 2, 3, 4, 5))
}
```

## 5-13

```kotlin
/**
 *
 * 연습문제 5-13
 *
 * zip 함수는 3장에서 이미 설명했다. 여기서는 직접 FunList에 zip 함수를 작성해보자.
 *
 * 힌트: 함수의 선언 타입은 아래와 같다.
 *
 */
 
tailrec fun <T, R> FunList<T>.zip(other: FunList<R>, acc: FunList<Pair<T, R>> = FunList.Nil): FunList<Pair<T, R>> = when {
    this == FunList.Nil || other == FunList.Nil-> acc.reverse()
    else -> getTail().zip(other.getTail(), acc.addHead(getHead() to other.getHead()))
}

fun main() {
    val intList:FunList<Int> = FunList.Cons(1, FunList.Cons(2, FunList.Cons(3, FunList.Cons(4, FunList.Cons(5, FunList.Nil)))))
    val charList = funListOf('a', 'b', 'c', 'd', 'e')

    require(intList.zip(charList) == funListOf(1 to 'a', 2 to 'b', 3 to 'c', 4 to 'd', 5 to 'e'))
    require(intList.zip(funListOf('a', 'b', 'c')) == funListOf(1 to 'a', 2 to 'b', 3 to 'c'))
    require(charList.zip(funListOf(1, 2, 3)) == funListOf('a' to 1, 'b' to 2, 'c' to 3))
}
```

## 5-14

```kotlin
/**
 *
 * 연습문제 5-14
 *
 * zip 함수는 리스트와 리스트를 조합해서 리스트를 생성하는 함수이다.
 * 여기서는 리스트의 값을 입력받은 조합 함수에 의해서 연관 자료구조인 맵을 생성하는 associate 함수를 작성해보자.
 *
 * 힌트: 함수의 선언 타입은 아래와 같다
 *
 */

fun <T, R> FunList<T>.associate(f: (T) -> Pair<T, R>): Map<T, R> = when(this) {
    FunList.Nil -> mapOf()
    is FunList.Cons -> tail.associate(f).plus(f(head))
}

fun <T, R> FunList<T>.associateWithFoldRight(f: (T) -> Pair<T, R>): Map<T, R> = foldRight(mapOf()) { head, acc ->  acc.plus(f(head)) }

fun <T, R> FunList<T>.associateWithFoldLeft(f: (T) -> Pair<T, R>): Map<T, R> = foldLeft(mapOf()) { acc, head ->  acc.plus(f(head)) }

fun main() {
    val intList:FunList<Int> = FunList.Cons(1, FunList.Cons(2, FunList.Cons(3, FunList.Cons(4, FunList.Cons(5, FunList.Nil)))))

    require(intList.associate { it to it * 10 } == mapOf(1 to 10, 2 to 20, 3 to 30, 4 to 40, 5 to 50))
    require(intList.associateWithFoldRight { it to it * 10 } == mapOf(1 to 10, 2 to 20, 3 to 30, 4 to 40, 5 to 50))
    require(intList.associateWithFoldLeft { it to it * 10 } == mapOf(1 to 10, 2 to 20, 3 to 30, 4 to 40, 5 to 50))
}
```

## 5-15

```kotlin
/**
 *
 * 연습문제 5-15
 *
 * FunList의 값들을 입력받은 키 생성 함수를 기준으로 맵을 생성하는 groupBy 함수를 작성해보자.
 *
 * 힌트: 함수의 선언 타입은 아래와 같다
 *
 */
 
 fun <T, K> FunList<T>.groupBy(f: (T) -> K): Map<K, FunList<T>> =
    foldRight(emptyMap()) { head, acc ->
        val key = f(head)
        acc.plus(key to acc.getOrElse(key) { funListOf() }.addHead(head))
    }
    
fun main() {
    val intList:FunList<Int> = FunList.Cons(1, FunList.Cons(2, FunList.Cons(3, FunList.Cons(4, FunList.Cons(5, FunList.Nil)))))

    require(intList.groupBy { it } == mapOf(1 to funListOf(1), 2 to funListOf(2), 3 to funListOf(3), 4 to funListOf(4), 5 to funListOf(5)))
    require(intList.groupBy { it % 2 == 0 } == mapOf(false to funListOf(1, 3, 5), true to funListOf(2, 4)))
}
```

## 5-16

```kotlin
/**
 *
 * 연습문제 5-16
 *
 * 앞에서의 예제와 반대로 이번에는 큰 수에서 감소하는 값들을 가진 리스트를 입력으로 할 때를 비교해 보자. 세 가지 함수의 성능을 비교하고 테스트 결과를 분석해 보자.
 *
 * 힌트: 함수의 선언 타입은 아래와 같다.
 *
 */

private fun imperativeWay(intList: List<Int>): Int {
    for(int in intList) {
        val double = int * int
        if(double < 10) {
            return double
        }
    }
    throw NoSuchElementException("There is no value")
}

private fun functionalWay(intList: List<Int>): Int = intList.map { n -> n * n }.first { n -> n < 10 }


private fun realFunctionalWay(intList: List<Int>): Int = intList.asSequence().map { n -> n * n }.first { n -> n < 10 }

fun main() {
    val bigIntList = (10000000 downTo 1).toList()

    var start = System.currentTimeMillis()
    imperativeWay(bigIntList)
    println("${System.currentTimeMillis() - start} ms")    // 0 ms

    start = System.currentTimeMillis()
    functionalWay(bigIntList)
    println("${System.currentTimeMillis() - start} ms")    // 640 ms

    start = System.currentTimeMillis()
    realFunctionalWay(bigIntList)
    println("${System.currentTimeMillis() - start} ms")    // 14 ms
}
```

## 5-17

```kotlin
/**
 *
 * 연습문제 5-17
 *
 * FunList에서 작성했던 sum 함수를 FunStream에도 추가하자.
 *
 * 힌트: 함수의 선언 타입은 아래와 같다.
 *
 */

fun FunStream<Int>.sum(): Int = foldLeft(0) {acc, head -> acc + head}

tailrec fun <T, R> FunStream<T>.foldLeft(acc: R, f: (R, T) -> R): R = when(this) {
    FunStream.Nil -> acc
    is FunStream.Cons -> tail().foldLeft(f(acc, head()), f)
}

sealed class FunStream<out T> {
    object Nil: FunStream<Nothing>()
    data class Cons<out T>(val head: () -> T, val tail: () -> FunStream<T>): FunStream<T>() {
        override fun equals(other: Any?): Boolean =
            if (other is Cons<*>) {
                if (head() == other.head()) {
                    tail() == other.tail()
                } else {
                    false
                }
            } else {
                false
            }

        override fun hashCode(): Int {
            var result = head.hashCode()
            result = 31 * result + tail.hashCode()
            return result
        }
    }
}

fun <T> FunStream<T>.getHead(): T = when(this) {
    FunStream.Nil -> throw NoSuchElementException()
    is FunStream.Cons -> head()
}

fun <T> FunStream<T>.getTail(): FunStream<T> = when(this) {
    FunStream.Nil -> throw NoSuchElementException()
    is FunStream.Cons -> tail()
}

fun <T> funStreamOf(vararg elements: T): FunStream<T> = elements.toFunStream()

private fun <T> Array<out T>.toFunStream(): FunStream<T> = when {
    this.isEmpty() -> FunStream.Nil
    else -> FunStream.Cons( { this.first() } , { this.copyOfRange(1, this.size).toFunStream() } )
}

fun IntProgression.toFunStream(): FunStream<Int> = when {
    step > 0 -> when {
        first > last -> FunStream.Nil
        else -> FunStream.Cons({ first }, { ((first + step)..last step step).toFunStream() })
    }
    else -> when {
        first >= last -> {
            FunStream.Cons({ first },
                { IntProgression.fromClosedRange(first + step, last, step).toFunStream() })
        }
        else -> {
            FunStream.Nil
        }
    }
}

fun main() {
    require(funStreamOf(1, 2, 3, 4, 5).sum() == 1 + 2 + 3 + 4 + 5)
    require((1..10000).toFunStream().sum() == 50005000)
}
```

## 5-18

```kotlin
/**
 *
 * 연습문제 5-18
 *
 * FunList에서 작성했던 product 함수를 FunStream에도 추가하자.
 *
 * 힌트: 함수의 선언 타입은 아래와 같다.
 *
 */

fun FunStream<Int>.product(): Int = foldLeft(1) {acc, head -> acc * head}

fun main() {
    require(funStreamOf(1, 2, 3, 4, 5).product() == 1 * 2 * 3 * 4 * 5)
}
```

## 5-19

```kotlin
/**
 *
 * 연습문제 5-19
 *
 * FunList에서 작성했던 appendTail 함수를 FunStream에도 추가하자.
 *
 * 힌트: 함수의 선언 타입은 아래와 같다.
 *
 */
 
fun <T> FunStream<T>.appendTail(value: T): FunStream<T> = when(this) {
    FunStream.Nil -> FunStream.Cons({value}, {FunStream.Nil})
    is FunStream.Cons -> FunStream.Cons(head) { tail().appendTail(value) }
}

fun main() {
    require(funStreamOf(1, 2, 3, 4, 5).appendTail(6) == funStreamOf(1, 2, 3, 4, 5, 6))
}
```

## 5-20

```kotlin
/**
 *
 * 연습문제 5-20
 *
 * FunList에서 작성했던 filter 함수를 FunStream에도 추가하자.
 *
 * 힌트: 함수의 선언 타입은 아래와 같다.
 *
 */
 
fun <T> FunStream<T>.filter(p: (T) -> Boolean): FunStream<T> = this.foldLeft(funStreamOf()) { acc, head -> if(p(head)) acc.appendTail(head) else acc }

// 최적화된 버전
fun <T> FunStream<T>.filterWithOptimized(p: (T) -> Boolean): FunStream<T> = when (this) {
    FunStream.Nil -> FunStream.Nil
    is FunStream.Cons -> {
        val first = dropWhile(p)
        if (first != FunStream.Nil) {
            FunStream.Cons({ first.getHead() }, { first.getTail().filter(p) })
        } else {
            FunStream.Nil
        }
    }
}

tailrec fun <T> FunStream<T>.dropWhile(p: (T) -> Boolean): FunStream<T> = when(this) {
    FunStream.Nil -> this
    is FunStream.Cons -> if (p(head())) {
        this
    } else {
        tail().dropWhile(p)
    }
}

fun main() {
    require(funStreamOf(1, 2, 3, 4, 5)
        .filter { it % 2 == 0 } == funStreamOf(2, 4))
    require(funStreamOf(1, 2, 3, 4, 5)
        .filter { it > 6 } == FunStream.Nil)
    require((1..100000000)
        .toFunStream()
        .filter { it > 100 }
        .getHead() == 101)
    
    require(funStreamOf(1, 2, 3, 4, 5)
        .filterWithOptimized { it % 2 == 0 } == funStreamOf(2, 4))
    require(funStreamOf(1, 2, 3, 4, 5)
        .filterWithOptimized { it > 6 } == FunStream.Nil)
    require((1..100000000)
        .toFunStream()
        .filterWithOptimized { it > 100 }
        .getHead() == 101)
}
```

## 5-21

```kotlin
/**
 *
 * 연습문제 5-21
 *
 * FunList에서 작성했던 map 함수를 FunStream에도 추가하자.
 *
 * 힌트: 함수의 선언 타입은 아래와 같다.
 *
 */

fun <T, R> FunStream<T>.map(f: (T) -> R): FunStream<R> = when(this) {
    FunStream.Nil -> FunStream.Nil
    is FunStream.Cons -> FunStream.Cons({ f(head()) }, { tail().map(f) })
}

// FoldFeft를 사용한 버전
fun <T, R> FunStream<T>.mapWithFoldLeft(f: (T) -> R): FunStream<R> = this.foldLeft(funStreamOf()) { acc, head -> acc.appendTail(f(head)) }

fun main() {
    require(funStreamOf(1, 2, 3, 4, 5)
        .map { it * 2 } == funStreamOf(2, 4, 6, 8, 10))
    require((1..100000000)
        .toFunStream()
        .map { it * it }
        .filter { it > 100 }
        .getHead() == 121)
    
    require(funStreamOf(1, 2, 3, 4, 5)
        .mapWithFoldLeft { it * 2 } == funStreamOf(2, 4, 6, 8, 10))
    require((1..100000000)
        .toFunStream()
        .mapWithFoldLeft { it * it }
        .filter { it > 100 }
        .getHead() == 121)
}
```

## 5-22

```kotlin
/**
 * 연습문제 5-22
 *
 * ``FunStream``에서 필요한 값을 가져오는 ``take`` 함수를 추가하자. ``FunStream``은 무한대를 표현한 컬렉션이다. ``take`` 함수를 사용하여 값을
 * 5개 가져온 후 합계를 구해 보자.
 *
 * 힌트: 함수의 선언 타입은 아래와 같다.
 */

fun <T> FunStream<T>.take(n: Int): FunStream<T> = when {
    n < 0 -> throw IllegalArgumentException()
    n == 0 || this == FunStream.Nil -> FunStream.Nil
    else -> FunStream.Cons( { getHead() }, { getTail().take(n-1) } )
}

fun <T> generateFunStream(seed: T, generate: (T) -> T): FunStream<T> =
    FunStream.Cons({ seed }, { generateFunStream(generate(seed), generate) })

fun main() {
    require((1..100000000)
        .toFunStream()
        .take(1)
        .getHead() == 1)
    require(generateFunStream(0) { it + 5 }
        .take(5) == funStreamOf(0, 5, 10, 15, 20))
}
```

## 5-23

```kotlin
/**
 *
 * 연습문제 5-23
 *
 * FunList에 toString 함수를 추가해보자.
 *
 * 힌트: 함수의 선언 타입은 아래와 같다.
 *
 */
 
 tailrec fun <T> FunList<T>.toString(acc: String): String = when(this) {
    FunList.Nil -> "[" + acc.drop(2) + "]"
    is FunList.Cons -> tail.toString("$acc, $head")
}

fun main() {
    require(funListOf(1, 2, 3, 4, 5).toString("") == "[1, 2, 3, 4, 5]")
}
```

## 5-24

```kotlin
/**
 *
 * 연습문제 5-24
 *
 * 모든 자연수의 제곱근의 합이 1000을 넘으려면 몇개의 자연수가 필요한지 계산하는 함수를 작성해보자.
 *
 * 힌트: 함수는 꼬리재귀로 작성하자.
 *
 */
 
tailrec fun squareRoot(num: Int = 1, acc: Int = 0): Int = when {
    acc > 1000 -> num - 1
    else -> squareRoot(num + 1, acc + (num * num))
}

fun main() {
    require(squareRoot() == 14)
}
```