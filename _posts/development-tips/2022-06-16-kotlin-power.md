---
title: kotlin 제곱 사용법
author: June
date: 2022-06-16 12:47:00 +0900
categories: [개발 팁]
tags: [tip, kotlin, 코틀린, power, pow, 제곱]
toc: true
math: true
mermaid: true
comments: true
---
## 1. kotlin의 제곱 함수

- kotlin의 제곱 함수는 kotlin.math package에 float와 double 타입에 대한 확장 함수로 선언되어 있다.
- 확장 함수로 형태만 변경 되었을 뿐, 내부적으로는 자바의 java.lang package의 Math.pow 함수를 그대로 사용한다.

> - code: kotlin.math.pow
>
> ```kotlin
> /**
>  * Raises this value to the power [x].
>  *
>  * Special cases:
>  *   - `b.pow(0.0)` is `1.0`
>  *   - `b.pow(1.0) == b`
>  *   - `b.pow(NaN)` is `NaN`
>  *   - `NaN.pow(x)` is `NaN` for `x != 0.0`
>  *   - `b.pow(Inf)` is `NaN` for `abs(b) == 1.0`
>  *   - `b.pow(x)` is `NaN` for `b < 0` and `x` is finite and not an integer
>  */
> @SinceKotlin("1.2")
> @InlineOnly
> public actual inline fun Double.pow(x: Double): Double = nativeMath.pow(this, x)
> 
> /**
>  * Raises this value to the integer power [n].
>  *
>  * See the other overload of [pow] for details.
>  */
> @SinceKotlin("1.2")
> @InlineOnly
> public actual inline fun Double.pow(n: Int): Double = nativeMath.pow(this, n.toDouble())
> 
> /**
>  * Raises this value to the power [x].
>  *
>  * Special cases:
>  *   - `b.pow(0.0)` is `1.0`
>  *   - `b.pow(1.0) == b`
>  *   - `b.pow(NaN)` is `NaN`
>  *   - `NaN.pow(x)` is `NaN` for `x != 0.0`
>  *   - `b.pow(Inf)` is `NaN` for `abs(b) == 1.0`
>  *   - `b.pow(x)` is `NaN` for `b < 0` and `x` is finite and not an integer
>  */
> @SinceKotlin("1.2")
> @InlineOnly
> public actual inline fun Float.pow(x: Float): Float = nativeMath.pow(this.toDouble(), x.toDouble()).toFloat()
> 
> /**
>  * Raises this value to the integer power [n].
>  *
>  * See the other overload of [pow] for details.
>  */
> @SinceKotlin("1.2")
> @InlineOnly
> public actual inline fun Float.pow(n: Int): Float = nativeMath.pow(this.toDouble(), n.toDouble()).toFloat()
> ```

## 2. 자바와 차이

- 자바는 일반 함수의 형태지만, kotlin은 확장 함수의 형태이다.

- ex) 5의 제곱

<table border="1">
<tr>
<th width='50%'><center>JAVA</center></th>
<th width='50%'><center>Kotlin</center></th>
</tr>
<tr><!-- 첫번째 줄 시작 -->
<td markdown="block">

```java
import java.lang.Math;

double result = Math.pow(5.0, 2.0);
```

</td>
<td markdown="block">

```kotlin
import kotlin.math.pow

val result = 5.0.pow(2.0)
```

</td>
</tr>
</table>

## 3. 사용법

- kotlin의 제곱 함수는 double과 float에 대한 확장 함수로 선언되어 있다.
- Int에 대해 사용하려면 형 변환이 필요하다.
  - 상수라면 2.0 식으로 소숫점을 붙여주면 된다.
  - 변수라면 .toDouble()를 붙여준다.
- 지수의 경우 밑수의 타입에 맞춰 (가능한 경우)자동 형변환이 된다.
- 결과는 (위의 함수 정의와 같이)밑수의 타입과 같이 타입이 반환된다.
  - Int 값이 필요한 경우 형변환(.toInt())을 하여야 한다.
- 예시

```kotlin
import kotlin.math.pow

fun main() {
    val a = 2
    val b = 3

    println("=======================")
    println("a.toDouble().pow(b).toInt() = ${a.toDouble().pow(b).toInt()}")
    println("4.0.pow(5).toInt() = ${4.0.pow(5).toInt()}")
    println("6.0.pow(7) = ${6.0.pow(7)}")
}
```

```text
=======================
a.toDouble().pow(b).toInt() = 8
4.0.pow(5).toInt() = 1024
6.0.pow(7) = 279936.0
```
