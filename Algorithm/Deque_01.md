> '파이썬 알고리즘 인터뷰(박상길, 책만) 참조'

# 문제 26. 원형 데크 디자인

## 1\. 데크의 개념

데크(Deque) 자료형을 구현하는 문제다.

데크는 더블 엔디드 큐(Doble-Ended Queue)의 줄임말로, 양쪽 끝에서 큐 연산을 진행할 수 있는, 추상 자료형(ADT)이다.

데크는 이중 연결 리스트(Double Linked List)로 구현할 수 있다.

데크는 ‘head’와 ‘tail’ 두개의 포인터를 가지고 있으며,

새로운 아이템이 추가되면 앞쪽, 또는 뒤쪽으로 연결시켜주면 된다.

![image](https://user-images.githubusercontent.com/96895686/177898928-a102bb76-e38e-40f4-95d8-84bad5964245.png)

## 2\. 문제 - 원형 데크 디자인(리트코드 641)

```
MyCircularDeque(int k) Initializes the deque with a maximum size of k.
    boolean insertFront() Adds an item at the front of Deque. Returns true if the operation is successful, or false otherwise.
    boolean insertLast() Adds an item at the rear of Deque. Returns true if the operation is successful, or false otherwise.
    boolean deleteFront() Deletes an item from the front of Deque. Returns true if the operation is successful, or false otherwise.
    boolean deleteLast() Deletes an item from the rear of Deque. Returns true if the operation is successful, or false otherwise.
    int getFront() Returns the front item from the Deque. Returns -1 if the deque is empty.
    int getRear() Returns the last item from Deque. Returns -1 if the deque is empty.
    boolean isEmpty() Returns true if the deque is empty, or false otherwise.
    boolean isFull() Returns true if the deque is full, or false otherwise.
```

위에서 제시한 데크의 함수를 구현하는 문제다

우선 데크의 맨 앞과 맨뒤에 요소를 추가하는 함수(insertFront() & insertLast())가 있다.

동시에 요소를 제거하는 함수(deleteFront() & deleteLast())도 구현한다.

요소의 추가 및 제거를 위해 별도의 함수(\_add() & \_del())를 구현할 예정이다.

데크의 맨 앞과 맨 뒤의 값을 조회하는 getFront(), getRear() 함수를 만들고,

마지막으로 데크가 비었는지 또는 가득 찼는지 여부를 판단하는 isEmpty(), isFull()함수를 구현한다.

### 1\. 초기화 함수

```
def __init__(self, k):
        """
        :type k: int
        """

        self.head, self.tail = ListNode(None), ListNode(None)
        self.k, self.len = k, 0
        self.head.right, self.tail.left = self.tail, self.head
```

초기화 함수의 입력값으로 데크의 길이를 나타내는 k를 설정한다.

먼저 앞서 말한 헤드와 테일 포인터를 빈 연결리스트(ListNode(None))로 생성한다.

또한 특정 시점의 데크 길이를 표현할 값(self.len)도 0으로 초기화 한다.

### 2\. 삽입 함수

```
def _add(self, node, new):
        n = node.right
        node.right = new
        new.left, new.right = node, n
        n.left = new

def insertFront(self, value):
        """
        :type value: int
        :rtype: bool
        """

        if self.len == self.k:
            return False

        else:
            self.len += 1
            self._add(self.head, ListNode(value))

            return True   

def insertLast(self, value):
        """
        :type value: int
        :rtype: bool
        """

        if self.len == self.k:
            return False

        else:
            self.len += 1
            self._add(self.tail.left, ListNode(value))
            return True
```

삽입 함수의 구성은 간단하다.

우선, 데크의 길이가 전체 길이와 같으면 더 이상 삽입할 공간이 없으므로 False를 리턴한다.

삽입할 공간이 있으면 데크의 길이를 1만큼 증가시키고, \_add() 함수를 호출하여 노드를 삽입한다.

앞 쪽 삽입(insertFront())의 경우 \_add() 함수의 입력값으로 self.head가 들어가고,

뒤 쪽 삽입(insertLast())의 경우 self.tail.left가 들어가는 차이가 있다.

\_add() 함수를 조금 더 살펴보자면,

입력값 노드와 본래 연결된 노드(함수에서 n으로 정의한다) 사이에 새로운 노드 new를 삽입하여 방식이다.

연산 과정은 아래 그림의 번호 순서대로 진행된다.

![image](https://user-images.githubusercontent.com/96895686/177898888-a8807ff2-ec4c-40e4-89a4-b1df952801a7.png)

### 3\. 제거 함수

```
def _del(self, node):
        n = node.right.right
        node.right = n
        n.left = node

def deleteFront(self):
        """
        :rtype: bool
        """

        if self.len == 0:
            return False

        else:
            self.len -= 1
            self._del(self.head)
            return True


    def deleteLast(self):
        """
        :rtype: bool
        """

        if self.len == 0:
            return False 

        else:
            self.len -= 1
            self._del(self.tail.left.left)
            return True
```

제거 함수의 구성도 삽입 함수와 크게 다르지 않다.

우선, 데크의 길이가 0이면 더 이상 제거할 값이 없으므로 False를 리턴한다.

제거할 값이 있다면 데크의 길이를 1만큼 줄이고 \_del() 함수를 호출한다.

del() 함수에서, 입력값인 node와 오른쪽에 이어진 노드(n으로 정의된다) 간의 연결을 끊어 내기 위해서,

node를 n 오른쪽의 노드와 연결한다.

그림으로 나타내면 아래와 같다.

<img width="750" alt="image" src="https://user-images.githubusercontent.com/96895686/177898854-b0bd0f65-ef4c-4161-a838-cfa80065cd19.png">


### 4\. 나머지 및 전체 코드

삽입과 제거 함수를 제외하면 나머지 함수는 이해하기에 무리가 없다.

전체 코드는 아래와 같다.

```
class MyCircularDeque(object):

    def __init__(self, k):
        """
        :type k: int
        """

        self.head, self.tail = ListNode(None), ListNode(None)
        self.k, self.len = k, 0
        self.head.right, self.tail.left = self.tail, self.head


    def _add(self, node, new):
        n = node.right
        node.right = new
        new.left, new.right = node, n
        n.left = new

    def _del(self, node):
        n = node.right.right
        node.right = n
        n.left = node

    def insertFront(self, value):
        """
        :type value: int
        :rtype: bool
        """

        if self.len == self.k:
            return False

        else:
            self.len += 1
            self._add(self.head, ListNode(value))

            return True


    def insertLast(self, value):
        """
        :type value: int
        :rtype: bool
        """

        if self.len == self.k:
            return False

        else:
            self.len += 1
            self._add(self.tail.left, ListNode(value))
            return True

    def deleteFront(self):
        """
        :rtype: bool
        """

        if self.len == 0:
            return False

        else:
            self.len -= 1
            self._del(self.head)
            return True


    def deleteLast(self):
        """
        :rtype: bool
        """

        if self.len == 0:
            return False 

        else:
            self.len -= 1
            self._del(self.tail.left.left)
            return True



    def getFront(self):
        """
        :rtype: int
        """

        return self.head.right.val if self.len else -1


    def getRear(self):
        """
        :rtype: int
        """

        return self.tail.left.val if self.len else -1


    def isEmpty(self):
        """
        :rtype: bool
        """

        return self.len == 0


    def isFull(self):
        """
        :rtype: bool
        """

        return self.len == self.k

# Your MyCircularDeque object will be instantiated and called as such:
# obj = MyCircularDeque(k)
# param_1 = obj.insertFront(value)
# param_2 = obj.insertLast(value)
# param_3 = obj.deleteFront()
# param_4 = obj.deleteLast()
# param_5 = obj.getFront()
# param_6 = obj.getRear()
# param_7 = obj.isEmpty()
# param_8 = obj.isFull()
```
