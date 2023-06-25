---
title: 코틀린으로 배우는 함수형 프로그래밍 - 2장 코틀린으로 함수형 프로그래밍 시작하기
author: June
date: 2022-07-13 18:21:00 +0900
categories: [개발 책, 코틀린으로 배우는 함수형 프로그래밍]
tags: [공부, 책, 코틀린으로 배우는 함수형 프로그래밍, 코틀린, kotlin, 함수형 프로그래밍, funtional programming]
toc: true
math: true
mermaid: true
comments: true
---
# 범위
- 2장 코틀린으로 함수형 프로그래밍 시작하기

# 요약
- 함수형 프로그래밍의 특징은 다음과 같다.
    - 불변성(immutable)
    - 참조 투명성(referential transparency)
    - 일급 함수(first-class function)
    - 게으른 평가(lazy evaluation)

# 개념 정리
- 익명 함수(anonymous function): 함수 이름을 선언하지 않고, 구현부만 작성하는 함수를 표현하는 방식의 일종.
- 확장 함수(extension function): 이미 작성된 클래스에 상속을 하거나 내부를 수정하지 않고 새롭게 추가한 함수.
- 패턴 매칭(pattern matching): 값, 조건, 타입 등의 패턴에 따라서 매칭되는 동작을 수행하게 하는 기능.
- 객체 분해: 객체를 구성하는 프로퍼티를 분해하여 편리하게 변수에 할당하는 것.
- 제네릭(generic): rorcp 내부에서 사용할 데이터 타입을 외부에서 정하는 기법.
- 변성(variance): 제네릭을 포함한 계층 관계에서 타입의 가변성을 처리하는 방식.
    - 무공변(invariant): 타입 S가 T의 하위 타입일 때, `Box<S>`가 `Box<T>`는 상속 관계가 없다.
    - 공변(covariant):  타입 S가 T의 하위 타입일 때, `Box<S>`는 `Box<T>`의 하위 타입이다.
    - 반공변(contravariant): 타입 S가 T의 하위 타입일 때, `Box<T>`는 `Box<S>`의 하위 타입이다.

# 책에서 기억하고 싶은 내용
- 2.1 프로퍼티 선언과 안전한 널 처리
    - val: 읽기 전용 프로퍼티를 선언하는 예약어
    - var: 선언 이후에 수정이 가능한 가변(mutable) 프로퍼티를 선언하는 예약어
    - 타입 뒤에 ?를 붙이면 해당 프로퍼티는 값으로 널을 할당할 수 있다.
- 2.2 함수와 람다
    - fun: 함수의 예약어
    - 반환 타입을 명시하지 않으면 Unit 타입을 반환한다.
- 2.3 제어 구문
    - 코틀린에서 if문은 기본적으로 표현식이다. 표현식은 구문과 달리 결과로서 어떤 값을 반환한다.
    - when문도 표현식이다. when문은 java의 swich문이나 스칼라의 패턴 매칭과 유사한 기능을 한다.
- 2.4 인터페이스
    - 코틀린에서 제공하는 인터페이스는 다음과 같은 특징이 있다.
        - 다중 상속이 가능하다.
        - 추상(abstract) 함수를 가질 수 있다.
        - 함수의 본문을 구현할 수 있다.
        - 여러 인터페이스에서 같은 이름의 함수를 가질 수 있다.
        - 추상 프로퍼티를 가질 수 있다.
-  인터페이스에서는 추상 프로퍼티의 값을 직접 초기화할 수 없고, getter를 구현해야 한다.

```kotlin
interface Foo {
    val bar1:Int = 3 // 컴파일 에러
    val bar2: Int get() = 3
}
```
- 2.5 클래스
    - data class
        - data class는 기본적으로 게터(getter), 세터(setter) 함수를 생성해주고, hashCode, equals, toString 함수와 같은 자바 Object 클래스에 정의된 함수들을 자동으로 생성한다.
        - data class로 선언된 객체는 추가로 copy와 componentN 함수를 제공한다. copy 함수는 객체의 값을 그대로 복사한 새로운 객체를 생성할 때 사용된다. componentN 함수는 객체가 가진 프로퍼티의 개수만큼 호출할 수 있는데, 프로퍼티 이름으로 접근하는 대신에 사용된다. 예를 들어 component1 함수는 객체의 첫 번째 프로퍼티의 값을 반환한다.
    - enum class
        - enum class는 특정 상수에 이름을 붙여주는 클래스다.
    - sealed class
        - sealed class는 enum class의 확장 형태로, 클래스를 묶은 클래스이다. 제약 없이 새로운 타입을 확장 할 수 있다.
- 2.6 패턴 매칭
    - when문에 값을 넣지 않으면 조건문에 따른 패턴을 정의할 수 있다.
- 2.7 객체 분해
- 2.8 컬렉션
    - 코틀린에서는 불변과 가변(muable) 자료구조를 분리해서 제공하고 있고, List, Set, Map 등의 자료 구조는 기본적으로 불변이다.
    - List와 Set은 불변 자료구조이기 때문에 add 함수가 없다. 대신 plus 함수가 제공된다. plus 함수는 원본 리스트를 변경하지 않고 새로운 리스트를 반환한다.
- 2.9 제네릭
    - 제네릭을 사용해 클래스를 일반화하면 재사용성이 높아진다. 마찬가지로 제네릭으로 함수의 타입을 일반화하면 재사용성이 높은 함수를 만들 수 있다.
- 2.10 코틀린 표준 라이브러리
    - 람다 리시버는 receiver의 타입 T를 block 함수의 입력인 T.()로 전달한다.
    - use는 클로즈 작업을 자동으로 해 주는 함수이다.
    - let, with, run, apply, also 함수
        - 사용 예제

```kotlin
fun main() {
    generalExample(Person("example", 30))
    letExample(Person("example", 30))
    withExample(Person("example", 30))
    runExample(Person("example", 30))
    applyExample(Person("example", 30))
    alsoExample(Person("example", 30))
}

data class Person(var name: String, val age: Int)

fun generalExample(input: Person) {
    input.name = "generalExample"
    val result = input
    println("일반적인 객체 변경")
    println("result: $result")
    println()
}


fun letExample(input: Person) {
    val result = input.let {
        it.name = "letExample"
        it
    }
    println("let")
    println("result: $result")
    println()
}

fun withExample(input: Person) {
    val result = with(input) {
        name = "withExample"
        this
    }
    println("with")
    println("result: $result")
    println()
}

fun runExample(input: Person) {
    val result = input.run {
        name = "runExample"
        this
    }
    println("run")
    println("result: $result")
    println()
}

fun applyExample(input: Person) {
    val result = input.apply {
        name = "applyExample"
    }
    println("apply")
    println("result: $result")
    println()
}

fun alsoExample(input: Person) {
    val result = input.also {
        it.name = "alsoExample"
    }
    println("also")
    println("result: $result")
    println()
}
```

```
일반적인 객체 변경
result: Person(name=generalExample, age=30)

let
result: Person(name=letExample, age=30)

with
result: Person(name=withExample, age=30)

run
result: Person(name=runExample, age=30)

apply
result: Person(name=applyExample, age=30)

also
result: Person(name=alsoExample, age=30)
```

    - 비교

<table data-layout="default">
    <colgroup>
        <col style="width: 85.0px;" />
        <col style="width: 134.2px;" />
        <col style="width: 134.2px;" />
        <col style="width: 134.2px;" />
        <col style="width: 134.2px;" />
        <col style="width: 134.2px;" />
    </colgroup>
    <tbody>
        <tr>
            <th>
            </th>
            <th>
                <p style="text-align: center;"><strong>let</strong></p>
            </th>
            <th>
                <p style="text-align: center;"><strong>with</strong></p>
            </th>
            <th>
                <p style="text-align: center;"><strong>run</strong></p>
            </th>
            <th>
                <p style="text-align: center;"><strong>apply</strong></p>
            </th>
            <th>
                <p style="text-align: center;"><strong>also</strong></p>
            </th>
        </tr>
        <tr>
            <th>
                <p style="text-align: center;"><strong>코드 블록</strong></p>
            </th>
            <td>
                <p style="text-align: center;">람다식</p>
            </td>
            <td>
                <p style="text-align: center;">람다 리시버</p>
            </td>
            <td>
                <p style="text-align: center;">람다 리시버</p>
            </td>
            <td>
                <p style="text-align: center;">람다 리시버</p>
            </td>
            <td>
                <p style="text-align: center;">람다식</p>
            </td>
        </tr>
        <tr>
            <th>
                <p style="text-align: center;"><strong>접근</strong></p>
            </th>
            <td>
                <p style="text-align: center;">it</p>
            </td>
            <td>
                <p style="text-align: center;">this</p>
            </td>
            <td>
                <p style="text-align: center;">this</p>
            </td>
            <td>
                <p style="text-align: center;">this</p>
            </td>
            <td>
                <p style="text-align: center;">it</p>
            </td>
        </tr>
        <tr>
            <th>
                <p style="text-align: center;"><strong>반환값</strong></p>
            </th>
            <td>
                <p style="text-align: center;">람다식 반환값</p>
            </td>
            <td>
                <p style="text-align: center;">람다식 반환값</p>
            </td>
            <td>
                <p style="text-align: center;">람다식 반환값</p>
            </td>
            <td>
                <p style="text-align: center;">자기 자신</p>
            </td>
            <td>
                <p style="text-align: center;">자기 자신</p>
            </td>
        </tr>
        <tr>
            <th>
                <p style="text-align: center;"><strong>확장 함수 여부</strong></p>
            </th>
            <td>
                <p style="text-align: center;">O</p>
            </td>
            <td>
                <p style="text-align: center;">X</p>
            </td>
            <td>
                <p style="text-align: center;">X</p>
            </td>
            <td>
                <p style="text-align: center;">O</p>
            </td>
            <td>
                <p style="text-align: center;">O</p>
            </td>
        </tr>
        <tr>
            <th>
                <p style="text-align: center;"><strong>선언</strong></p>
            </th>
<td markdown="block">

```kotlin
fun <T, R> T.let(block: (T) -> R): R
```

</td>
<td markdown="block">

```kotlin
fun <T, R> with(receiver: T, block: T.() -> R): R
```

</td>
<td markdown="block">

```kotlin
fun <R> run(block: () -> R): R
```

</td>
<td markdown="block">

```kotlin
fun <T> T.apply(block: T.() -> Unit): T
```

</td>
<td markdown="block">

```kotlin
fun <T> T.also(block: (T) -> Unit): T
```

</td>
        </tr>
    </tbody>
</table>

- 2.11 변성

<table data-layout="default">
    <colgroup>
        <col style="width: 65.0px;" />
        <col style="width: 231.67px;" />
        <col style="width: 231.67px;" />
        <col style="width: 231.67px;" />
    </colgroup>
    <tbody>
        <tr>
            <th>
            </th>
            <th>
                <p style="text-align: center;"><strong>무공변</strong></p>
            </th>
            <th>
                <p style="text-align: center;"><strong>공변</strong></p>
            </th>
            <th>
                <p style="text-align: center;"><strong>반공변</strong></p>
            </th>
        </tr>
        <tr>
            <th>
                <p style="text-align: center;"><strong>전제</strong></p>
                <p style="text-align: center;"><strong>조건</strong></p>
            </th>
            <td colspan="3">
                <p style="text-align: center;">타입 S가 T의 하위 타입(S --▷ T)</p>
                <p style="text-align: center;">ex) Kotlin --▷ Language</p>
            </td>
        </tr>
        <tr>
            <th>
                <p style="text-align: center;"><strong>상속</strong></p>
                <p style="text-align: center;"><strong>관계</strong></p>
            </th>
            <td>
                <p style="text-align: center;">상속 관계가 없다</p>
            </td>
            <td>
                <p style="text-align: center;">Box&lt;S&gt; --▷ Box&lt;T&gt;</p>
                <p style="text-align: center;">ex) Box&lt;Kotlin&gt; --▷ Box&lt;Language&gt;</p>
            </td>
            <td>
                <p style="text-align: center;">Box&lt;S&gt; ◁-- Box&lt;T&gt;</p>
                <p style="text-align: center;">ex) Box&lt;Kotlin&gt; ◁-- Box&lt;Language&gt;</p>
            </td>
        </tr>
        <tr>
            <th>
                <p style="text-align: center;"><strong>키워드</strong></p>
            </th>
            <td>
                <p style="text-align: center;">X</p>
            </td>
            <td>
                <p style="text-align: center;">out</p>
                <p style="text-align: center;">ex) Box&lt;out Language&gt;</p>
            </td>
            <td>
                <p style="text-align: center;">in</p>
                <p style="text-align: center;">ex) Box&lt;in Kotlin&gt;</p>
            </td>
        </tr>
        <tr>
            <th>
                <p style="text-align: center;"><strong>java</strong></p>
                <p style="text-align: center;"><strong>키워드</strong></p>
            </th>
            <td>
            </td>
            <td>
                <p style="text-align: center;">extends (upper bound)</p>
                <p style="text-align: center;">ex) Box&lt;T extends Language&gt;</p>
                <p style="text-align: center;">(Kotlin: Box&lt;T : Language&gt;)</p>
            </td>
            <td>
                <p style="text-align: center;">super (low bound)</p>
                <p style="text-align: center;">ex) Box&lt;T super Kotlin&gt;</p>
                <p style="text-align: center;">(Kotlin: x)</p>
            </td>
        </tr>
        <tr>
            <th>
                <p style="text-align: center;"><strong>속성</strong></p>
            </th>
            <td>
            </td>
            <td>
                <p style="text-align: center;">out 키워드를 사용한 공변에서는 Box 안의 값을 꺼내서 읽을 때(read)는 문제가 없지만, Box에 값을 넣으려고 할 때(write) 컴파일 오류가 발생한다.</p>
            </td>
            <td>
                <p style="text-align: center;">out 키워드를 사용한 반공변에서는 Box 안의 값을 넣을 때(write)는 문제가 없지만, Box에 값을 읽으려고 할 때(read) 컴파일 오류가 발생한다.</p>
            </td>
        </tr>
    </tbody>
</table>

# 연습 문제
## 2-1

```kotlin
fun String.helloThis() = "Hello, $this"

fun main() {
    require("kotlin".helloThis() == "Hello, kotlin")
    require("FP".helloThis() == "Hello, FP")
}
```