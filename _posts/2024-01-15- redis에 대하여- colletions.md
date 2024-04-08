---
title: redis에 대하여 - colletions
date: 2024-01-15 06:44:00 +09:00
categories:
  - redis
tags:
  - [redis, colletions, data]
---
## 들어가며

이번 포스트에서는 Colletions에 대하여 이야기해보겠습니다.

## Colletions

**컬렉션은 레디스에서 제공하는 다양한 데이터 구조를 말합니다.**

1. Strings: 가장 간단한 형태의 데이터 구조로 문자열을 저장합니다. 키와 값을 하나의 문자열로 저장하고, 여러 가지 연산을 수행할 수 있습니다.
2. Lists: 순서가 있는 문자열 목록을 저장합니다. 리스트에 요소를 추가하거나 제거하고, 인덱스로 요소를 조회하거나 수정할 수 있습니다.
3. Sets: 중복이 허용되지 않는 문자열의 집합을 저장합니다. 집합에 요소를 추가하거나 제거하고, 합집합, 교집합, 차집합 등의 연산을 수행할 수 있습니다.
4. Sorted Sets: Sets와 유사하지만 각 요소에 순서가 지정되어 있습니다. 순서는 점수(score)로 지정되며, 요소는 이 점수에 따라 정렬됩니다.
5. Hashes: 키-값 쌍의 집합을 저장합니다. 각 키는 하나의 값과 연관되며, 여러 가지 연산을 통해 키의 값을 조회하거나 수정할 수 있습니다.

코드 예시를 보며 실제로 해보겠습니다.

**Strings**

```

> SET mykey "Hello"
OK
> GET mykey
"Hello"

```

Redis에서 "Hello"라는 문자열을 "mykey"라는 키에 저장하는 명령어입니다.

 SET은 Redis의 문자열 데이터 타입을 설정하는 명령어이며, mykey는 키(key)이고 "Hello"는 해당 키에 대응하는 값(value)입니다. 

이렇게 저장된 값은 나중에 GET 명령어를 사용하여 다시 검색할 수 있습니다.

![1](https://github.com/Sejin-999/blog-test/assets/76008226/77e6e60d-7d5b-472e-924e-b61c74f69aed)

**Lists**

```

> LPUSH mylist "world"
(integer) 1
> LPUSH mylist "hello"
(integer) 2
> LRANGE mylist 0 -1
1) "hello"
2) "world"

```

Redis에서 Lists는 순서가 있는 문자열 요소의 컬렉션을 나타냅니다. 이 리스트는 중복을 허용하며, 요소의 순서는 삽입 순서를 따릅니다.

Lists는 연결 목록(linked list) 형태로 구현되어 있어서 삽입과 삭제가 O(1)의 시간 복잡도를 갖습니다.

Lists의 주요 특징

1. **순서 유지**: 요소들은 삽입된 순서대로 저장되며, 인덱스를 통해 접근할 수 있습니다.
2. **중복 허용**: 동일한 요소를 여러 번 삽입할 수 있습니다.
3. **삽입/삭제 효율**: 연결 목록의 형태로 구현되어 있어서 삽입과 삭제가 효율적입니다.
4. **리스트의 길이 제한 없음**: Redis의 Lists는 동적으로 크기가 조정되므로 길이에 제한이 없습니다.

![2](https://github.com/Sejin-999/blog-test/assets/76008226/8fdf7530-a0ef-4a78-ad40-8b6814790b17)

길이를 주는 List + range → Lrange 를 통해 검색할 수 있고 0 -1 은 전체 조회를 의미합니다.

**Sets**

```

> SADD myset "apple"
(integer) 1
> SADD myset "orange"
(integer) 1
> SADD myset "banana"
(integer) 1
> SMEMBERS myset
1) "apple"
2) "banana"
3) "orange"

```

Redis에서 Sets(집합)은 중복되지 않는 고유한 요소들을 저장하는 데이터 구조입니다.

 Sets는 각 요소가 키와 연관된 정렬되지 않은 데이터 구조로, 각 요소는 중복되지 않습니다. 

**특징**

1. 중복 요소가 없음: 각 요소는 집합 내에서 고유합니다.
2. 정렬되지 않음: 요소들은 특정 순서로 저장되지 않으며, 요소의 순서는 무작위입니다.
3. 집합 연산 지원: Redis Sets는 여러 집합 연산을 지원하며, 이를 통해 교집합, 합집합, 차집합 등을 쉽게 수행할 수 있습니다.
4. 추가, 삭제, 존재 여부 확인 등 다양한 작업 지원: 요소를 집합에 추가하거나 삭제하고, 특정 요소의 존재 여부를 확인하는 등의 작업을 지원합니다.

Sets는 주로 고유한 값의 컬렉션을 저장하고 관리하는 데 사용됩니다.

![3](https://github.com/Sejin-999/blog-test/assets/76008226/4f0f7ffe-6237-40da-ba61-4981b4fe98bd)

**Sorted Sets**

```

> ZADD myzset 1 "one"
(integer) 1
> ZADD myzset 2 "two"
(integer) 1
> ZADD myzset 3 "three"
(integer) 1
> ZRANGE myzset 0 -1 WITHSCORES
1) "one"
2) "1"
3) "two"
4) "1"
5) "three"
6) "1"

```

Sorted Sets는 Set과 유사하지만 각 멤버에 대해 연결된 점수를 가지고 있습니다. 

이로써 멤버들은 정렬되어 있으며, 멤버 간의 관계를 표현할 수 있습니다. 

Sorted Sets는 멤버의 추가, 제거 및 갱신을 허용하며, 멤버에 대한 순위 및 범위 기반의 쿼리를 지원합니다.

![4](https://github.com/Sejin-999/blog-test/assets/76008226/41694c23-4dbc-42ff-89c7-e6ffeb0161e4)

**Hashes**

```

> HSET myhash field1 "value1"
(integer) 1
> HSET myhash field2 "value2"
(integer) 1
> HGETALL myhash
1) "field1"
2) "value1"
3) "field2"
4) "value2"

```

 여러 필드와 값의 쌍을 저장하는 데이터 구조입니다. 각 필드는 문자열이며, 각 값은 문자열이거나 숫자일 수 있습니다. 

Hashes는 사용자가 쉽게 필드를 식별하고 해당 필드에 대한 값을 검색할 수 있도록 해줍니다.

Hashes는 주로 관련 데이터를 하나의 키 아래에 그룹화하고 싶을 때 사용됩니다. 예를 들어, 사용자 프로필, 제품 정보, 설정 등과 같이 연관된 데이터를 저장하고 검색하는 데 유용합니다.