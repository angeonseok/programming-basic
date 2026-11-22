# 🐍 부록 A. 코딩테스트 활용

> 본문(01~14장)에서 배운 문법을 **코딩테스트(알고리즘 문제 풀이)**에 맞게 쓰는 법을 모았다. 빠른 입출력, 재귀 한도, 자료구조 선택, 그리고 자주 나오는 구현 뼈대(격자 탐색·DFS·BFS·다익스트라·이분 탐색)와 자주 하는 실수 목록이다.

---

## 빠른 입출력

### 빠른 입력

> `input()`은 느려서 입력이 수십만 줄이면 시간 초과가 날 수 있다. `sys.stdin.readline`으로 바꾸면 훨씬 빠르다.

```python
import sys
input = sys.stdin.readline      # 이후 input()이 빠른 버전으로 동작

n = int(input())
nums = list(map(int, input().split()))
line = input().rstrip()         # ⚠️ 문자열은 끝의 줄바꿈 제거 필수
grid = [list(map(int, input().split())) for _ in range(n)]   # n줄짜리 2차원 입력
```

> ⚠️ `sys.stdin.readline`은 줄 끝의 **`\n`까지 포함**해서 돌려준다. `int()`와 `split()`은 알아서 무시하지만, 문자열 그대로 쓸 땐 `.rstrip()`으로 지워야 한다. 안 그러면 `"hello\n"`처럼 줄바꿈이 붙은 값이 된다.

### 빠른 출력

> `print`를 수십만 번 부르는 것도 느리다. 결과를 모아 **한 번에** 출력하자.

```python
answers = [1, 2, 3]
print("\n".join(map(str, answers)))   # 한 줄에 하나씩, print 한 번
print(*answers)                       # 공백으로 구분해 한 줄에
```

---

## 재귀 한도

> 파이썬의 기본 재귀 깊이 한도는 **1000**이다. 깊이가 10만까지 갈 수 있는 DFS는 한도를 올리거나, 재귀 대신 **스택**으로 짠다. ([09. 스코프 · 재귀 · 람다](09.python_functions_advanced.md))

```python
import sys
sys.setrecursionlimit(10 ** 6)
```

> ⚠️ 한도를 올려도 실제 스택 메모리가 부족하면 프로그램이 그냥 종료될 수 있다. 깊이가 아주 깊다면 아래의 **스택 DFS**가 안전하다.

---

## 자료구조 선택과 시간 복잡도

| 하고 싶은 것 | 쓸 것 | 복잡도 | 피할 것 |
|--------------|-------|--------|---------|
| 끝에서 넣고 빼기 (스택) | `list.append` / `list.pop()` | O(1) | |
| 앞에서 빼기 (큐) | `deque.popleft()` | O(1) | `list.pop(0)` → O(n) |
| 존재 확인 | `x in set` / `x in dict` | 평균 O(1) | `x in list` → O(n) |
| 개수 세기 | `Counter`, `dict` | O(n) 한 번 | 매번 `list.count(x)` |
| 최솟값 반복 꺼내기 | `heapq` | O(log n) | 매번 `min()` → O(n) |
| 정렬된 리스트에서 위치 찾기 | `bisect` | O(log n) | 처음부터 훑기 |

> 💡 자료구조별 연산은 [04. 리스트 · 튜플](04.python_list_tuple.md)의 시간 복잡도 표와 [10. 모듈 · 표준 라이브러리](10.python_modules.md)에 자세히 있다. 파이썬은 단순 연산 기준으로 1초에 대략 천만 번 안팎을 처리한다고 보고, 입력 크기로 필요한 복잡도를 먼저 가늠하자.

---

## 격자 탐색 — 방향 배열

> `(r, c)` 칸에서 상하좌우로 이동할 때 방향마다 `if`를 쓰지 않고 **변화량 배열**로 묶는다.

```python
dr = [-1, 1, 0, 0]     # 상, 하, 좌, 우
dc = [0, 0, -1, 1]

for d in range(4):
    nr, nc = r + dr[d], c + dc[d]
    if not (0 <= nr < n and 0 <= nc < m):   # 격자 밖이면 건너뜀 (비교 연결)
        continue
    # (nr, nc) 처리
```

> ⚠️ 경계 검사를 빠뜨리면 `IndexError`가 나거나, **음수 인덱스가 반대편 끝을 가리켜서** 에러 없이 틀린 답이 나온다(`grid[-1]`은 마지막 행). 파이썬에서 특히 찾기 어려운 버그다.

---

## DFS — 깊이 우선 탐색

**재귀 버전** — 연결된 영역의 크기 세기

```python
import sys
sys.setrecursionlimit(10 ** 6)

def dfs(r, c):
    visited[r][c] = True
    size = 1
    for d in range(4):
        nr, nc = r + dr[d], c + dc[d]
        if 0 <= nr < n and 0 <= nc < m and not visited[nr][nc] and grid[nr][nc] == 1:
            size += dfs(nr, nc)
    return size

visited = [[False] * m for _ in range(n)]   # ⚠️ [[False] * m] * n 은 안 됨 (06장)
```

**스택 버전** — 재귀 한도 걱정이 없다

```python
def dfs_stack(start, graph):
    visited = {start}
    stack = [start]
    while stack:
        node = stack.pop()
        for nxt in graph[node]:
            if nxt not in visited:
                visited.add(nxt)
                stack.append(nxt)
    return visited
```

---

## BFS — 너비 우선 탐색 · 최단 거리

> 가까운 곳부터 차례로 퍼져 나가므로, **간선 가중치가 모두 같을 때 최단 거리**를 구한다. 큐는 반드시 `deque`로.

```python
from collections import deque

def bfs(start, graph):
    dist = {start: 0}
    q = deque([start])
    while q:
        node = q.popleft()
        for nxt in graph[node]:
            if nxt not in dist:              # 처음 방문할 때 거리 확정
                dist[nxt] = dist[node] + 1
                q.append(nxt)
    return dist

graph = {1: [2, 3], 2: [4], 3: [4], 4: []}
bfs(1, graph)        # {1: 0, 2: 1, 3: 1, 4: 2}
```

> ⚠️ 방문 표시는 **큐에 넣을 때** 하자. 꺼낼 때 표시하면 같은 칸이 큐에 여러 번 들어가 느려진다.

---

## 다익스트라 — 가중치 최단 거리

> 간선마다 비용이 다를 때의 최단 거리. `heapq`에 `(거리, 정점)`을 넣고 가장 가까운 정점부터 꺼낸다.

```python
import heapq

def dijkstra(start, graph, n):        # graph[u] = [(v, w), ...]
    INF = float("inf")
    dist = [INF] * n
    dist[start] = 0
    h = [(0, start)]
    while h:
        d, u = heapq.heappop(h)
        if d > dist[u]:               # 이미 더 짧은 거리로 처리된 낡은 항목
            continue
        for v, w in graph[u]:
            if d + w < dist[v]:
                dist[v] = d + w
                heapq.heappush(h, (dist[v], v))
    return dist

graph = [[(1, 4), (2, 1)], [(3, 1)], [(1, 2), (3, 5)], []]
dijkstra(0, graph, 4)                 # [0, 3, 1, 4]
```

---

## 이분 탐색 응용

**정렬된 리스트에서 개수 세기** — `bisect` ([10. 모듈 · 표준 라이브러리](10.python_modules.md))

```python
from bisect import bisect_left, bisect_right
a = [1, 2, 2, 3, 5]
bisect_right(a, 2) - bisect_left(a, 2)    # 2  (값 2의 개수)
bisect_right(a, 3) - bisect_left(a, 2)    # 3  (2 이상 3 이하의 개수)
```

**파라메트릭 서치** — "조건을 만족하는 최소 x"를 답의 범위에서 이분 탐색

```python
def ok(x):                    # x가 커질수록 False ... False True ... True
    return x * x >= 1000

lo, hi = 0, 10 ** 9           # 답이 [lo, hi] 안에 있다고 가정
while lo < hi:
    mid = (lo + hi) // 2
    if ok(mid):
        hi = mid              # mid도 답 후보 → 왼쪽 절반(mid 포함)
    else:
        lo = mid + 1
print(lo)                     # 32
```

> 💡 파이썬 `int`는 오버플로가 없어서 C++처럼 `lo + (hi - lo) // 2`로 쓸 필요가 없다.

---

## 완전탐색 — itertools

> 순열·조합·중복 순열은 직접 재귀를 짜지 않고 `itertools`로 만든다. ([10. 모듈 · 표준 라이브러리](10.python_modules.md))

```python
from itertools import permutations, combinations, product

best = max(sum(c) for c in combinations([3, 1, 4, 1, 5], 3))   # 3개를 골라 만든 최대 합 → 12
```

> ⚠️ 경우의 수부터 계산하자. 10개의 순열은 약 360만 가지, 12개면 약 4억 8천만 가지라 시간 안에 못 끝난다.

---

## 자주 하는 실수 체크리스트

| 증상 | 흔한 원인 | 관련 내용 |
|------|-----------|-----------|
| 2차원 리스트의 한 칸을 바꿨는데 열 전체가 바뀜 | `[[0] * m] * n` → `[[0] * m for _ in range(n)]` | [06. 참조 · 복사](06.python_reference_copy.md) |
| 시간 초과 | `input()` 그대로 사용, `list.pop(0)`, `x in list` 반복 | 이 문서 |
| `RecursionError` | 재귀 깊이 1000 초과 | 이 문서 · [09. 스코프 · 재귀 · 람다](09.python_functions_advanced.md) |
| 문자열 비교가 항상 틀림 | `sys.stdin.readline` 결과에 `\n`이 붙어 있음 → `.rstrip()` | 이 문서 |
| 음수 나눗셈 결과가 C++과 다름 | `-7 // 2 == -4`, `-7 % 2 == 1` | [02. 연산자](02.python_operators.md) |
| 반올림 결과가 이상함 | `round(2.5) == 2` (짝수 쪽 반올림) | [02. 연산자](02.python_operators.md) |
| 함수를 여러 번 호출하면 결과가 누적됨 | 기본값이 `[]`인 매개변수 | [08. 함수](08.python_functions.md) |
| 정렬했는데 결과가 `None` | `x = lst.sort()` → `lst.sort()` 또는 `x = sorted(lst)` | [04. 리스트 · 튜플](04.python_list_tuple.md) |
| 최대 힙이 필요함 | `heapq`는 최소 힙뿐 → 부호를 뒤집어 넣기 | [10. 모듈 · 표준 라이브러리](10.python_modules.md) |

---

[← 14. 파일 입출력](14.python_file_io.md) · [📚 목차](README.md)
