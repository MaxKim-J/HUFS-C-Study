# heap
2020.01.23

## 정의
일차원 배열 중에서 저장된 값이 힙 조건을 만족하는 리스트  
완전 이진트리의 일종의로 우선순위 큐 구현에 사용한다  

### 힙 조건  
리스트를 이진트리로 해석하면  
1. 마지막 레벨을 제외한 레벨엔 빠짐없이 노드가 존재
2. 마지막 레벨의 노드는 왼쪽부터 차례대로 빈틈없이 채워짐  
3. 루트 노드를 제외한 모든 노드의 값은 부모 노드의 값보다 크지 않아야 함

### 특징  
1. 일종의 반 정렬 상태를 유지(느슨한 정렬 상태) : 큰 값이 상위레벨에 있고 작은 값이 하위 레벨에 있다는 정도의 정렬상태
2. 힙 트리에서는 대체로 중복된 값을 허용함
3. 노드 A[k]의 왼쪽/오른쪽 자식노드의 인덱스와 부모노드의 인덱스를 O(1)시간 안에 계산 가능함
    - 왼쪽 자식노드 : A[2k+1]
    - 오른쪽 자식노드 : A[2k+2]
    - 부모노드 : A[(k-1)/2]

### 종류
1. max heap : 부모 노드의 키 값이 자식 노드의 키 값보다 크거나 같은 완전 이진트리
2. min heap : 부모 노드의 키 값이 자식 노드의 키 값보다 작거나 같은 완전 이진트리

## 연산 구현

### 힙 만들기
```python
def heapify_down(self, k, n): 
  while 2*k+1 < n:          
    L, R = 2*k + 1, 2*k + 2	 
    if L < n and self.A[L] > self.A[k]: 
      m = L
    else: 
      m = k
    if R < n and self.A[R] > self.A[m]: 
      m = R 
    if m != k:	
      self.A[k], self.A[m] = self.A[m], self.A[k]
      k = m
    else: 
      break	

def make_heap(self):
	n = len(self.A)
	for k in range(n-1, -1, -1): #
		self.heapify_down(k, n)
```
A[k]는 자신의 자손 노드들보다 크거나 같아야 함  
A[k]를 자식노드의 값과 비교하면서 더 큰값을 갖는 자식노드와 바꾸는 과정을 반복함  
마지막 노드부터 첫 노드까지 차례로 heapify_down을 호출해서 힙으로 만든다 => make_heap  
### 힙소트
일단 주어진 리스트를 힙으로 만든 후  
맨 끝값과 루트노드를 계속 바꿔가면서 새로운 루트가 힙 성질을 만족하도록 히피파이 다운  
새로운 루트값을 히피파이 다운 했을 때 루트에는 가장 큰값/가장 작은 값이 온다는 것을 이용

```python
def heap_sort(self):
  n = len(self.A)	
  for k in range(len(self.A)-1, -1, -1):
    self.A[0],self.A[k] = self.A[k],self.A[0]
    n = n - 1	
    self.heapify_down(0, n)
```
### 힙의 크기
노드 개수별 최대 노드수를 계산하면, 레벨 h의 노드수는 최대 2^h  
전체 노드 수 n은 최소 level 0 노드 수+ ... +level h-1노드수 이상이어야 함  
즉 n >= 1 + 2^1 + 2^2 + ... +2^h-1 
양변에 로그를 취하면 h <= log2(n+1)이 되므로 h = O(log n)
### 삽입삭제
1. 삽입 : 힙의 가장 오른쪽에 새로운 값 x를 저장하고, 힙 성질이 만족하도록 위치를 재조정해야 함. 삽입한 노드를 위쪽의 부모노드들과 값을 비교하면서 위치 조정(heapify up) 
2. 삭제 : 루트노드에 있는 최대/최소값을 삭제하여 리턴, 남은 힙의 힙 성질은 그대로 유지시킴. 루트와 꼬리값을 바꾸고 꼬리값 pop시킨 다음 루트를 기준으로 히피파이 다운

