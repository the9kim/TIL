# 문제 31. 상위 K 빈도 요소

> ‘파이썬 알고리즘 인터뷰(박상길, 책만)’ 참조

## 1\. 문제 - 상위 K 빈도 요소(리트코드 347)

```
- 문제: 상위 k번 이상 등장하는 요소를 출력하라

  - Input: nums = [1,1,1,2,2,3], k = 2
  - Output: [1,2]
```

입력값으로 주어진 리스트에서 동일한 요소가 가장 많은 값 순서대로 k개를 출력하는 문제다.

예시로 주어진 리스트에서 엘리먼트 ‘1’, ‘2’, ‘3’은 각각 3개, 2개, 1개가 있다.

개수가 많은 순서대로 2개(k=2)를 출력하면 ‘1’과 ‘2’가 출력된다.

## 2\. 풀이 - 파이썬 모듈 활용

‘우선순위 큐’를 이용하여 엘리먼트의 빈도수 순서대로 정렬하는 방식이 일반적인 풀이일 것이다.

하지만 파이썬 모듈을 활용한 간결한 풀이 방법을 사용하려 한다.

우선 전체 코드는 아래와 같다.

짧은 코드이지만 다양한 기능을 함축하고 있다.

```
def topKFrequent(self, nums, k):

    return list(zip(*collections.Counter(nums).most_common(k)))[0]
```

### 1)  collections.Counter()

코드를 세분화 하여 살펴보자.

우선 collections.Counter()모듈은 엘리먼트(key)와 그 개수(value)를 튜플 형태로 반환한다.

<img width="508" alt="스크린샷 2022-07-09 오전 11 11 22" src="https://user-images.githubusercontent.com/96895686/178088092-9b508e75-441e-4100-874b-ad1e08b81fbf.png">

### 2) most\_common(k)

most\_common(k) 매서드르 활용하면 value값이 큰 순서대로 k개를 반환한다.

<img width="506" alt="스크린샷 2022-07-09 오전 11 11 47" src="https://user-images.githubusercontent.com/96895686/178088098-039c8a0b-5374-410e-8b4d-0562e08eb68b.png">

### 3) zip()

zip() 함수는 서로 다른 튜플 내 요소들을 인덱스에 맞게 짝지어 튜플로 반환한다.

```
>>> a = (1, 2)
>>> b = (3, 4)
>>> list(zip(a,b))

[(1,3), (2, 4)]
```

하지만 ‘zip(collections.Counter(nums).most\_common(k))’를 실행해보면 위의 예시와 다른 방식으로 출력된다. 엘리먼트끼리 짝지어지는 것이 아니라, 하나로 묶인 엘리먼트와 빈 값이 짝지어진다.

<img width="505" alt="image" src="https://user-images.githubusercontent.com/96895686/178125388-67dec1ac-4f41-40ab-83ba-7ac71a94ab78.png">


### 4) 아스테리스크(\*)

아스테리스크(\*)는 시퀀스를 언팩(Unpack)하는 연산자다. 위 예시에서 튜플로 묶인 엘리먼트를 언팩한 후 zip() 함수를 적용하면 우리가 원하는 형태의 값을 얻을 수 있다.

<img width="505" alt="image" src="https://user-images.githubusercontent.com/96895686/178125396-17e2806a-dd03-4817-821b-6c52ff38ca34.png">


### 5) 인덱싱

우리는 개수가 가장 많은 엘리먼트 k개(1과 2)를 리스트에 담아서 출력하고자 한다. 따라서 인덱싱으로 리스트의 첫 번째 요소를 출력하면 최종 답을 구할 수 있다.

<img width="505" alt="image" src="https://user-images.githubusercontent.com/96895686/178125415-0b0c76a1-0d73-4ae1-b7a5-e99bf6298c36.png">
