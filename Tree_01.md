# 47\. 이진트리 직렬화 & 역직렬화

> '파이썬 알고리즘 인터뷰(박상길, 책만)' 참조

## 1\. 문제 - 이진트리 직렬화 & 역직렬화(리트코드 297)

![image](https://user-images.githubusercontent.com/96895686/178177946-2cbc64fb-482e-4bd8-a768-dfb955ddbd3b.png)


```
- 문제: 이진 트리를 배열로 직렬화하고, 반대로 역직렬화하는 기능을 구현하라.
    - Input: root = [1,2,3,null,null,4,5]
    - Output: [1,2,3,null,null,4,5]
```

직렬화(Serialize)는 논리적 구조인 이진트리를 파일이나 메모리에 저장하기 위해 배열구조로 바꾸는 것을 말하며 반대는 과정은 역직렬화(Deserialize)라고 한다.

본 문제는 이진트리를 배열로 바꾸는 직렬화 함수와 배열을 이진트리로 바꾸는 역직렬화 함수 모두를 구현하는 것이다.

## 2\. 풀이

<img width="457" alt="스크린샷 2022-07-11 오전 11 28 25" src="https://user-images.githubusercontent.com/96895686/178178026-cefff283-41ad-4e85-b483-822d09f2f912.png">


### 1) 직렬화

**1-1**

우선, 이진 트리를 배열화 한 모습을 도식화하면 위와 같다.

노드 레벨 구분의 편의를 위해 인덱스를 1부터 시작했다. 인덱스 1, 2, 4, 8, … 순서로 앞 수에 곱하기 2를 하면 노드 레벨의 시작 위치를 구분할 수 있기 때문이다.

이진트리를 배열로 바꾸기 위해 너비 우선 탐색 방식(BFS)을 사용한다.

우선, 리스트 보다 시간 효율적인 데크 자료형으로 ‘큐'를 생성한다.

그리고 배열을 담을 리스트(result)를 만들어 주는데, 앞 서 말했듯이 인덱스가 1부터 시작하기 때문에 ‘#’ 하나를 미리 채워놓았다.

파이썬의 경우 None값을 string 형태로 출력할 수 없기 때문에 ‘#’를 이용하여 None을 표현했다.

```

def serialize(self, root):
    queue = collections.deque([root])
    result = ['#']
```


**1-2**

while 반복문을 통해 큐(queue)에서 노드를 하나씩 추출한 다음, 해당 노드의 자식 노드를 다시 큐에 삽입하는 과정을 반복한다.

추출된 부모 노드의 값을 결과를 담을 리스트(result)에 추가하되, 빈 노드의 경우 ‘#’를 추가한다.

최종 값을 하나의 문자열로 출력해야 하기 때문에 리스트(result)의 요소들을 join() 함수를 이용하여 통합한다.

```

def serialize(self, root):
    queue = collections.deque([root])
    result = ['#']

    # 트리 BFS 직렬화
    while queue:
        node = queue.popleft()

        if node:
            queue.append(node.left)
            queue.append(node.right)

            result.append(str(node.val))

        else:
            result.append('#')

    return ' '.join(result)
```

### 2) 역직렬화


**2 - 1**

직렬화 과정에서 join() 함수로 통합했던 배열을 split() 함수로 분할한 후 ‘nodes’ 변수에 담아준다.

루트 노드(root)를 생성하고, 해당 노드를 큐에 삽입한다.

인덱스 변수에 정수 2를 할당하는데, 루트 노드의 왼쪽 자식 노드가 인덱스 2에 위치하기 때문이다.

```
# 역직렬화
    def deserialize(self, data):

        # 예외 처리 
        if data == '# #':
            return None

        nodes = data.split()
        root = TreeNode(nodes[1])
        queue = collections.deque([root])

        index = 2
```


**2 - 2**

while 반복문을 통해 부모 노드를 큐에서 출력한 후, 자식 노드를 큐에 다시 삽입하는 과정을 반복한다.

이때 인덱스를 ‘+1’씩 증가시키며 부모 노드와 자식 노드를 연결한다.

자식 노드가 존재하지 않을 경우, queue에 추가하지 않음으로써 빈 노드를 처리한다.

```
while queue:

        node = queue.popleft()

    # 자식 노드 존재 시, 큐에 삽입
    if nodes[index] != '#':
        node.left = TreeNode(int(nodes[index]))
        queue.append(node.left)
    index += 1

    if nodes[index] != '#':
        node.right = TreeNode(int(nodes[index]))
        queue.append(node.right)
    index += 1
```

### 3) 전체 코드

```
class Codec:
    # 직렬화
    def serialize(self, root):

        queue = collections.deque([root])
        result = ['#']

        # 트리 BFS 직렬화
        while queue:
            node = queue.popleft()

            if node:
                queue.append(node.left)
                queue.append(node.right)

                result.append(str(node.val))

            else:
                result.append('#')

        return ' '.join(result)



    # 역직렬화
    def deserialize(self, data):

        # 예외 처리 
        if data == '# #':
            return None

        nodes = data.split()
        root = TreeNode(nodes[1])
        queue = collections.deque([root])

        index = 2

        while queue:

            node = queue.popleft()

            # 자식 노드 존재 시, 큐에 삽입
            if nodes[index] != '#':
                node.left = TreeNode(int(nodes[index]))
                queue.append(node.left)
            index += 1

            if nodes[index] != '#':
                node.right = TreeNode(int(nodes[index]))
                queue.append(node.right)
            index += 1

        return root
```
