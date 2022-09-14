## 최장 증가 부분 수열(Longest Increasing Subsequence, LIS)란?
+ 원소가 n개인 배열의 일부 원소를 골라내서 만든 부분 수열 중 각 원소가 이전 원소보다 크다는 조건을 만족하고 그 길이가 최대인 부분 수열
  + ex) {6, 2, 5, 1, 7, 4, 8, 3} -> {2, 5, 7, 8}
+ 두 가지 문제 풀이 방법
  + 동적 계획법(Dynamic Programming)
  + 이분 탐색(Binary Search)

## LIS 알고리즘 구현
### 동적 계획법
+ **10  20  10  30  20  50**
  + 위와 같은 수열이 주어졌을 때 최장 증가 부분 수열은 [10, 20, 30, 50] 이며 길이는 4
```python
x = int(input()) # 수열 A의 크기
arr = list(map(int, input().split())) # 수열 A를 이루고 있는 A(i)를 담은 배열
dp = [1 for i in range(x)] # arr[i]를 마지막 원소로 가질 때 최장 증가 부분 수열의 길이

for i in range(x):
  for j in range(i):
    if arr[i] > arr[j]:
      dp[i] = max(dp[i], dp[j] + 1)
      
print(max(dp))
```
1. dp[i] 값을 1로 초기화
2. 현재 위치(i)보다 이전에 있는 원소(j)가 작은지 확인
3. 작다면 현재 위치의 이전 숫자 중 dp 최댓값을 구하고 그 길이에 1을 더함
4. dp 배열 원소 중 가장 큰 원소 출력
> N개의 수들에 대해 현재 위치 이전의 모든 수를 다 훑어봐야 하므로 시간 복잡도는 O(N^2)

### 이분 탐색 1
```python
import bisect

x = int(input())
arr = list(map(int, input().split()))

dp = [arr[0]]

for i in range(x):
  if arr[i] > dp[-1]:
    dp.append(arr[i])
  else:
    idx = bisect.bisect_left(dp, arr[i])
    dp[idx] = arr[i]
    
print(len(dp))
```
1. dp를 arr[0]로 초기화
2. 현재 위치(i)가 이전 위치 원소들보다 크면 dp에 추가
3. 현재 위치(i)가 이전 위치 원소보다 작거나 같으면, bisect.bisect_left로 이전 위치의 원소 중 가장 큰 원소의 index값을 구하고, dp의 index 원소를 arr[i]로 바꿈
4. dp 길이를 출력
> N개 수들에 대해 이진 탐색을 진행하므로 시간 복잡도는 O(NlongN)

### 이분 탐색 2
+ LIS 길이뿐만 아니라 LIS 원소들을 알고 싶다면?
```python
x = int(input())
arr = list(map(int, input().split()))

dp = [1 for i in range(x)]

for i in range(x):
  for j in range(i):
    if arr[i] > arr[j]:
      dp[i] = max(dp[i], dp[j] + 1)
      
max_dp = max(dp) # 최장 증가 부분 수열의 길이
max_idx = dp.index(max_dp) # max_dp의 위치
list = [] # 최장 증가 부분 수열 담을 배열

while max_idx >= 0:
  if dp[max_idx] == max_dp:
    list.append(arr[max_idx])
    max_dp -= 1
  max_idx -= 1
  
list.reverse()
print(list)
```
+ 수열 A의 각 원소 위치마다 가장 긴 증가하는 부분 수열의 길이를 알아내는 방식은 동일
