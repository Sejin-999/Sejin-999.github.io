---
title: 알고리즘 - 선택정렬
date: 2024-01-23 06:44:00 +09:00
categories:
  - algorithm
tags:
  - [java, algorithm, sort , selection sort ]
---
선택 정렬은 리스트를 순회하면서 가장 작은(또는 가장 큰) 원소를 선택하여 알맞은 위치로 이동시키는 방식으로 동작합니다.

## 핵심

남은 정렬부분에서 최솟값 또는 최댓값을 찾는다

남은 정렬 부분에서 가장 앞에있는 데이터와 선택된 데이터를 swap한다.

가장 앞에 있는 데이터의 위치를 변경해 남은 정렬 부분을 축소한다.

전체 데이터 크기만큼 인덱스가 커질때 까지 즉, 남은 부분이 없을 때까지 반복한다.

## 예시코드

```java
public class SelectionSort {
    public static void selectionSort(int[] arr) {
        int n = arr.length;

        // 배열을 순회하면서 최소값의 인덱스를 찾아서 알맞은 위치로 이동시킴
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

## 예제코드 - 백준1427

```java
 private static void test() throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        String s = String.valueOf(n);

        int [] arrN = new int[s.length()];
        for(int i=0; i<s.length(); i++){
            arrN[i] = Integer.parseInt(s.substring(i,i+1));
        }
        for (int i = 0; i < arrN.length - 1; i++) {
            int maxIndex = i;
            for (int j = i + 1; j < arrN.length; j++) {
                if (arrN[j] > arrN[maxIndex]) {
                    maxIndex = j;
                }
            }
            if (maxIndex != i) {
                // Swap
                int temp = arrN[i];
                arrN[i] = arrN[maxIndex];
                arrN[maxIndex] = temp;
            }
        }

        for (int a : arrN) {
            System.out.println(a);
        }
    }
```