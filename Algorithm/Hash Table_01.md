> '파이썬 알고리즘 인터뷰(박상길, 책만)' 참조
# 해시 테이블 
## 1\. **(문제)** 중복 문자 없는 가장 긴 부분 문자열(리트코드 3)

```
- 문제: 중복 문자가 없는 가장 긴 부분 문자열(Substring)의 길이를 리턴하라

  - Input: s = "abcabcbb"
  - Output: 3
  - Explanation: The answer is "abc", with the length of 3.
```

문자열 내에서 중복이 없고, 길이가 가장 긴 부분 문자열을 출력하는 문제다.

s = ‘abcabcbb’ 일 때,

중복이 없고 가장 긴 문자열은 ‘abc’이므로,

부분문자열의 최대 길이는 3이다.

## 2\. **(풀이)** 슬라이딩 윈도우와 투 포인터로 윈도우 크기 조정

<img width="697" alt="image" src="https://user-images.githubusercontent.com/96895686/177914657-25138d4a-177d-4619-9ca1-4420992fb010.png">


우선, 문제의 풀이 과정을 그림으로 표현하겠다.

우선 투 포인터 left와 right를 모두 인덱스 0에 지정한다.

for 반복문으로 right 포인터를 한 칸씩 이동 시키면서, 부분 문자열의 중복 여부를 판단하고,

부분 문자열의 최대 길이를 업데이트한다.

위에 빨간선은 left와 right 포인터가 가리키는 범위(윈도우)이며,

윈도우 크기는 최대 3으로 ‘abc’, ‘bca’, ‘cab’ ‘abc’ 순서로 위치는 바뀌지만 크기는 모두 동일하다.

### **1) (해시 테이블)** 생성 및 ‘key:value’ 삽입

우선, for 반복문으로 right 포인터를 오른쪽으로 한 칸씩 이동하면서 해시 테이블(used)에 포인터가 가리키는 값(char, 해시 맵 key)과 위치(right, 해시 맵 value)를 담는다.

```
# 해시 테이블 생성
used = {}

for right, char in enumerate(s):


        '''       

        # 해시테이블 상 현재 문자(key)에 인덱스(value) 삽입
        used[char] = right
```

### **2) (for 반복)** 부분 문자열의 중복 여부 판단 and 최대 길이 업데이트

```
for right, char in enumerate(s):
            # 해시 테이블에 존재하는 문자의 경우 'left' 위치 업데이트
            # 'left' 위치가 업데이트 되는 경우 max_len 갱신 불필요
            if char in used and left <= used[char]:
                left = used[char] + 1

            else:
                # 문자의 최대 길이 업데이트
                max_len = max(max_len, right - left + 1)


            # 현재 문자에 인덱스 삽입
            used[char] = right
```

**2)-1** for 반복문에서 right 포인터가 가리키는 문자열이 해시 테이블(used)에 포함 되어 있는지 여부를 판단한다.

만약 이미 해시 테이블에 포함된 문자열이라면 해당 윈도우 내에서 중복 문자열이 발생하기 때문이다.

따라서, left포인터를 ‘해시 테이블에 든 문자열의 위치 + 1 ‘로 이동시킨다면 윈도우 내에서 중복 문자가 제외된다.

**2)-2** 반면, for 반복문에서 right 포인터가 가리키는 문자열이 해시 테이블에 포함되어 있지 않다면,

해당 부분 문자열은 중복이 없으므로 길이를 구한다. 이전에 구한 최대 길이(max\_len) 보다 더 크다면 새로 구한 길이로 ‘max\_len’을 업데이트한다. for 반복 연산을 마친 후, ‘max\_len’을 리턴하여 최대 길이를 출력하면 된다.

### 3. 전체 코드

```
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """

        # 해시 테이블 생성
        used = {}

        left = 0
        max_len = 0

        for right, char in enumerate(s):
            # 해시 테이블에 존재하는 문자의 경우 'left' 위치 업데이트
            # 'left' 위치가 업데이트 되는 경우 max_len 갱신 불필요
            if char in used and left <= used[char]:
                left = used[char] + 1

            else:
                # 문자의 최대 길이 업데이트
                max_len = max(max_len, right - left + 1)


            # 현재 문자에 인덱스 삽입
            used[char] = right

        return max_len
```
