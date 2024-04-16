---
title: 알고리즘 - 삽입정렬
date: 2024-01-24 06:44:00 +09:00
categories:
  - algorithm
tags:
  - [java, algorithm, sort , insertion sort ]
---
이미 정렬된 데이터 범위에 정렬되지 않은 데이터를 적절한 위치에 삽입시켜 정렬하는 방법입니다.

촤악의 경우 ,- O(n^2)으로 느립니다.

## 동작방식

1. 주어진 리스트에서 가장 작은(또는 가장 큰) 요소를 찾습니다.
2. 해당 요소를 리스트의 맨 앞 요소와 위치를 교환합니다.
3. 정렬된 부분 리스트와 정렬되지 않은 부분 리스트로 나눕니다. 정렬된 부분 리스트는 계속해서 증가하고, 정렬되지 않은 부분 리스트는 감소합니다.
4. 위의 과정을 반복하여 정렬이 완료될 때까지 진행합니다.

## Sudo

```java
procedure selectionSort(A : list of sortable items)
    n = length(A)
    for i = 0 to n - 2 do
        minIndex = i
        for j = i + 1 to n - 1 do
            if A[j] < A[minIndex] then
                minIndex = j
        end for
        swap A[i] with A[minIndex]
    end for
end procedure

```

## 예시코드 - 자바

```java
public class SelectionSort {
    public static void selectionSort(int[] arr) {
        int n = arr.length;

        // 배열을 순회하면서 가장 작은 값을 찾아서 알맞은 위치로 이동시킴
        for (int i = 0; i < n - 1; i++) {
            int minIndex = i; // 현재까지의 최소값의 인덱스를 저장

            // 현재 위치부터 배열 끝까지 순회하면서 최소값의 인덱스를 찾음
            for (int j = i + 1; j < n; j++) {
                if (arr[j] < arr[minIndex]) {
                    minIndex = j;
                }
            }

            // 최소값을 현재 위치로 이동
            int temp = arr[minIndex];
            arr[minIndex] = arr[i];
            arr[i] = temp;
        }
    }

    public static void main(String[] args) {
        int[] arr = {64, 25, 12, 22, 11};
        selectionSort(arr);

        System.out.println("Sorted array:");
        for (int num : arr) {
            System.out.print(num + " ");
        }
    }
}

```