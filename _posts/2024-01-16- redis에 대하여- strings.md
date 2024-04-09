---
title: redis에 대하여 - strings
date: 2024-01-16 06:44:00 +09:00
categories:
  - redis
tags:
  - [redis, colletions, strings]
---
## 들어가며

이번 포스트에서는 colletions 중 하나인 Lists 에 대하여 알아보겠습니다.

## 개념

 Lists는 순서가 있는 문자열 요소들의 컬렉션을 저장하는 데 사용됩니다. 각 요소는 인덱스로 식별되며, 리스트의 시작 또는 끝에 요소를 추가하거나 제거하는 등의 작업을 수행할 수 있습니다. 

자바로 치면 ArrayList나 LinkedList와 유사한 데이터 구조 라고 알고 있습니다.

쉽게 생각해 각 요소에 인덱스로 접근한다. 시작과 마지막을 통해 요소를 추가 , 삭제하는 작업을 할 수 있는 자료 구조입니다.

사실 알고리즘을 공부하면 배열과 리스트는 다르게 구현합니다. 

배열은 인덱스를 통해 빠르게 조회가 가능한 대신 처음과 끝이 아닌 경우 다소 복잡한 방법을 거쳐 데이터를 추가해야 합니다.

리스트는 인덱스 대신 노드와 이걸 연결하는 링크를 가지고 있습니다. 따라서 중간에 값을 연산해도 배열보다 빠르게 처리가 가능합니다.

그래서 저는 보통 값을 쌓는 형태의 구조가 필요하면 배열을 잦은 연산이 필요한 경우 리스트를 사용한다고 알고 있었는데요

이런 문제점을 개발자들이 이미 좋은 방식으로 바꿔서

ArrayList나 LinkedList 를 사용할 수 있도록 해두었습니다.

따라서 Redis에서 Lists를 사용할때는 처음과 끝만 연산이 가능한 큐처럼 사용할 수 있고

Stack으로도 구현할 수 있다는 것도 같은 의미입니다.

만약 중간에 값을 연산해야 한다면 다른 자료구조를 사용하거나

삽입 부 전까지 삭제를 하고 넣은 다음 삭제한 부분을 다시 채워 넣는 방식으로 구현해야 합니다.

## 사용법

### **LPUSH/RPUSH**

리스트의 왼쪽/오른쪽에 요소를 추가합니다.

```sql
LPUSH key value1 [value2 ...] / RPUSH key value1 [value2 ...]
```

**ex**

```sql
LPUSH mylist "1"
```

![제목 추가](https://github.com/Sejin-999/blog-test/assets/76008226/f42519bd-424e-46ac-87fc-9259fa275a0a)

```sql
RPUSH mylist "2"
```

![제목 추가 (1)](https://github.com/Sejin-999/blog-test/assets/76008226/f13978ff-4965-429b-8bb0-f55888626832)

```sql
LPUSH mylist "3"
```

![제목 추가 (2)](https://github.com/Sejin-999/blog-test/assets/76008226/9972cd63-0c67-4de2-9dce-5b4bd0dab66a)

![1](https://github.com/Sejin-999/blog-test/assets/76008226/41c218fc-ec86-432d-b1fc-3cddd9a76383)

### **LPOP/RPOP**

리스트의 왼쪽/오른쪽에서 요소를 가져오고 제거합니다.

흔히 큐에서 사용하는 pop이라고 생각해도 동일합니다.

```sql
Lpop mylist
```

![제목 추가 (3)](https://github.com/Sejin-999/blog-test/assets/76008226/da53e428-cfde-4027-9d21-ca2fa7667681)

```sql
Rpop mylist
```

![제목 추가 (4)](https://github.com/Sejin-999/blog-test/assets/76008226/6d96b695-bd5f-411a-b85e-bad0c610b51d)

![2](https://github.com/Sejin-999/blog-test/assets/76008226/c281890b-84b2-4404-a070-1e4b2b7cfbd8)

### **LRANGE**

리스트에서 지정된 범위의 요소를 가져옵니다.

**LRANGE key start stop**

```sql
Lrange mylist 0 -1
```

![3](https://github.com/Sejin-999/blog-test/assets/76008226/f9839368-80d7-4b09-bb98-25a3247786d5)
이렇게 범위를 볼 수도 있습니다.

여기서 L은 List의 Range 라고 생각하시면 됩니다.

위 push와 pop 의 L 은 Left라고 생각하시면 이해하기 더욱 좋습니다.

### **LLEN**

리스트의 길이(요소의 수)를 반환합니다.

**LLEN key**

```sql
Llen mylist
```

![4](https://github.com/Sejin-999/blog-test/assets/76008226/5ef0c28b-d0a4-48ea-8829-5bb45f332338)

![제목 추가 (5)](https://github.com/Sejin-999/blog-test/assets/76008226/27006803-ae31-4c57-a542-febb0f1b8574)

### **LINDEX**

리스트에서 지정된 인덱스의 요소를 가져옵니다.

**LINDEX key index**

![5](https://github.com/Sejin-999/blog-test/assets/76008226/afd3d8fe-c47d-45fe-8d1a-48a10aa0683f)

### **LREM**

리스트에서 지정된 값을 삭제합니다.

**LREM key count value**

```sql
LREM mylist 2 "2"
```

mylist 에서 “2” 값을 가진 것을 최대 2개까지 삭제한다는 의미입니다.

ex

일단 Rpush로 두개의 “2”를 삽입하겠습니다.

![제목 추가 (6)](https://github.com/Sejin-999/blog-test/assets/76008226/bccb704a-c6a9-4483-88cc-f7ef5a0e0f0c)

![6](https://github.com/Sejin-999/blog-test/assets/76008226/b6065cbf-4e2f-49fc-928b-678466d484df)

이렇게 삭제가 되었습니다.

![제목 추가 (7)](https://github.com/Sejin-999/blog-test/assets/76008226/9aeafaa1-b280-4083-a9ae-3c5c54822f6b)

참고로 Rrem이라고 하면 오른쪽부터 삭제될지 궁금해서 넣어봤는데

안되는걸 보니 List rem 이라는 뜻으로 사용되는 것 같습니다.
![7](https://github.com/Sejin-999/blog-test/assets/76008226/95d4e42c-5793-41b1-bff5-2c04ac9e3570)

![8](https://github.com/Sejin-999/blog-test/assets/76008226/d1363f17-257c-4684-b807-d153d15fa0dc)

![제목 추가 (8)](https://github.com/Sejin-999/blog-test/assets/76008226/26803648-574b-49a8-ade1-125dabd15908)

### **LTRIM**

리스트의 특정 범위의 요소만 유지하고 나머지 요소를 삭제합니다.

 **LTRIM key start stop**

![9](https://github.com/Sejin-999/blog-test/assets/76008226/5d1f48bc-c686-474d-8c25-a23f3cc5d5a5)

이렇게 값을 일단 추가해주겠습니다.
![제목 추가 (9)](https://github.com/Sejin-999/blog-test/assets/76008226/353f1032-4576-4d5a-a65d-c5bf1ff95c79)

```sql
Ltrim mylist 0 1
```

이렇게 하면 0 ~1 을 제외한 나머지 요소를 삭제합니다.

![제목 추가 (10)](https://github.com/Sejin-999/blog-test/assets/76008226/a963e1af-d828-4ce8-a0a6-6e0fba5d277f)

![10](https://github.com/Sejin-999/blog-test/assets/76008226/f9bffe33-ff25-4da7-bb06-37bf16a970d8)

그래서 이런식으로 남게 됩니다.

## 정리하며

이번 포스트는 Redis 의 collections 중 Strings 타입에 대해 알아보았습니다.

redis를 쓸 떄 가장 기초적으로 사용할 수 있는 부분입니다.

추가로 redis 공식문서를 참고하면 더욱 많은 정보를 찾을 수 있습니다.

이번 포스트에서는 제가 사용할법한 , 사용한 내용만 정리했습니다.