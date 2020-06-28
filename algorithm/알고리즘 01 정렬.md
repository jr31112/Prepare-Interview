# 알고리즘 01 - 정렬

| 정렬        | 평균 수행시간 | 공간 복잡도 |
| ----------- | ------------- | ----------- |
| 버블 정렬   | O(`n²`)       | O(`1`)      |
| 카운팅 정렬 | O(`n + m`)    | O(`m`)      |
| 선택 정렬   | O(`n²`)       | O(`1`)      |
| 퀵 정렬     | O(`nlogn`)    | O(`n²`)     |
| 삽입 정렬   | O(`n²`)       | O(`n²`)     |
| 병합 정렬   | O(`nlogn`)    | O(`n`)      |
| 힙 정렬     | O(`nlogn`)    | O(`1`)      |



## 1. 버블정렬(Bubble Sort)

인접한 두개의 자료를 비교하며 자리를 계속 교환하는 방식

한 단계가 끝나면 가장 큰(작은) 원소가 마지막 자리로 정렬된다.

시간 복잡도 : `O(n²)`

```python
def bubblesort(a):
    for i in range(len(a)-1):
        for j in range(len(a)-i-1):
            (a[j], a[j+1]) = (a[j+1], a[j]) if a[j] > a[j+1] else (a[j], a[j+1])
    return a
```



## 2. 카운팅 정렬(Counting Sort)

항목들의 순서를 결정하기 위해 집합에 각 항목이 몇 개씩 있는지 세고 정렬하는 방식

시간 복잡도 : `O(n+k)` (n은 리스트의  길이, k는 정수의 최대값)

정렬 과정

* 정렬하고자 하는 값 중 **최대값에 해당하는 값을 size 로 하는 임시 배열** 을 만든다. 
* 개수를 센후 임시 배열을 누적해준다.
* 임시 배열의 index 는 정렬하고자 하는 값을 나타내고 value 는 정렬하고자 하는 값들이 정렬되었을 때의 index 를 나타내게 된다

```python
def countingsort(a):
    # max값 확인
    for i in range(len(a)-1):
        max_val = a[i] if i == 0 else max_val if max_val > a[i] else a[i]
    # count 리스트 생성
    cnt = [0]*(max_val+1)
    # 빈도수 계산
    for val in a:
        cnt[val] += 1
    # 누적 빈도수 계산
    for i in range(len(cnt)):
        cnt[i] = cnt[i] + cnt[i-1] if i!=0 else cnt[i]
    # 정렬
    print(cnt)
    result = [0]*len(a)
    for i in range(len(result)-1,-1,-1):
        result[cnt[a[i]]-1] = a[i]
        cnt[a[i]] -= 1
    return result
```



## 3. 선택 정렬(Selection Sort)

> 주어진 값중 최소값(최대값)을 찾는다
>
> 그값을 맨앞과 교환한다

```python
def selection(a):
    n = len(a)
    for i in range(n):
        MIN = 0xffff
        index = i
        for j in range(i, n):
            if a[i] < MIN:
                index, MIN = j, a[j]
        a[i], a[index] = a[index], a[i]
    return a            
```



## 4. 퀵 정렬(Quick Sort)

> 피봇 설정 후 분할된 값들을 정렬
>
> 여기있는 방법은 효율적인 방법이 아니당

`Quick Sort`는 `Divide and Conquer` 전략을 사용하여 Sorting 이 이루어진다. Divide 과정에서 `pivot`이라는 개념이 사용된다. 입력된 배열에 대해 오름차순으로 정렬한다고 하면 이 pivot 을 기준으로 좌측은 pivot 으로 설정된 값보다 작은 값이 위치하고, 우측은 큰 값이 위치하도록 `partition`된다. 이렇게 나뉜 좌, 우측 각각의 배열을 다시 재귀적으로 Quick Sort 를 시키면 또 partition 과정이 적용된다.이 때 한 가지 주의할 점은 partition 과정에서 pivot 으로 설정된 값은 다음 재귀과정에 포함시키지 않아야 한다. 이미 partition 과정에서 정렬된 자신의 위치를 찾았기 때문이다.

* Quick Sort's worst case
  그렇다면 어떤 경우가 Worst Case 일까? Quick Sort 로 오름차순 정렬을 한다고 하자. 그렇다면 Worst Case 는 partition 과정에서 pivot value 가 항상 배열 내에서 가장 작은 값 또는 가장 큰 값으로 설정되었을 때이다. 매 partition 마다 unbalanced partition이 이뤄지고 이렇게 partition 이 되면 비교 횟수는 원소 n 개에 대해서 n 번, (n-1)번, (n-2)번 … 이 되므로 시간 복잡도는 O(n^2) 이 된다.

* Balanced-partitioning
  자연스럽게 Best-Case 는 두 개의 sub-problems 의 크기가 동일한 경우가 된다. 즉 partition 과정에서 반반씩 나뉘게 되는 경우인 것이다. 그렇다면 Partition 과정에서 pivot 을 어떻게 정할 것인가가 중요해진다. 어떻게 정하면 정확히 반반의 partition 이 아니더라도 balanced-partitioning 즉, 균형 잡힌 분할을 할 수 있을까? 배열의 맨 뒤 또는 맨 앞에 있는 원소로 설정하는가? Random 으로 설정하는 것은 어떨까? 특정 위치의 원소를 pivot 으로 설정하지 않고 배열 내의 원소 중 임의의 원소를 pivot 으로 설정하면 입력에 관계없이 일정한 수준의 성능을 얻을 수 있다. 또 악의적인 입력에 대해 성능 저하를 막을 수 있다.

* Partitioning
  정작 중요한 Partition 은 어떻게 이루어지는가에 대한 이야기를 하지 않았다. 가장 마지막 원소를 pivot 으로 설정했다고 가정하자. 이 pivot 의 값을 기준으로 좌측에는 작은 값 우측에는 큰 값이 오도록 해야 하는데, 일단 pivot 은 움직이지 말자. 첫번째 원소부터 비교하는데 만약 그 값이 pivot 보다 작다면 그대로 두고 크다면 맨 마지막에서 그 앞의 원소와 자리를 바꿔준다. 즉 pivot value 의 index 가 k 라면 k-1 번째와 바꿔주는 것이다. 이 모든 원소에 대해 실행하고 마지막 과정에서 작은 값들이 채워지는 인덱스를 가리키고 있는 값에 1 을 더한 index 값과 pivot 값을 바꿔준다. 즉, 최종적으로 결정될 pivot 의 인덱스를 i 라고 했을 때, 0 부터 i-1 까지는 pivot 보다 작은 값이 될 것이고 i+1 부터 k 까지는 pivot 값보다 큰 값이 될 것이다.

1. 왼쪽이 피봇구현

   ```python
   def quickSort(lo, hi)
   	if lo >= hi: return
       i, j = lo, hi # arr[lo] : 피봇
       while i < j:
           while i <= hi and arr[lo] >= arr[i]: i += 1
           while arr[lo] < arr[j]:j -= 1
           if i < j:
               arr[i], arr[j] = arr[j], arr[i]
       arr[lo], arr[j] = arr[j], arr[li]
   	quickSort(lo, j-1)
       quickSort(j+1, hi)
   ```

2. 오른쪽이 피봇구현(Lomuto partion)

   ```python
   def partition(lo, hi):
       i = lo -1
       for j in range(lo, hi):
           if arr[j] < arr[hi]:
               i += 1
               arr[i], arr[j] = arr[j], arr[i]
       i += 1
       arr[i], arr[hi] = arr[hi], arr[i]
       partition(lo, i-1)
       partition(i+1, hi)
   ```




## 5. 삽입 정렬(Insertion Sort)

1. 정렬 과정
   - 정렬할 자료를 정렬된 원소와 정렬되지 않은 원소로 구분
   - 정렬되지 않은 부분 집합의 원소를 하나 씩 꺼내서 이미 정렬 되어있는 부분 집합의 마지막 원소부터 비교하면서 삽입
2. 시간 복잡도
   - O(n^2)
3. 구현

```python
arr = [69, 10, 30, 2, 16, 8, 31, 22]
N = len(arr)
# 삽입하는 작업을 n-1번 반복 (1~n-1)
for i in range(1, N):
    tmp = arr[i]
    j = i -1
    while j >= 0 and tmp < arr[j]:
        arr[j+1] = arr[j]
        j -= 1
    arr[j + 1] = tmp
```



## 6. 병합 정렬(Merge Sort)

1. 정렬 과정

   - 여러 개의 정렬된 자료의 집합을 병합하여 한 개의 정렬된 집합으로 만드는 방식
   - 분할 정복 알고리즘을 활용하여 최소 단위 까지 문제를 나눈 후에 차례대로 정렬하여 최종 결과를 얻어낸다.

2. 시간 복잡도

   - O(n log n)

3. 구현

   ```python
   def mergeSort(arr):
       if len(arr) <= 1:
           return arr
       mid = int(len(arr) / 2)
       left = arr[:mid]
       right = arr[mid:]
   
       left = mergeSort(left)
       right = mergeSort(right)
   
       result = []
       while len(left) > 0 and len(right) > 0:
           if left[0] < right[0]:
               result.append(left.pop(0))
           else:
               result.append(right.pop(0))
       if len(left) > 0:
           result += left
       else:
           result += right
       return result
   ```

   ```python
   def mergeSort(lo, hi):
       if lo >= hi:
           return
   
       mid = (lo + hi) >> 1
       mergeSort(lo, mid)
       mergeSort(mid + 1, hi)
   
       i, j, k = lo, mid + 1, lo
       while i <= mid and j <= hi:
           if arr[i] < arr[j]:
               sort[k] = arr[i]
               k, i = k + 1, i + 1
           else:
               sort[k] = arr[j]
               k, j = k + 1, j + 1
       while i <= mid:
           sort[k] = arr[i]
           k, i = k + 1, i + 1
       while j <= hi:
           sort[k] = arr[j]
           k, j = k + 1, j + 1
       for i in range(lo, hi + 1):
           arr[i] = sort[i]
   ```