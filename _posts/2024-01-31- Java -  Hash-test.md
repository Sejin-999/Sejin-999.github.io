---
title: 자바에 대하여 - Hash에 대하여
date: 2024-01-31 06:44:00 +09:00
categories:
  - java
tags:
  - [java, Hash , HashNap ]
---
## 들어가며

이번 포스트 에서는 Hash에 대하여 공부해볼 것입니다.

## Hash?

고정된 크기로 값을 바꾸는 함수 or 알고리즘

### java에서 hash

**키**와 **값**의 쌍을 저장하는 데이터 구조입니다. 

키는 고유한 식별자이며 값은 키와 연관된 데이터입니다. 

해시 테이블이라고도 불리는 해시 구조는 데이터 검색 및 삽입 속도가 매우 빠르다는 장점이 있습니다.

어떤 키값을 통해 해시함수로 고정된 크기의 해시 값을 만들고 이 값으로 배열의 인덱스 , 위치 , 데이터 값을 저장하거나 검색할 때 활용합니다.

해시 코드는 고정된 길이의 정수 값이며, 일반적으로 키의 일부 비트를 추출하거나 키를 제곱한 후 일부 비트를 추출하는 방식으로 계산됩니다.

### 해쉬함수

임의의 데이터를 고정된 길이의 값으로 리턴해주는 함수

## Array쓰면 되는거 아닌가?

다른 블로그에서 hash에 대해 조사하다 보니 목차를 예시로 드는 경우가 있었습니다.

사실 목차라는게 index이고 index 방식으로 자료를 정렬해서 찾아 내는건 Array도 유사하게 동작하는 방식이고 ,  array의 단점인 데이터 변경에 어려운점이 있다면 LinkedList 로 구현할 수 있기 때문입니다.

그래서 굳이? hash가 필요한지에 대해 생각해봤습니다.

HashMap의 내부구조는 배열로 되어 있고, Key는 직접 내부 배열의 인덱스가 될 수 있으며, 이를 버킷이라 한다. 키 값은 해사함수의 의해 생성된다. 

즉, 배열의 장점인 인덱스를 통한 빠르게 검색할 수 있다는 점과 가변 키를 사용해 수정/삽입이 용의한 자료구조이다.

완전 탐색이 필요하지 않고 , 특정 부분의 변경을 저장할 때 용의하다. 

예를 들어 String 값을 기반으로 한 어떤 문제를 해결하려고 한다면 어떨까

```java
1. A B C
2. A C
```

두 개의 리스트값을 받아서 2번 리스트의 값들 중 1번에 없는 애들만 골라내는 방법을 구현하라고 생각해보겠습니다.

```java
boolean checkList [] = new boolean[1.length()]; 

for 0 -> i i<checkList.length(); i++ // 1번 리스트에 대한 for 
		checkList[i] = false;

for 0 -> i i<1.length(); i++
		for 0-> j j<2.length(); j++
				if(i == j){
					checkList[i] = true;
				}
int cnt = 0;
for 0 -> i i<checkList.length(); i++
		if(checkList[i] == false)
				cnt++;
				
char resultList = new Chareters[cnt];

int flag = 0;
for 0 -> i i<checkList.length(); i++
		if(checkList[i] == false)
				resultList[flag++] = 1[i] ; 
```

수도 코드로 간단하게 생각해보면 이런식으로 풀어낼 수 있을 것입니다.

문제를 풀기 위해 상당히 많은 소요가 필요하고 코드도 비효율적으로 보일 수 밖에 없겠죠

자바에서의 HashMap을 통해 풀어보면 다음과 같습니다.

```java
public static List<Character> findUniqueCharacters(List<Character> list1, List<Character> list2) {
        if (list1 == null || list2 == null || list1.isEmpty() || list2.isEmpty()) {
            throw new IllegalArgumentException("Input lists cannot be null or empty");
        }

        
        HashMap<Character, Integer> charCounts = new HashMap<>();
        for (Character ch : list1) {
            charCounts.put(ch, 1);
        }

        List<Character> uniqueChars = new ArrayList<>();
        for (Character ch : list2) {
            if (!charCounts.containsKey(ch)) {
                uniqueChars.add(ch);
            }
        }

        return uniqueChars;
    }
```

딱 봐도 이해하기도 편하고 반복문이 많지 않기 때문에 쉽게 확인 할 수 있습니다.

이런식으로 String값으로 표현된 경우 (위에는 편의상 charcter로 함) 훨씬 쉽게 문제를 풀어볼 수 있습니다.

즉, 순서가 중요하지 않고 값이 중요한 경우에는 이런식으로 문제를 해결하는 것이 더욱 좋은 방법일 것입니다.

## 해쉬의 장단점

**장점:**

- **빠른 검색 및 삽입:** 해시 테이블은 키 기반 데이터 검색 및 삽입 속도가 매우 빠릅니다. 평균적으로 O(1)의 시간 복잡도를 가지고 있습니다. 이는 다른 데이터 구조(예: 트리, 연결 리스트)에 비해 훨씬 빠릅니다. → 단, 모든 경우에서 충돌이 날 경우 O(n)임
- **효율적인 메모리 사용:** 해시 테이블은 데이터를 밀집하게 저장하기 때문에 메모리 사용 효율이 높습니다.

**단점:**

- **해시 충돌:** 서로 다른 키가 동일한 해시 코드를 가질 경우 발생합니다. 해시 충돌을 해결하기 위해 다양한 방법들이 사용됩니다. 가장 일반적인 방법은 **분리 연결법**으로, 해시 코드가 동일한 키들을 연결 리스트에 저장하는 방식입니다. 해시 충돌이 발생하면 검색 및 삽입 성능이 저하될 수 있습니다.
- **해시 함수 의존성:** 해시 테이블의 성능은 해시 함수의 질에 크게 의존합니다. 좋은 해시 함수는 키를 고르게 분산시키고 해시 충돌을 최소화해야 합니다.

## 해쉬충돌 해결법

**1. 분리 연결법 (Separate Chaining)**

- 동일한 해시 코드를 가진 키들을 연결 리스트에 저장합니다. → LinkedList
- 장점: 간단한 구현, 해시 충돌 발생 시 영향 범위 제한
- 단점: 연결 리스트 길이 증가 시 검색 성능 저하

분리 연결법은 해시 충돌 발생 시 연결 리스트에 키-값 쌍을 추가하여 해결합니다. 연결 리스트는 동일한 해시 코드를 가진 키들을 순서대로 저장하는 구조입니다.
![약간의 본문 텍스트 추가](https://github.com/Sejin-999/blog-test/assets/76008226/8f18a3c9-d144-4d1c-bc73-fb5ae6d1999c)


이런식으로 충돌된 부분에 대해서 연결 리스트를 통해 값들을 저장하는 방법을 사용해 볼 수 있습니다.

**2. 오픈 주소법 (Open Addressing)**

- 충돌 발생 시 다른 해시 버킷을 탐색하여 빈 공간을 찾습니다.
- 일반적인 오픈 주소법으로는 선형 탐색, 이중 해시, 재해시 등이 있습니다.
- 장점: 연결 리스트 사용에 비해 빠른 검색 성능
- 단점: 해시 충돌 발생 시 탐색 과정 필요, 일부 해시 버킷 집중될 수 있음

오픈 주소법은 충돌 발생 시 다른 해시 버킷을 탐색하여 빈 공간을 찾아 키-값 쌍을 저장합니다. 탐색 방법은 다양하며, 일반적인 방법으로는 선형 탐색, 이중 해시, 재해시 등이 있습니다.

둘다 해결법이지만 일반적으로 데이터양과 충돌횟수에 따라 구분해서 사용됩니다.

데이터 와 충돌 횟수가 적은 경우 분리 연결법을 활용하는 것이 좋은 방법이지만,

반대인 경우 오픈 주소법을 통해 새롭게 위치를 재 선택해주는 것이 선호됩니다.


## Hash Map - hash table

일반적으로 hash 를 활용하는 기술 중 Hash Map 을 많이 사용하게 됩니다. 여기서 hash table을 통해 저장 방법을 선택합니다.

여기서 동작원리에 대해 간단히 이해해 보겠습니다.

![약간의 본문 텍스트 추가 (1)](https://github.com/Sejin-999/blog-test/assets/76008226/66bdaa44-ec48-46c9-9806-107a19f9cc63)


어떤 값을 넣어주고 → 함수를 통해 해쉬값을 만들면 이값을 모드연산을 통해 인덱스로

사용하는 것을 hashTable이라고 합니다. 

따라서 hashMap은 array자료구조를 가지게 되어서 자료를 찾는데 빠르게 접근할 수 있게 되는 것입니다.