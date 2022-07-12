# 38\. 일정 재구성

| '파이썬 알고리즘 인터뷰(박상길, 책만)' 참조

## 1\. 문제 - 일정 재구성(리트코드 332)

```
- 문제: [from, to]로 구성된 항공권 목록을 이용해 JFK에서 출발하는 여행 일정을 구성하라. 여러 일정이 있는 경우 사전 어휘 순(Lexical Order)로 방문한다.
  - Input: tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
  - Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]
```

![image](https://user-images.githubusercontent.com/96895686/178433927-2a049599-991a-4476-a615-5f085580fa3f.png)



티켓의 출발지와 도착지가 주어질 때, 가능한 여행 경로를 구하는 문제다.

만약 출발지에 도착지가 두개 이상이라면, 알파벳 순서가 빠른 곳으로 간다.

JFK에서 갈 수 있는 곳은 ‘SFO’와 ‘ATL’ 두 곳인데,

‘ATL’이 알파벳 순서가 빠르기 때문에 이곳을 먼저 방문한다.

따라서 JFK → ATL → JFK → SFO → ATL → SFO 순서대로 방문한다.

## 2\. 풀이 - 재귀함수를 활용한 DFS 탐색

### 1) 그래프 만들기

<img width="654" alt="image" src="https://user-images.githubusercontent.com/96895686/178434216-5ffdb769-d8d2-4e99-b079-cfa467fc7a53.png">


이동 가능한 경로를 그래프로 나타내야 한다

출발점을 key로 도착 가능한 지역들을 value로 입력한 딕셔너리 형태로 구성한다.

```python
# 그래프 사전식 순서대로 구성
        dict = collections.defaultdict(list)

        for a,b in sorted(tickets):
            dict[a].append(b)
```

코드를 살펴보면,

우선 ‘defaultdict’ 매서드로 빈 그래프(dict)를 만든다.

중요한 것은 도착 지점들이 알파벳 순으로 정렬돼야 한다는 점이다.

따라서 입력값(tickets)을 먼저 정렬하면 key의 문자열이 빠른 순으로 정렬되고, key가 같을 경우 value가 빠른 순으로 정렬된다.

그 후, 딕셔너리에 출발점을 key로, 도착지를 value로 입력한다면,

도착지는 알파벳 순으로 정렬되어 있을 것이다.

### 2) 재귀호출 및 dfs 탐색

![image](https://user-images.githubusercontent.com/96895686/178434298-21d4ed59-f46d-4760-b285-032891988e91.png)

‘JFK’에서 출발하여 도착 가능한 경로 중 알파벳이 빠른 곳에 먼저 방문한다.

앞서 도착지를 미리 정렬했기 때문에 도착지 리스트 중 맨 앞의 것을 추출(pop())하면 된다.

위 그림에서는 ‘ATL’이 첫 번째 도착지에 해당할 것이다.

추출한 값 ‘ATL’을 다시 입력값으로 넣어 재귀 함수를 호출하면,

이번에는 알파벳이 가장 앞선 도착지 ‘JFK’가 되고 해당 값을 추출하여 재귀 호출하는 과정을 반복한다.

마지막 지점까지 도착했다면, 마지막 지점부터 경로 리스트(route)에 차례대로 담는다.

```python
route = []

      # value의 첫 번째 값을 읽어 어휘순 방문
      def dfs(a):
          while dict[a]:
              dfs(dict[a].pop(0))

          route.append(a)

      dfs('JFK')
```

### 3) 결과 출력 및 전체 코드

위와 같이 마지막 지점부터 경로 리스트에 담기면, 결국 경로가 거꾸로 저장된다.

따라서 최종 결과 값을 뒤집어서 알파벳 순서대로 다시 정렬한다. 전체 코드는 아래와 같다.

```python
class Solution(object):
    def findItinerary(self, tickets):
        """
        :type tickets: List[List[str]]
        :rtype: List[str]
        """ 

        # 그래프 사전식 순서대로 구성
        dict = collections.defaultdict(list)

        for a,b in sorted(tickets):
            dict[a].append(b)

        route = []

        # value의 첫 번째 값을 읽어 어휘순 방문
        def dfs(a):
            while dict[a]:
                dfs(dict[a].pop(0))

            route.append(a)

        dfs('JFK')

        # 다시 뒤집어서 어휘순 결과대로 리턴
        return route[::-1]
```
