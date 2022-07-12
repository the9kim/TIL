# 1 36\. 조합의 합

> '파이썬 알고리즘 인터뷰(박상길, 책만)' 참조

## 1\. 문제 - 조합의 합(리트코드 39)

```
- 문제: 숫자 집합 candidates를 조합하여 합이 target이 되는 원소를 나열하라. 각 원소는 중복으로 나열 가능하다. 
  - Input: candidates = [2,3,6,7], target = 7
  - Output: [[2,2,3],[7]]
```

배열(candidates)의 요소를 더해서 특정 수(target)를 만드는 문제다.

중복해서 수를 뽑을 수 있다는 점이 중요한 조건이다.

위 예를 보면 2, 3, 6, 7 중에서 임의로 숫자를 뽑아 더한 다음 7을 만든다.

‘2+2+3 = 7 ‘과 ‘7 = 7’ 두 가지 경우의 수가 발생하므로,

정답은 \[ \[2, 2, 3\] , \[7\] \] 이 된다.

## 2\. 풀이 - DFS로 그래프 탐색

![image](https://user-images.githubusercontent.com/96895686/178389171-86ae2a62-a0a8-4b62-9a4a-a8b8e2c3e890.png)

### 1)
풀이 과정의 일부를 도식화 하면 위 그림과 같다.

숫자 2부터 경로를 찾아나간다.

다음 경로의 후보로 ‘자기 자신 + 뒤에 나열된 수’가 된다.

2의 경우 \[2, 3, 4, 5\]이고,

3의 경우 \[3, 4, 5\]가 될 것이다.

<img width="750" alt="image" src="https://user-images.githubusercontent.com/96895686/178389233-b958df6f-c20e-4e92-be57-5c1fb750c118.png">

### 2)

위 과정을 재귀함수로 구현한다.

csum은 구하고자 하는 값(target)에서 탐색한 경로의 값의 차이다.

예를 들어, 맨 처름 숫자 2를 선택했으면, 앞으로 ‘7-2 = 5’만큼을 더 찾아야 한다.

‘index’는 ‘자기자신 + 뒤에 나열된 수’의 순서를 나타낸다.

마지막으로 ‘path’는 지나온 경로를 담고 있으며, 정답을 충족할 경우 ‘result’ 변수에 담기게 된다.

### 3)

위의 예에서, 2를 총 네번 선택하는 경로를 선택(7-2-2-2-2 = -1)한다면,

값이 1만큼 초과하여 오답이 되어 해당 경로는 탐색이 종료된다.

이번에 2를 세 번 선택할 경우를 살펴보면,

‘7-2-2-2 =1 ‘로 값이 1만큼 부족하기 때문에 오답이 된다.

인덱스를 1만큼 증가시켜서 ‘2’ 세 번, ‘3’ 한 번을 탐색할 경우

‘7-2-2-3 = 0’으로 정답에 해당하며, ‘result’ 변수에 담긴다.

이처럼 경로의 값들을 더했을 때 원하는 값(target)을 초과할 경우, 탐색을 종료한다.

경로의 합이 원하는 값과 일치할 경우, 해당 경로를 정답 리스트에 추가한다.

## 3\. 전체 코드

```python
class Solution(object):
    def combinationSum(self, candidates, target):
        """
        :type candidates: List[int]
        :type target: int
        :rtype: List[List[int]]
        """

        result = []

        def dfs(csum, index, path):

            # 종료 조건
            if csum < 0:
                return

            elif csum == 0:
                result.append(path)
                return 

            # 자신부터 하위 원소까지의 나열 재귀 호출
            for i in range(index, len(candidates)):
                dfs(csum-candidates[i], i, path + [candidates[i]])


        dfs(target, 0, [])

        return result
```
