# 원형 큐 디자인

## 1. 문제 - 원형 큐 디자인(리트코드 622)

**문제 정의**

원형 큐(Circular Queue)를 구현하는 문제다. 

세부적으로 구해야하는 함수는 아래와 같다

- 특정 요소(value)를 삽입하는 **enQueue(value)**
- 맨 앞의 요소를 제거하는 **deQueue()**
- 맨 앞의 요소를 출력하는 **Front()**
- 맨 마지막 요소를 출력하는 **Rear()**
- 원형 큐가 비어 있는지 여부를 판단하는 **isEmpty()**
- 원형 큐가 가득 차 있는지 여부를 판단하는 **isFull()**

```markdown
- Input
    ["MyCircularQueue", "enQueue", "enQueue", "enQueue", "enQueue", "Rear", "isFull", "deQueue", "enQueue", "Rear"]
    [[3], [1], [2], [3], [4], [], [], [], [4], []]
- Output
    [null, true, true, true, false, 3, true, true, true, 4]

- Explanation
    MyCircularQueue myCircularQueue = new MyCircularQueue(3);
    myCircularQueue.enQueue(1); // return True
    myCircularQueue.enQueue(2); // return True
    myCircularQueue.enQueue(3); // return True
    myCircularQueue.enQueue(4); // return False
    myCircularQueue.Rear();     // return 3
    myCircularQueue.isFull();   // return True
    myCircularQueue.deQueue();  // return True
    myCircularQueue.enQueue(4); // return True
    myCircularQueue.Rear();     // return 4
```

**원형 큐 개념**

일반적인 큐는 공간이 가득 차면 요소를 더이상 추가할 수 없다. 

deQueue()로 앞부분의 값이 빠져 공간이 비어있어도 추가하는 방법이 없다.

반면, 원형 큐는 공간이 가득차도, 앞 부분이 비어있으면 요소를 추가할 수 있다.

<img width="677" alt="image" src="https://user-images.githubusercontent.com/96895686/177711720-4c641b7c-6050-45ee-8690-921566cda2cf.png">


원형 큐의 구현 방식은 기본적으로 투포인터와 유사하다.

큐에 요소가 삽입될 때마다 ‘rear’포인트가 움직이고, 요소가 제거되면 ‘front’포인터가 움직인다.

삽입 또는 제거 명령에 따라 포인터가 움직이다가, 두 포인터가 한 지점에서 만나면 큐에 모든 공간이 가득 차게 된다.

위 그림 두 번째 줄에서 세 번째 큐를 보면 rear 포인터가 돌면서 원형 큐에 요소를 가득 채웠음을 알 수 있다.

하지만 다음 그림에서 보듯이 큐 앞부분에 빈 공간을 이용하여 새로운 요소를 삽입하게 된다.

## 2. 코드

- 초기화 함수

```python
def __init__(self, k):
        """
        :type k: int
        """
        self.q = [None] * k
        self.maxlen = k 
        self.p1 = 0
        self.p2 = 0
```

k개의 공간을 가지는 원형 큐의 초기화 함수를 정의한다.

우선 원형 큐에 k개의 None값으로 채워주며, 당연히 최대 길이는 k이다.

앞서 말한 ‘front 포인터’는 p1, ‘rear 포인터'는 p2로 정의하며 초기값을 0으로 할당한다.

- 삽입

```python
def enQueue(self, value):
        """
        :type value: int
        :rtype: bool
        """
        if self.q[self.p2] is None:
            self.q[self.p2] = value
            self.p2 = (self.p2 + 1) % self.maxlen
            return True
            
        else: 
            return False
```

매개변수로 삽입할 값인 value를 설정한다.

rear 포인터(p2)가 가리키는 삽입 위치가 비어있다면 해당 위치에 value값을 지정한다.

포인터를 +1만큼 이동시켜야 하는데, 주의할 점은 최대 길이로 나눈 나머지로 계산하는 것이다.

원형큐 구조이니 만큼, ‘인덱스 5’에서 +1이 될 경우, ‘6’이 아닌 ‘0’이 되어야 하기 때문이다.

- 제거

```python
def deQueue(self):
        """
        :rtype: bool
        """
        if self.q[self.p1] is None:
            return False
        
        else: 
            self.q[self.p1] = None
            self.p1 = (self.p1 + 1) % self.maxlen
            return True
```

front 포인터(p1)가 가리키는 제거 위치가 비어있지 않다면, 해당 값을 지워준다.

삽입 함수와 동일하게 포인터를 +1 만큼 추가하되 모듈로 연산으로 원형큐에 적합한 인덱스로 변환해준다.



- 전체 코드(나머지 코드 포함)

```python
class MyCircularQueue(object):

    def __init__(self, k):
        """
        :type k: int
        """
        self.q = [None] * k
        self.maxlen = k 
        self.p1 = 0
        self.p2 = 0
        

    # EnQueue: rear 포인터(p2) 이동
    def enQueue(self, value):
        """
        :type value: int
        :rtype: bool
        """
        if self.q[self.p2] is None:
            self.q[self.p2] = value
            self.p2 = (self.p2 + 1) % self.maxlen
            return True
            
        else: 
            return False
        

    def deQueue(self):
        """
        :rtype: bool
        """
        if self.q[self.p1] is None:
            return False
        
        else: 
            self.q[self.p1] = None
            self.p1 = (self.p1 + 1) % self.maxlen
            return True
        

    def Front(self):
        """
        :rtype: int
        """
        return -1 if self.q[self.p1] is None else self.q[self.p1]

    def Rear(self):
        """
        :rtype: int
        """
        return -1 if self.q[self.p2-1] is None else self.q[self.p2-1]

    def isEmpty(self):
        """
        :rtype: bool
        """
        return self.p1 == self.p2 and self.q[self.p1] is None

    def isFull(self):
        """
        :rtype: bool
        """
        return self.p1 == self.p2 and self.q[self.p1] is not None

# Your MyCircularQueue object will be instantiated and called as such:
# obj = MyCircularQueue(k)
# param_1 = obj.enQueue(value)
# param_2 = obj.deQueue()
# param_3 = obj.Front()
# param_4 = obj.Rear()
# param_5 = obj.isEmpty()
# param_6 = obj.isFull()
```
