# 39\. 코스 스케줄

| '파이썬 알고리즘 인터뷰(박상길, 책만)' 참조

## 1\. 문제 - 코스 스케줄(리트코드 207)

```
- 문제: 0을 완료하기 위해서는 1을 끝내야 한다는 것을 [0,1], 쌍으로 표현하는 n개의 코스가 있다. 코스 개수 n과 이 쌍들을 입력으로 받았을 때 모든 코스가 완료 가능한지 판별하라
    - Input: numCourses = 2, prerequisites = [[1,0]]
    - Output: true
    - Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So it is possible.
```

선행 수강해야하는 과목(prerequisites)을 고려했을 때, 특정 개수(numCourse)의 수업을 모두 들을 수 있는지 여부를 판단하는 문제다.

위 예시에서 ‘numCourses = 2’이므로 2개의 수업을 들어야 된다.

‘prerequisites = \[\[1,0\]\]’ 조건에서 1번 수업을 듣기 위해서는 0번 수업을 먼저 수강해야 한다.

즉 위의 경우는 수업을 들을 수 있으므로 ‘참’이다.

하지만 만약 조건이 numCourses = 2, prerequisites = \[\[1,0\], \[0,1\]\]이라고 한다면,

1번 수업을 듣기 위해 0번 수업을 먼저 들어야 하고, 동시에 0번 수업을 듣기 위해 1번 수업을 사전에 들어야 하므로, ‘거짓'이 된다.

## 2\. 풀이 - DFS로 순환구조 판별

이 문제는 순환 구조 여부를 판단해야 한다.

a강의를 듣기 위해 b강의를 먼저 수강해야 하고,

b강의를 듣기 위해 a강의를 먼저 수강해야 한다면,

a와 b가 서로 연결되는 구조가 만들어지며,

순환구조로 구성될 경우 ‘False’를 리턴한다.

### 1) 그래프 생성

```python

graph = collections.defaultdict(list)

for a, b in prerequisites:
    graph[a].append(b)
```

우선 입력값으로 주어진 수강 순서(변수 prerequisites)를 그래프 형태로 바꿔준다.

defaultdict 자료형을 이용하며, 도식화하면 아래와 같다.

![image](https://user-images.githubusercontent.com/96895686/178638382-a823c660-9c7e-41e9-9f1e-88ad8efab342.png)

### 2) 순환구조 판단

<img width="700" alt="image" src="https://user-images.githubusercontent.com/96895686/178638405-6c840f3a-8867-438e-b858-c0487b5f352d.png">


이미 방문했던 경로는 ‘traced’ 변수에 저장하고,

재귀 호출을 진행하면서 해당 경로가 ‘traced 변수'에 존재할 경우(중복일 경우) False를 리턴하는 구조다.(#3)

우선 그래프에 키값(a)을 대상으로 for 반복문을 실행하고, dfs 함수를 호출한다.(#1)

키에 속한 경로들을 나타내는 value(b) 대상으로 for 반복문을 실행하고, 재귀 함수를 호출한다.(#2)

경로 탐색을 마친 후 해당 경로는 ‘traced’에서 삭제한다.(# 4)

만약 삭제를 하지 않으면 for 문이 실행되는 과정에서 부모가 아닌 형제 노드가 방문한 경로를 중복 경로로 판단할 수 있기 때문이다.

이 과정을 통해 순환 경로의 존재 여부를 판단한다.

```python

traced = set()  # 3-1

def dfs(i):
    if i in traced:  #3-2
        return False

    set.add(i)

    # 2 
    for b in graph[i]: 
        if not dfs(b):
            return False
    # 4
    set.remove(i)

    return True

# 1
for a in list(graph):
    if not dfs(a):
        return False
```

### 3) 전체 코드

```python
class Solution(object):
    def canFinish(self, numCourses, prerequisites):
        graph = collections.defaultdict(list)
        # 그래프 구성
        for x, y in prerequisites:
            graph[x].append(y)

        traced = set()

        def dfs(i):
            # 순환 구조이면 False
            if i in traced:
                return False

            traced.add(i)
            for y in graph[i]:
                if not dfs(y):
                    return False
            # 탐색 종료 후 순환 노드 삭제
            traced.remove(i)

            return True

        # 순환 구조 판별
        for x in list(graph):
            if not dfs(x):
                return False

        return True
```
