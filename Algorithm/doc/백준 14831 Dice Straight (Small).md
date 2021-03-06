# Dice Straight (Small)

| 시간 제한 | 메모리 제한 | 제출 | 정답 | 맞은 사람 | 정답 비율 |
| :-------- | :---------- | :--- | :--- | :-------- | :-------- |
| 5 초      | 512 MB      | 29   | 21   | 12        | 85.714%   |

## 문제

You have a special set of **N** six-sided dice, each of which has six different positive integers on its faces. Different dice may have different numberings.

You want to arrange some or all of the dice in a row such that the faces on top form a *straight* (that is, they show consecutive integers). For each die, you can choose which face is on top.

How long is the longest straight that can be formed in this way?

## 입력

The first line of the input gives the number of test cases, **T**. **T** test cases follow. Each test case begins with one line with **N**, the number of dice. Then, **N** more lines follow; each of them has six positive integers **Dij**. The j-th number on the i-th of these lines gives the number on the j-th face of the i-th die.

Limits

- 1 ≤ **T** ≤ 100.
- 1 ≤ **Dij** ≤ 106 for all i, j.
- 1 ≤ **N** ≤ 100.

## 출력

For each test case, output one line containing `Case #x: y`, where `x` is the test case number (starting from 1) and `y` is the length of the longest straight that can be formed.

## 예제 입력 1 복사

```
3
4
4 8 15 16 23 42
8 6 7 5 30 9
1 2 3 4 55 6
2 10 18 36 54 86
2
1 2 3 4 5 6
60 50 40 30 20 10
3
1 2 3 4 5 6
1 2 3 4 5 6
1 4 2 6 5 3
```

## 예제 출력 1 복사

```
Case #1: 4
Case #2: 1
Case #3: 3
```

## 힌트

In sample case #1, a straight of length 4 can be formed by taking the 2 from the fourth die, the 3 from the third die, the 4 from the first die, and the 5 from the second die.

In sample case #2, there is no way to form a straight larger than the trivial straight of length 1.

In sample case #3, you can take a 1 from one die, a 2 from another, and a 3 from the remaining unused die. Notice that this case demonstrates that there can be multiple dice with the same set of values on their faces.

## 출처

https://www.acmicpc.net/problem/14831



## 풀이

투 포인터 알고리즘을 사용해서 푼다

https://woovictory.github.io/2019/05/28/Two-Pointer-Algorithm/



### 전체 코드

```kotlin
import java.lang.Integer.min
import java.util.Scanner

fun main() = with(Scanner(System.`in`)) {
    val N = nextInt()
    val S = nextInt()
    val numbers = Array(N) { 0 }
    for (i in 0 until N) {
        numbers[i] = nextInt()
    }

    var result = Int.MAX_VALUE
    var sum = numbers[0]

    var low = 0
    var hight = 0
    while (hight in low until numbers.size) {
        when {
            sum < S -> {
                ++hight
                sum += if (hight < numbers.size) numbers[hight] else 0
            }
            sum == S -> {
                result = min(result, hight - low + 1)
                ++hight
                sum += if (hight < numbers.size) numbers[hight] else 0
            }
            sum > S -> {
                result = min(result, hight - low + 1)
                sum -= numbers[low++]
            }
        }
    }

    println(if (result == Int.MAX_VALUE) {
        0
    } else {
        result
    })
}
```



### 시간 복잡도

O(N)

### 공간복잡도

O(N)



## 오답 노트

투 포인터 알고리즘이 무었인지 몰랐다. 

그래서 처음 접근 방식은 무식하게 다 계산을 하는걸로 접근을 하였고,
그 후 특정 구간 에 대해서 앞에 있는 포인터를 이동하는 방식으로 최적화를 진행했다. 이 경우 시간 복잡도가 최악일 경우는 O(N^2)로 동일해서 시간 초과가 나왔다. 그후 접근 방식은 분할 정복법을 이용해서 길이가 1인 것의 합 길이가 2인 것들의 합, ... 으로 나누어서 계산을 시도 했다. 이경우 시간 복잡도가 O(N^2)로 동일하고 공간 복잡도가 조금 늘어나서 동일한 결과를 얻었다. 투 포인터 알고리즘에 대해서 추가적인 공부가 필요할 듯 하다.

