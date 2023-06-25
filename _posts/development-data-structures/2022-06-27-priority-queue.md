---
title: 우선순위 큐
author: June
date: 2022-06-27 14:26:00 +0900
categories: [개발 자료 구조]
tags: [개발 자료 구조, 자료 구조, 큐, 우선순위 큐]
toc: true
math: true
mermaid: true
comments: true
---
## 정의

- 컴퓨터 과학에서, 우선순위 큐(Priority queue)는 평범한 큐나 스택과 비슷한 축약 자료형이다. 그러나 각 원소들은 우선순위를 갖고 있다. 우선순위 큐에서, 높은 우선순위를 가진 원소는 낮은 우선순위를 가진 원소보다 먼저 처리된다. 만약 두 원소가 같은 우선순위를 가진다면 그들은 큐에서 그들의 순서에 의해 처리된다.[[우선순위 큐](https://ko.wikipedia.org/wiki/%EC%9A%B0%EC%84%A0%EC%88%9C%EC%9C%84_%ED%81%90)]

    > 스택: <b>후입 선출</b> 순  
    > 큐: <b>선입 선출</b> 순  
    > 우선순위 큐: <b>우선 순위</b> 순

## 구조

- 우선순위 큐는 힙이나 다양한 다른 방법을 이용해 구현될 수 있다.
  - 일반 적으로 힙으로 구현되며, 이진 트리 구조로 이루어진다.
  - 일반 적으로 입출력에 대한 시간 복잡도는 <b>O(NlogN)</b>이다.

## 예제

### 스택, 큐, 우선 순위 큐 처리 순서 비교

#### 스택, 큐, 우선 순위 큐 처리 순서 비교 - kotlin code

```kotlin
fun main() {
    val stack = java.util.Stack<Int>()
    val queue:java.util.Queue<Int> = java.util.LinkedList()
    val priorityQueue = java.util.PriorityQueue<Int>()

    print("Input: ")
    intArrayOf(6, 1, 9, 3, 0).forEach {
        stack.push(it)
        queue.offer(it)
        priorityQueue.offer(it)
        print("$it ")
    }
    println()
    println()
    print("Stack Output(후입 선출): ")
    while (stack.isNotEmpty()) print("${stack.pop()} ")
    println()
    print("Queue Output(선입 선출): ")
    while (queue.isNotEmpty()) print("${queue.poll()} ")
    println()
    print("Priority Queue Output(우선 순위): ")
    while (priorityQueue.isNotEmpty()) print("${priorityQueue.poll()} ")
    println()
}
```

#### 스택, 큐, 우선 순위 큐 처리 순서 비교 - 출력

```text
Input: 6 1 9 3 0 

Stack Output(후입 선출): 0 3 9 1 6 
Queue Output(선입 선출): 6 1 9 3 0 
Priority Queue Output(우선 순위): 0 1 3 6 9 
```

### Linked List와 연산 시간 비교

#### Linked List와 연산 시간 비교 - 시나리오

- 10000000개의 수가 연속적으로 입력된다.
- 1000000번 마다 입력된 전체 수 중 가장 작은 수(min)를 출력한다.

#### Linked List와 연산 시간 비교 - kotlin code

```kotlin
fun main() {
    val list = java.util.LinkedList<Int>()
    val priorityQueue = java.util.PriorityQueue<Int>()
    val secureRandom = java.security.SecureRandom()
    repeat(10000000) {
        val num = secureRandom.nextInt()

        if((it+1) % 1000000 != 0) {
            list.add(num)
            priorityQueue.offer(num)
        }
        else {
            println("element 수: ${it + 1}")
            print("[Linked List] ")
            val elapsedListAdd: Long = kotlin.system.measureNanoTime {
                list.add(num)
            }
            print("입력 연산 시간: $elapsedListAdd ns")

            val elapsedList: Long = kotlin.system.measureNanoTime {
                print(" / min: ${list.minOf { x -> x }} ")
            }
            print(" / min 연산 시간: $elapsedList ns")
            print(" / 전체 연산 시간: ${elapsedListAdd + elapsedList} ns")
            println()

            print("[Priority Queue] ")
            val elapsedQueueAdd: Long = kotlin.system.measureNanoTime {
                priorityQueue.offer(num)
            }
            print("입력 연산 시간: $elapsedQueueAdd ns")
            val elapsedQueue: Long = kotlin.system.measureNanoTime {
                print(" / min: ${priorityQueue.peek()} ")
            }
            print(" / min 연산 시간: $elapsedQueue ns")
            print(" / 전체 연산 시간: ${elapsedQueueAdd + elapsedQueue} ns")
            println()
            println()
        }
    }
}
```

#### Linked List와 연산 시간 비교 - 출력

- 입력 값은 랜덤 수이므로 돌릴때 마다 다르다.
- 걸린 시간은 시스템에 따라 다르다. 시간차만 보면 될 듯.

```text
element 수: 1000000
[Linked List] 입력 연산 시간: 2000 ns / min: -2147482659  / min 연산 시간: 102550500 ns / 전체 연산 시간: 102552500 ns
[Priority Queue] 입력 연산 시간: 3600 ns / min: -2147482659  / min 연산 시간: 72300 ns / 전체 연산 시간: 75900 ns

element 수: 2000000
[Linked List] 입력 연산 시간: 6000 ns / min: -2147482722  / min 연산 시간: 52289000 ns / 전체 연산 시간: 52295000 ns
[Priority Queue] 입력 연산 시간: 14800 ns / min: -2147482722  / min 연산 시간: 38900 ns / 전체 연산 시간: 53700 ns

element 수: 3000000
[Linked List] 입력 연산 시간: 200 ns / min: -2147483380  / min 연산 시간: 37254600 ns / 전체 연산 시간: 37254800 ns
[Priority Queue] 입력 연산 시간: 1600 ns / min: -2147483380  / min 연산 시간: 23300 ns / 전체 연산 시간: 24900 ns

element 수: 4000000
[Linked List] 입력 연산 시간: 600 ns / min: -2147483380  / min 연산 시간: 65972700 ns / 전체 연산 시간: 65973300 ns
[Priority Queue] 입력 연산 시간: 1800 ns / min: -2147483380  / min 연산 시간: 20600 ns / 전체 연산 시간: 22400 ns

element 수: 5000000
[Linked List] 입력 연산 시간: 300 ns / min: -2147483380  / min 연산 시간: 71249900 ns / 전체 연산 시간: 71250200 ns
[Priority Queue] 입력 연산 시간: 2200 ns / min: -2147483380  / min 연산 시간: 30400 ns / 전체 연산 시간: 32600 ns

element 수: 6000000
[Linked List] 입력 연산 시간: 100 ns / min: -2147483380  / min 연산 시간: 323506600 ns / 전체 연산 시간: 323506700 ns
[Priority Queue] 입력 연산 시간: 2700 ns / min: -2147483380  / min 연산 시간: 21900 ns / 전체 연산 시간: 24600 ns

element 수: 7000000
[Linked List] 입력 연산 시간: 100 ns / min: -2147483380  / min 연산 시간: 331125900 ns / 전체 연산 시간: 331126000 ns
[Priority Queue] 입력 연산 시간: 2400 ns / min: -2147483380  / min 연산 시간: 19900 ns / 전체 연산 시간: 22300 ns

element 수: 8000000
[Linked List] 입력 연산 시간: 200 ns / min: -2147483380  / min 연산 시간: 300763100 ns / 전체 연산 시간: 300763300 ns
[Priority Queue] 입력 연산 시간: 3100 ns / min: -2147483380  / min 연산 시간: 29100 ns / 전체 연산 시간: 32200 ns

element 수: 9000000
[Linked List] 입력 연산 시간: 300 ns / min: -2147483380  / min 연산 시간: 387778000 ns / 전체 연산 시간: 387778300 ns
[Priority Queue] 입력 연산 시간: 1000 ns / min: -2147483380  / min 연산 시간: 46900 ns / 전체 연산 시간: 47900 ns

element 수: 10000000
[Linked List] 입력 연산 시간: 200 ns / min: -2147483380  / min 연산 시간: 311248100 ns / 전체 연산 시간: 311248300 ns
[Priority Queue] 입력 연산 시간: 2300 ns / min: -2147483380  / min 연산 시간: 16500 ns / 전체 연산 시간: 18800 ns
```

#### Linked List와 연산 시간 비교 - 결과

- 입출력에 대한 연산시간은 Priority Queue가 더 길다.
  - 하지만, element 수가 10,000,000 개 일때도, 2,300 ns(2 ms, 0.002 초)로 무시 가능한 수준이다.
  - O(NlogN)의 연산 시간이 이렇게 적게 걸리는 이유는 이전 입력된 값들이 이미 정렬이 되어 있기 때문이다.
- min 연산시간은 Linked List가 더 길다.
  - element 수가 많이 잘수록 그 갭은 커지며, element 수가 10,000,000 개 일때는 311,248,100 ns vs 16,500 ns로 Linked List가 약 18,864 배 더 오래 걸린다.
  - 우선순위 큐는 입출력 시점에 우선 순위 연산이 완료된 상태이므로 연산이라고 할 것도 없긴하다.
- element 수가 많이 잘수록 전체 연산 갭은 커진다.
  - element 수가 10,000,000 개 일때는 311,248,300 ns vs 18,800 ns로 Linked List가 약 16,556 배 더 오래 걸린다.

## 결론

- 우선순위 큐(Priority queue)는 입출력에 O(NlogN)의 시간 복잡도 우선 순위 연산을 한다.
- 연속적인 자료들에 대한 우선순위 연산이 필요할 경우 우선순위 큐는 많은 연산 시간의 이득을 가져온다.

## Reference

- [우선순위 큐](https://ko.wikipedia.org/wiki/%EC%9A%B0%EC%84%A0%EC%88%9C%EC%9C%84_%ED%81%90)
