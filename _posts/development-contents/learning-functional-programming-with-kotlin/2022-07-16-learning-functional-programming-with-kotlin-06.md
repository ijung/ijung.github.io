---
title: 코틀린으로 배우는 함수형 프로그래밍 - 6장 함수형 타입 시스템
author: June
date: 2022-07-16 12:56:00 +0900
categories: [개발 책, 코틀린으로 배우는 함수형 프로그래밍]
tags: [공부, 책, 코틀린으로 배우는 함수형 프로그래밍, 코틀린, kotlin, 함수형 프로그래밍, funtional programming]
toc: true
math: true
mermaid: true
comments: true
---
# 범위
- 6장 함수형 타입 시스템

# 개념 정리
- 대수적 데이터 타입(Algebraic data type): 다른 타입들을 모아서 형성되는 합성 타입의 종류로, 곱 타입(product type)과 합 타입(sum type)이 있다. 대수적 데이터 타입의 핵심은 기존 타입들을 결합하여 새로운 타입을 정의하는 것이다.[[대수적 자료형](https://ko.wikipedia.org/wiki/%EB%8C%80%EC%88%98%EC%A0%81_%EC%9E%90%EB%A3%8C%ED%98%95)]
- 곱 타입(product type): 하나의 자료구조에 여러 가지 타입을 한번에 정의할 수 있는 것으로 튜플이나 레코드가 대표적인 예다. 두 개 이상의 타입을 AND로 결합한 형태이다.
- 합 타입(sum type): 합 타입은 곱 타입과 달리 두 개 이상의 타입을 OR로 결합합니다. 코틀린은 sealed class를 사용해서 합 타입을 만든다.
- 타입 변수(type variable): 코틀린에서 제네릭으로 선언된 타입(T).
- 다형 함수(polymorphic function): 타입 변수를 가진 함수.
- 값 생성자(value constructor): 타입에서 타입의 값을 반환하는 것.
- 타입 생성자(type constructor): 새로운 타입을 생성하기 위해서 매개변수화된 타입을 받는 것.
- 인터페이스(interface): 객체지향 프로그래밍에서 클래스의 기능 명세이다. 클래스의 행위를 메스드의 서명(signiture)으로 정의하고, 구현부는 작성하지 않는다.
- 트레이트(trait): 인터페이스와 유사하지만, 구현부를 포함한 메서드를 정의할 수 있다.
- 추상 클래스(abstract class): 상속 관계에서의 추상적인 객체를 나타내기 위해서 사용되는 것이다. 인터페이스나 트레이트와는 사용 목적이 다르다. 모든 종류의 프로퍼티와 생성자를 가질 수 있고, 다중 상속이 불가능하다.
- 믹스인(mixin): 클래스들 간에 어떤 프로퍼티나 메서드를 결합하는 것이다. 메서드 재사용성이 높고 유연하며, 다중 상속에서 발생하는 모호성(diamond problem)도 해결할 수 있다.
- 재귀적 자료구조: 대수적 데이터 타입에서 구성하는 값 생성자의 필드에 자신을 포함하는 구조.

# 책에서 기억하고 싶은 내용
- 컴파일러는 정적 타입 시스템이 주는 정보를 사용해서 리플렉션(reflection), 타입 추론과 같은 고도화된 기능을 제공한다.
- 함수형 언어에서는 객체뿐만이 아니라 표현식(expression)도 타입을 가진다.
- 곱 타입에서 when 구문을 사용할 때는 else 구문을 반드시 작성해야 한다. 곱 타입은 타입을 구성하는 값들의 합이 전체를 의미하지 않는다. 컴파일러는 하위 클래스가 얼마나 있을지 예측할 수 없기 때문에 else 구문에서 다른 입력이 들어오는 경우를 대비해야 한다.
- 합 타입에서 when 구문을 사용할 때는 합타입에서는 부분의 합이 전체가 되기 때문에 else 구문을 작성할 필요가 없다. 컴파일러는 세가지 타입 외에 다른 타입이 들어오지 않을 것이라는 것을 예측할 수 있다.
- 합 타입의 장점
    - 더 쉽게 타입을 결합하고 확장할 수 있음.
    - 생성자 패턴 매칭을 활용해서 간결한 코드를 작성할 수 있음.
    - 철저한 타입 체크로 더 안전한 코드를 작성할 수 있음.
    - 추가적인 else 구문을 작성하지 않아도 되고, 호출자(caller)에서도 예외에 대한 처리를 할 필요가 없다. 즉, 함수에 부수효과가 없고, 참조 투명한 함수를 설계할 수 있다.
    - 타입의 값이 변경될 때 해당 값에 대한 처리가 되어 있지 않은 비지니스 로직에 컴파일 오류를 발생시킨다.
- 객체지향 프로그래밍(OOP)에서 행위를 가진 타입을 정의하는 방법에는 인터페이스(interface), 추상 클래스(abstract class), 트레이트(trait), 믹스인(mixin) 등이 있다.
- 타입 클래스는 다음과 같은 기능을 가지고, 코틀린의 인터페이스와 유사하다.
    - 행위에 대한 선언을 할 수 있다.
    - 필요시, 행위의 구현부도 포함할 수 있다.

# 연습 문제
## 6-1

```kotlin
/**
 *
 * 연습문제 6-1
 *
 * 재귀적 데이터 구조를 활용하여 이진 트리를 만들어 보자. 여기서 이진 트리는 균형잡힌 트리가 아니고 일반적인 트리다. 트리의 정의는 아래와 같다.
 *
 * 힌트 : 모든 노드는 하위 노드가 없거나(EmptyTree) 최대 두개의 하위 노드를 가진다.(Node)
 *       두개의 하위 노드 중, 하나는 왼쪽에 다른 노드는 오른쪽에 있다.
 *       함수의 기본선언은 다음과 같다.
 */

sealed class Tree<out T>

data class Node<T>(
    val value: T,
    val leftTree: Tree<T> = EmptyTree,
    val rightTree: Tree<T> = EmptyTree
) : Tree<T>()

object EmptyTree : Tree<Nothing>()
```

## 6-2

```kotlin
/**
 *
 * 연습문제 6-2
 *
 * 6-1에서 만든 이진 트리에 노드를 추가하는 insert 함수를 Tree의 확장 함수로 만들어보자.
 * 이때 노드의 왼쪽 하위  노드의 값은 오른쪽 하위 노드의 값보다 항상 작아야 한다.
 *
 * 단, 값을 비교하기 위해서는 ``T``가 항상 ``Comparable`` 속성을 가지고 있어야 한다.
 * 여기에서는 문제의 복잡도를 낮추기 위해 입력 타입을 ``Int``로 제한한다.
 *
 * 힌트: 함수의 선언 타입은 아래와 같다.
 */
 
 fun Tree<Int>.insert(elem: Int): Tree<Int> = when(this) {
    EmptyTree -> Node(elem)
    is Node -> when {
        elem <= value -> Node(value, leftTree.insert(elem), rightTree)
        else -> Node(value, leftTree, rightTree.insert(elem))
    }
}

fun main() {
    val tree1 = EmptyTree.insert(5)
    require(tree1 == Node(5, EmptyTree, EmptyTree))

    val tree2 = tree1.insert(3)
    require(tree2 ==
        Node(5,
            Node(3, EmptyTree, EmptyTree),
            EmptyTree)
    )

    val tree3 = tree2.insert(10)
    require(tree3 ==
        Node(5,
            Node(3, EmptyTree, EmptyTree),
            Node(10, EmptyTree, EmptyTree)
        )
    )

    val tree4 = tree3.insert(20)
    require(tree4 ==
        Node(5,
            Node(3, EmptyTree, EmptyTree),
            Node(10,
                EmptyTree,
                Node(20, EmptyTree, EmptyTree)
            )
        )
    )

    val tree5 = tree4.insert(4)
    require(tree5 ==
        Node(5,
            Node(3,
                EmptyTree,
                Node(4, EmptyTree, EmptyTree)),
            Node(10,
                EmptyTree,
                Node(20, EmptyTree, EmptyTree)
            )
        )
    )

    val tree6 = tree5.insert(2)
    require(tree6 ==
        Node(5,
            Node(3,
                Node(2, EmptyTree, EmptyTree),
                Node(4, EmptyTree, EmptyTree)
            ),
            Node(10,
                EmptyTree,
                Node(20, EmptyTree, EmptyTree)
            )
        )
    )

    val tree7 = tree6.insert(8)
    require(tree7 ==
        Node(5,
            Node(3,
                Node(2, EmptyTree, EmptyTree),
                Node(4, EmptyTree, EmptyTree)
            ),
            Node(10,
                Node(8, EmptyTree, EmptyTree),
                Node(20, EmptyTree, EmptyTree)
            )
        )
    )
}
```

## 6-3

```kotlin
/**
 *
 * 연습문제 6-3
 *
 * 연습문제 6-2 에서 작성한 insert 코드를 100000만번 이상 연속해서 insert 해 보자.
 *
 * 힌트 : 테스트하는 디바이스마다 오류가 발생하는 시기는 다르겠지만 StackOverflowError 가 날 때까지 해보자.
 *
 */
 
fun main() {
    (1..100000).fold(EmptyTree as Tree<Int>) { acc, i -> acc.insert(i) }
}
```

## 6-4

- 우선 답을 베낌. 나중에 다시 풀어볼 것!

```kotlin
/**
 *
 * 연습문제 6-4
 *
 * SOF(Stack Overflow)가 일어나지 않도록 insertTailrec을 작성해보자.
 *
 * 힌트 : 함수의 선언 타입은 아래와 같다.
 *       필요하다면 내부 함수를 별도로 생성하자.
 *
 */

fun Tree<Int>.insertTailrec(elem: Int): Tree<Int> = rebuild(path(this, elem), elem)

private fun path(tree: Tree<Int>, value: Int): FunStream<Pair<Tree<Int>, Boolean>> {

    tailrec fun loop(tree: Tree<Int>,
        path: FunStream<Pair<Tree<Int>, Boolean>>): FunStream<Pair<Tree<Int>, Boolean>> =
        when (tree) {
            EmptyTree -> path
            is Node -> when {
                value < tree.value -> loop(tree.leftTree, path.addHead(tree to false))
                else -> loop(tree.rightTree, path.addHead(tree to true))
            }
        }

    return loop(tree, funStreamOf())
}

private fun rebuild(path: FunStream<Pair<Tree<Int>, Boolean>>, value: Int): Tree<Int> {

    tailrec fun loop(path: FunStream<Pair<Tree<Int>, Boolean>>, subTree: Tree<Int>): Tree<Int> =
        when (path) {
            FunStream.Nil -> subTree
            is FunStream.Cons -> when ((path.head()).second) {
                false -> loop(path.tail(),
                    Node((path.head().first as Node).value, subTree,
                        (path.head().first as Node).rightTree))
                true -> loop(path.tail(), Node((path.head().first as Node).value,
                    (path.head().first as Node).leftTree, subTree))

            }
        }
    return loop(path, Node(value, EmptyTree, EmptyTree))
}

fun main() {
    val tree1 = EmptyTree.insertTailrec(5)
    require(tree1 == Node(5, EmptyTree, EmptyTree))

    val tree2 = tree1.insertTailrec(3)
    require(tree2 ==
        Node(5,
            Node(3, EmptyTree, EmptyTree),
            EmptyTree)
    )

    val tree3 = tree2.insertTailrec(10)
    require(tree3 ==
        Node(5,
            Node(3, EmptyTree, EmptyTree),
            Node(10, EmptyTree, EmptyTree)
        )
    )

    val tree4 = tree3.insertTailrec(20)
    require(tree4 ==
        Node(5,
            Node(3, EmptyTree, EmptyTree),
            Node(10,
                EmptyTree,
                Node(20, EmptyTree, EmptyTree)
            )
        )
    )

    val tree5 = tree4.insertTailrec(4)
    require(tree5 ==
        Node(5,
            Node(3,
                EmptyTree,
                Node(4, EmptyTree, EmptyTree)),
            Node(10,
                EmptyTree,
                Node(20, EmptyTree, EmptyTree)
            )
        )
    )

    val tree6 = tree5.insertTailrec(2)
    require(tree6 ==
        Node(5,
            Node(3,
                Node(2, EmptyTree, EmptyTree),
                Node(4, EmptyTree, EmptyTree)
            ),
            Node(10,
                EmptyTree,
                Node(20, EmptyTree, EmptyTree)
            )
        )
    )

    val tree7 = tree6.insertTailrec(8)
    require(tree7 ==
        Node(5,
            Node(3,
                Node(2, EmptyTree, EmptyTree),
                Node(4, EmptyTree, EmptyTree)
            ),
            Node(10,
                Node(8, EmptyTree, EmptyTree),
                Node(20, EmptyTree, EmptyTree)
            )
        )
    )

    (1..100000).fold(EmptyTree as Tree<Int>) { acc, i ->
        acc.insertTailrec(i)
    }

}
```

## 6-5

```kotlin
/**
 *
 * 연습문제 6-5
 *
 * 6-1에서 만든 이진 트리에 어떤 노드가 존재하는지 확인하는 contains 함수를 추가해보자.
 *
 * 주의사항 : 문제의 복잡도를 낮추기 위해 입력 타입을 Int로 제한한다.
 *
 * 힌트: 함수의 선언 타입은 아래와 같다.
 *
 */
 
fun Tree<Int>.contains(elem: Int): Boolean = when(this) {
    EmptyTree -> false
    is Node -> when {
        elem < value -> leftTree.contains(elem)
        elem > value -> rightTree.contains(elem)
        else -> true
    }
}

fun main() {
    require(!EmptyTree.contains(5))

    val tree1 = EmptyTree.insert(5)
    require(tree1.contains(5))
    require(!tree1.contains(10))

    val tree2 = tree1.insert(3)
    require(tree2.contains(5))
    require(tree2.contains(3))
    require(!tree2.contains(10))

    val tree3 = tree2.insert(10)
    require(tree3.contains(5))
    require(tree3.contains(3))
    require(tree3.contains(10))
}
```

# Reference
- [대수적 자료형](https://ko.wikipedia.org/wiki/%EB%8C%80%EC%88%98%EC%A0%81_%EC%9E%90%EB%A3%8C%ED%98%95)