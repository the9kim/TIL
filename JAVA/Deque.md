# Deque란?

Deque는 'Double-Ended-Queue'의 줄임말로,

앞과 뒤 양쪽에서 데이터를 삽입하고 제거할 수 있는 자료구조 입니다.

흔히 알고 있는 스택과 큐를 합친 형태입니다.

자바에서 Vector를 상속받은 Stack의 경우, 초기 용량 설정을 지원하지 않거나 스레드 안정성이 떨어지는 문제가 있기 때문에

Deque 인터페이스 사용이 권장되곤 합니다.

그럼 Deque의 기본적인 사용법에 관해 알아보겠습니다.

# Duque 선언

Deque 인터페이스를 구현한 ArrayDeque(), LinkedList(), 등을 활용하여 Deque를 선언할 수 있습니다.
아래는 ArrayDeque()를 이용한 예제입니다.
<img width="463" alt="스크린샷 2023-03-14 오전 8 26 31" src="https://user-images.githubusercontent.com/96895686/224854715-1f7638cb-9d06-4f5c-8c15-da5a8a07b1a9.png">

# 데이터 추가
데이터를 추가하는 메서드로 add()와 offer()를 사용할 수 있습니다. 
두 메서드의 대표적인 차이로, add()의 경우 용량이 초과할 경우 Exception이 발생하지만
offer()의 경우 false를 리턴한다는 점을 들 수 있습니다.
자료구조의 맨 앞에 데이터를 추가할 경우 addFirst()와 offerFirst()를 사용하고
맨 뒤에 데이터를 추가할 경우 addLast(), offerLast()를 이용할 수 있습니다.
add()와 offer()는 addLast()와 offerLast()와 동일한 기능을 수행합니다.

<img width="518" alt="image" src="https://user-images.githubusercontent.com/96895686/224855433-8d924558-ae8b-4d91-ba6d-a99461f8a824.png">


# 데이터 제거
데이터를 제거하는 메서드로 remove()와 poll()이 있습니다.
두 메서드 간 대표적 차이는 앞서 설명한 바와 동일하게
deque가 비어있을 경우(제거할 대상이 없으므로),
remove()의 경우 exception을 호출하지만
poll()은 false를 리턴한다는 점을 들 수 있습니다.
removeFirst()와 pollFirst() 맨 앞의 데이터를 제거하고,
removeLast(), pollLast()는 맨 뒤의 데이터를 삭제합니다.
remove()와 Poll()의 경우 removeFirst()와 pollFirst()와 각기 동일한 기능을 수행합니다.

<img width="666" alt="image" src="https://user-images.githubusercontent.com/96895686/224856077-cbc598e6-14ad-425b-bd2d-5bd4a8dba685.png">


# 데이터 조회
데이터를 조회하는 메서드로 peek()와 get()를 활용할 수 있습니다.
첫번 째 데이터를 조회하는 메서드로 peekFirst(), getFirst(), peek()이 있고,
마지막 데이터를 조회하는 메서드로 peekLast(), getLast()가 있습니다.
<img width="698" alt="image" src="https://user-images.githubusercontent.com/96895686/224856337-76114af9-5817-4cf0-b294-fa5848718abb.png">


> 참고 자료
- [Tecoble 블로그 / 3기_와일더 / Java 의 Stack 대신 Deque](https://tecoble.techcourse.co.kr/post/2021-05-10-stack-vs-deque/)
- [침착곰 블로그 / [JAVA] Deque/ArrayDeque(데크) 개념 및 사용법 정리](https://crazykim2.tistory.com/581)
