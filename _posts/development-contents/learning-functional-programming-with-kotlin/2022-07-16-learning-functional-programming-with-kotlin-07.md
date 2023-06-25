---
title: 코틀린으로 배우는 함수형 프로그래밍 - 7장 펑터
author: June
date: 2022-07-16 13:31:00 +0900
categories: [개발 책, 코틀린으로 배우는 함수형 프로그래밍]
tags: [공부, 책, 코틀린으로 배우는 함수형 프로그래밍, 코틀린, kotlin, 함수형 프로그래밍, funtional programming]
toc: true
math: true
mermaid: true
comments: true
---
# 범위
- 7장 펑터

# 개념 정리
- 펑터(Functor): 매핑할 수 있는 것(can be mapped over)이라는 행위를 선언한 타입 클래스.
- 메이비(Maybe): 어떤 값이 있을 수도 있고 없을 수도 있는 하스켈의 컨테이너형 타입이다. 자바8에는 옵셔널(Optional), 스칼라에서는 옵션(Option)이라는 이름으로 동일한 역할을 하는 컨테이너형 타입을 제공한다. 코틀린에서는 물음표를 사용해서 널에 대한 처리가 가능하기 때문에 내부적으로 메이비와 같은 타입을 지원하지 않는다.
- 이더(Either): 레프트(Left) 또는 라이트(Right) 타입만 허용하는 대수적 타입.
- 일급 함수: 함수에 대해 다음 세 가지 조건을 만족하는 것.
    - 함수를 함수의 매개변h 넘길 수 있다.
    - 함수를 함수의 반환값으로 돌려 줄 수 있다.
    - 함수를 변수나 자료구조에 담을 수 있다.
- 펑터의 법칙(functor law): 펑터가 되기 위해서 만족해야 하는 두 가지 법칙.
    - 항등 함수(identity function)에 펑터를 통해서 매핑하면, 반환되는 펑터는 원래의 펑터와 같다.
    - 두 함수를 합성한 함수의 매핑은 각 함수를 매핑한 결과를 합성한 것과 같다.
    - 펑터 제 1 법칙: fmap을 호출할 때 항등 함수 id를 입력으로 넣은 결과는 반드시 항등 함수를 호출한 결과와 동일해야 한다.
        - 표현식: fmap(identity()) == identity()
    - 펑터 제 2 법칙: 함수 f와 g를 먼저 합성하고 fmap 함수의 입력으로 넣어서 얻은 결괏값은 함수 f를 fmap에 넣어서 얻은 함수와 g를 fmap에 넣어서 얻은 함수를 합성한 결과와 같아야 한다.
        - 표현식: fmap(f compose g) == fmap(f) compose fmap(g)
- 항등 함수: { x → x }와 같이 입력받은 매개 변수를 어떠한 가공 없이 그대로 반환하는 함수.
- 커링: 입력 매개변수가 여러 개인 함수를 입력 매개 변수가 한 개인 함수의 체인으로 만드는 것.

# 책에서 기억하고 싶은 내용
- 함수형 프로그래밍은 카테고리 이론(Category theory)이라는 수학적 원리를 토대로 만들어졌다.
- 어떤 값을 담을 수 있는 타입은 항상 펑터로 만드는 것을 생각해 볼 수 있다.
-  Functor의 타입 생성자는 매개변수가 한 개이기 때문에 타입이 다른 두 개 이상의 매개변수를 가지는 타입을 Functor의 인스턴스로 만들기 위해서는 fmap 함수에 의해서 변경되는 매개변수를 제외한 나머지 값들을 고정해야 한다.
- 함수형 언어에서는 함수도 타입이다.
- Functor 타입 클래스의 타입 생성자는 하나의 매개변수만 가진다. 하지만 함수의 타입은 함수의 매개변수가 여러 개인 경우, 하나 이상의 타입 매개변수를 가질 수 있다. 따라서 변경할 수 있는 타입 한 개를 제외한 나머지는 고정해야 한다.
- 펑터는 왜 펑터의 법칙을 만족하도록 만들어야 할까?
    - 타입이 두 가지 펑터의 법칙을 만족하면 어떻게 동작할 것인가에 대한 동일한 가정을 할 수 있다. 즉, fmap 함수를 호출했을 때, 매핑하는 동작 외에 어떤 것도 하지 않는다는 것을 알 수 있다.
    - 이러한 예측 가능성은 함수가 안정적으로 동작할 뿐만 아니라 더 추상적인 코드로 확장할 때도 도움이 된다.

# 연습 문제
## 7-1

```kotlin
/**
 *
 * 연습문제 7-1
 *
 * 5장에서 만든 FunList를 Functor의 인스턴스로 만들어 보자.
 * FunList에 이미 map 함수 등이 존재하지만, fmap, first, size와 같은 기본적인 기능만 제공하는 형태로 재작성하라.
 *
 * 힌트 : 펑터의 의미에 집중하기 위해 꼬리 재귀나, 효율은 생각하지 않고 작성한다.
 */
 
interface Functor<out A> {
    fun <B> fmap(f: (A) -> B): Functor<B>
}

sealed class FunList<out A>: Functor<A> {
    abstract override fun <B> fmap(f: (A) -> B): FunList<B>
    abstract fun first(): A
    abstract fun size(): Int
}

object Nil: FunList<Nothing>() {
    override fun <B> fmap(f: (kotlin.Nothing) -> B): FunList<B> = Nil
    override fun first(): Nothing = throw NoSuchElementException()
    override fun size(): Int = 0
}

data class Cons<out T>(val head: T, val tail: FunList<T>): FunList<T>() {
    override fun <B> fmap(f: (T) -> B): FunList<B> = Cons(f(head), tail.fmap(f))
    override fun first(): T = head
    override fun size(): Int = tail.size() + 1
}

fun main() {
    val funList: FunList<Int> = Cons(1, Cons(2, Cons(3, Nil)))

    require(funList.fmap { it * 3 } ==
            Cons(3, Cons(6, Cons(9, Nil))))
    require(funList.first() == 1)
    require(funList.size() == 3)

    val funList2: FunList<Int> = Nil

    require(funList2.fmap { it * 3 } == Nil)
//    require(funList2.first()  throw NoSuchElementException())
    require(funList2.size() == 0)
}
```

## 7-2

```kotlin
/**
 *
 * 연습문제 7-2
 *
 * 연습문제에서 만들어본 리스트 펑터인 FunList가 펑터의 법칙을 만족하는지 확인해보자.
 */
 
fun <T> identity(x: T): T = x

infix fun <F, G, R> ((F) -> R).compose(g: (G) -> F): (G) -> R {
    return { gInput: G -> this(g(gInput))}
}

fun main() {
    val funList: FunList<Int> = Cons(1, Cons(2, Cons(3, Nil)))

    // functor 1 lows
    require(Nil.fmap { identity(it) } == identity(Nil))
    require(funList.fmap { identity(it) } == identity(funList))

    // functor 2 lows
    val f = { a: Int -> a + 1 }
    val g = { b: Int -> b * 2 }

    val nilLeft = Nil.fmap(f compose g)
    val nilRight = Nil.fmap(g).fmap(f)
    require(nilLeft == nilRight)

    val funListLeft = funList.fmap(f compose g)
    val funListRight = funList.fmap(g).fmap(f)
    require(funListLeft == funListRight)
}
```