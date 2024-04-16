---
title: 알고리즘 - 버블소트
date: 2024-01-22 06:44:00 +09:00
categories:
  - algorithm
tags:
  - [java, algorithm, sort , bubble sort ]
---
버블 정렬(Bubble Sort)은 간단하면서도 비효율적인 정렬 알고리즘 중 하나입니다. 이 알고리즘은 인접한 두 요소를 비교하여 필요한 경우 위치를 교환하는 방식으로 동작합니다.

여기서 각 패스(반복)마다 가장 큰 요소가 맨 끝으로 이동하므로, "거품이 물 위로 떠오르는 것과 같다"는 개념에서 유래하여 "버블(Bubble)"이라는 이름이 붙여졌습니다.

예시 코드

```java
public class BubbleSort {
    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    // 인접한 두 요소의 값이 순서대로 되어 있지 않으면 교환
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
        }
    }

    public static void main(String[] args) {
        int[] arr = {64, 34, 25, 12, 22, 11, 90};
        bubbleSort(arr);
        System.out.println("Sorted array:");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}
```

내가 만든 코드

```java
    private static void test() throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st= new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int [] arrN = new int[n];

        for(int i=0; i<n; i++){
            int a = Integer.parseInt(br.readLine());
            arrN[i] = a;
        }
        for(int i=0; i<n-1; i++){
            for(int j=0; j<n-1; j++){
                if(arrN[j] > arrN[j+1]){
                    int temp = arrN[j];
                    arrN[j] = arrN[j+1];
                    arrN[j+1] = temp;
                }
            }
        }

        for(int a : arrN){
            System.out.println(a);
        }

    }
```

## 핵심

두개 비교하기 위해 2중 중첩문을 통해 구현한다.

비교조건에 따라 Swap문을 만든다.

## 예제문제

```java
 private static void test() throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st= new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int [] arrN = new int[n];
        int [] arrS = new int[n];
        for(int i=0; i<n; i++){
            int a = Integer.parseInt(br.readLine());
            arrN[i] = a;
            arrS[i] = a;
        }
        Arrays.sort(arrS);

        HashMap<Integer, Integer> hashMap1 = new HashMap<>();
        HashMap<Integer, Integer> hashMap2 = new HashMap<>();
        for(int i =0; i<n; i++){
            hashMap1.put(arrN[i],i);
            hashMap2.put(arrS[i],i);
        }

        int maxDiff = Integer.MIN_VALUE;
        for (Integer key : hashMap1.keySet()) {
            if (hashMap2.containsKey(key)) {
                int diff = hashMap1.get(key) - hashMap2.get(key);
                maxDiff = Math.max(maxDiff, diff);
            }
        }

        System.out.println(maxDiff + 1);
    }

    public static void main(String[] args) throws IOException {
        test();
    }
```