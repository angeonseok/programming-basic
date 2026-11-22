# 🔷 부록 B. 알고리즘 구현 패턴

> 코딩테스트에 반복해서 나오는 **구현 뼈대**를 모았다. 각 패턴에 쓰인 문법은 본문에서 다룬다: 재귀([04. 함수](04.cpp_functions.md)), `queue`·`priority_queue`([11. 순차 컨테이너](11.cpp_sequence_containers.md)), `sort`·이분 탐색([13. STL 알고리즘](13.cpp_algorithm.md)). 입출력 세팅과 자주 하는 실수는 [부록 A. 코딩테스트 준비](A.cpp_coding_test.md)에 있다.

---

## 격자 탐색 — 2차원 순회 · 방향 배열

### 2차원 격자 순회

> `N×M` 격자를 **행(row) → 열(col)** 순으로 훑는 기본 틀. 지도 탐색·시뮬레이션의 출발점이다.

```cpp
int N, M;
cin >> N >> M;
vector<vector<int>> grid(N, vector<int>(M));

for (int r = 0; r < N; r++) {
    for (int c = 0; c < M; c++) {
        cin >> grid[r][c];       // 격자 입력
    }
}
```

### ⚠️ 방향 배열 (dx/dy) — 상하좌우 이동

> 현재 칸 `(r, c)`에서 **인접한 네 칸**으로 이동하는 관용구. BFS/DFS/시뮬레이션에서 거의 항상 쓴다. 방향마다 `if`를 네 번 쓰는 대신 배열로 묶는다.

```cpp
int dr[4] = {-1, 1, 0, 0};   // 상, 하, 좌, 우의 행 변화
int dc[4] = {0, 0, -1, 1};   // 상, 하, 좌, 우의 열 변화

for (int dir = 0; dir < 4; dir++) {
    int nr = r + dr[dir];    // 다음 행
    int nc = c + dc[dir];    // 다음 열

    // 격자 밖으로 나가는지 반드시 검사!
    if (nr < 0 || nr >= N || nc < 0 || nc >= M) continue;

    // nr, nc 칸 처리...
}
```

| 방향 | dr | dc |
|------|----|----|
| 상(↑) | -1 | 0 |
| 하(↓) | +1 | 0 |
| 좌(←) | 0 | -1 |
| 우(→) | 0 | +1 |

> 💡 대각선까지 8방향이면 배열을 8칸으로 늘린다.
> ```cpp
> int dr[8] = {-1,-1,-1, 0, 0, 1, 1, 1};
> int dc[8] = {-1, 0, 1,-1, 1,-1, 0, 1};
> ```

> ⚠️ **경계 검사(`nr`, `nc`가 격자 안인지)를 빠뜨리면** 배열 범위를 벗어나 런타임 에러(또는 쓰레기 값)가 난다. 방향 이동 뒤엔 항상 범위 체크가 세트다.

---

## DFS · 백트래킹

### DFS 뼈대 (격자 탐색)

> 방향 배열(위의 격자 탐색 참고)과 재귀를 합치면 격자 DFS가 된다. **방문 체크 + 경계 검사**가 핵심이다.

```cpp
int N, M;
vector<vector<int>> grid;      // 지도 (전역)
vector<vector<bool>> visited;  // 방문 여부 (전역)
int dr[4] = {-1, 1, 0, 0};
int dc[4] = {0, 0, -1, 1};

void dfs(int r, int c) {
    visited[r][c] = true;               // 현재 칸 방문 처리
    for (int dir = 0; dir < 4; dir++) {
        int nr = r + dr[dir], nc = c + dc[dir];
        if (nr < 0 || nr >= N || nc < 0 || nc >= M) continue;  // 경계
        if (visited[nr][nc]) continue;                          // 이미 방문
        if (grid[nr][nc] == 0) continue;                        // 벽 등 조건
        dfs(nr, nc);                    // 다음 칸으로 재귀
    }
}
```

> 💡 **전역으로 두는 이유:** `grid`·`visited`를 매 재귀 호출마다 값으로 넘기면 복사 폭발이 난다. 전역 변수(또는 `&` 전달)로 공유하는 게 관례다.

전역 `vector`는 빈 상태로 선언해두고, **크기를 입력받은 뒤 `main`에서 잡는다.**

```cpp
int main() {
    cin >> N >> M;
    grid.assign(N, vector<int>(M));
    visited.assign(N, vector<bool>(M, false));
    for (auto& row : grid)
        for (auto& x : row) cin >> x;

    int count = 0;                       // 연결된 덩어리 개수
    for (int r = 0; r < N; r++)
        for (int c = 0; c < M; c++)
            if (grid[r][c] == 1 && !visited[r][c]) {
                dfs(r, c);
                count++;
            }
    cout << count << "\n";
}
```

### 백트래킹 뼈대

> 모든 경우를 탐색하되, 선택했다가 **되돌리는(원복)** 패턴. 순열·조합·N-Queen 등에 쓴다.

```cpp
int n, k;              // n개 중 k개를 순서 있게 고르기
vector<int> pick;      // 현재까지 고른 것
vector<bool> used;     // 크기는 main에서: used.assign(n, false);

void backtrack(int depth) {
    if (depth == k) {              // 다 골랐으면 결과 처리
        // pick 사용
        return;
    }
    for (int i = 0; i < n; i++) {
        if (used[i]) continue;
        used[i] = true;            // 선택
        pick.push_back(i);
        backtrack(depth + 1);
        pick.pop_back();           // 되돌리기 (핵심!)
        used[i] = false;
    }
}
```

> ⚠️ 선택 후 반드시 **원복(`pop_back`, `used[i] = false`)**해야 다음 경우를 올바르게 탐색한다. 이 되돌리기를 빠뜨리는 게 백트래킹 최다 실수다.

> ⚠️ **전역에서 `vector<bool> used(n);`처럼 크기를 주면 안 된다.** 전역 변수는 `main`보다 먼저 만들어지므로, 그 시점의 `n`은 아직 입력 전인 `0`이다 → `used`의 크기가 **0**이 되고 `used[i]` 접근이 범위 초과가 된다. 전역 `vector`는 빈 채로 선언하고, `n`을 입력받은 뒤 `main`에서 `assign`으로 크기를 잡아라.

### 전역 배열 활용

> 큰 배열은 함수 안(스택)이 아니라 **전역(정적 영역)**에 선언하는 게 흔하다. 스택 오버플로를 피하고, 자동으로 0 초기화된다.

```cpp
int arr[1'000'001];        // 전역: 자동으로 전부 0
bool visited[1'000'001];   // 전역: 자동으로 전부 false

int main() { /* ... */ }
```

> ⚠️ **큰 배열을 지역(함수 안)에 두면** 스택 한도(보통 1~8MB)를 넘겨 죽을 수 있다. `int arr[1000000]`을 `main` 안에 두면 위험 → 전역으로 빼거나 `vector`를 써라.

---

## BFS 뼈대 (queue 활용)

> 격자 최단거리의 표준 틀. `queue`에 좌표를 넣고, 꺼내며 인접 칸을 탐색한다.

```cpp
int dr[4] = {-1,1,0,0}, dc[4] = {0,0,-1,1};
vector<vector<int>> dist(N, vector<int>(M, -1));   // -1 = 미방문
queue<pair<int,int>> q;

q.push({sr, sc});
dist[sr][sc] = 0;
while (!q.empty()) {
    auto [r, c] = q.front(); q.pop();              // 구조적 바인딩 (C++17)
    for (int d = 0; d < 4; d++) {
        int nr = r + dr[d], nc = c + dc[d];
        if (nr < 0 || nr >= N || nc < 0 || nc >= M) continue;
        if (dist[nr][nc] != -1) continue;          // 이미 방문
        dist[nr][nc] = dist[r][c] + 1;             // 거리 갱신
        q.push({nr, nc});
    }
}
```

> ⚠️ `auto [r, c] = q.front();`는 **복사**라서 바로 `pop()`해도 안전하다. `auto& [r, c] = q.front();`처럼 참조로 받은 뒤 `pop()`하면 지워진 원소를 가리키게 된다.

---

## pair를 담은 우선순위 큐 (다익스트라)

> `pair`를 담으면 **first 기준**으로 정렬된다. 다익스트라는 `{거리, 정점}`을 최소 힙에 넣어, 거리가 가장 짧은 정점을 매번 꺼낸다.

```cpp
const int INF = 1e9;
vector<vector<pair<int,int>>> adj(n);   // adj[u] = {(v, w), ...}  u → v, 가중치 w
vector<int> dist(n, INF);

// {거리, 정점} 최소 힙 → 거리가 가장 짧은 것이 top
priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;

dist[start] = 0;
pq.push({0, start});
while (!pq.empty()) {
    auto [d, u] = pq.top();      // {거리, 정점} (C++17 구조적 바인딩, 복사)
    pq.pop();
    if (d > dist[u]) continue;   // 이미 더 짧은 거리로 처리된 낡은 항목 → 버림
    for (auto [v, w] : adj[u]) {
        if (dist[u] + w < dist[v]) {   // 완화(relax): u를 거쳐 가는 게 더 짧으면 갱신
            dist[v] = dist[u] + w;
            pq.push({dist[v], v});
        }
    }
}
// dist[v] == INF 이면 start에서 v로 갈 수 없음
```

> ⚠️ `if (d > dist[u]) continue;`를 빠뜨리면 답은 맞아도 같은 정점을 여러 번 처리해 **시간 초과**가 날 수 있다. 거리 합이 21억을 넘을 수 있으면 `dist`와 `pq`를 `long long`으로 바꿔라.

> 💡 거리를 **음수로 넣어 최대 힙을 최소 힙처럼** 쓰는 편법도 있지만(`pq.push({-dist, v})`), `greater`를 쓰는 정석이 실수가 적다.

---

## 직접 구현하는 이분 탐색 (파라메트릭 서치)

> "조건을 만족하는 **최소(또는 최대) 값**"을 찾는 문제는 배열이 아니라 **답의 범위**를 이분 탐색한다. `lower_bound`로는 안 되고 직접 짠다. 조건 `ok(x)`가 x가 커질수록 `F F F T T T`처럼 한 번만 바뀔 때 쓸 수 있다.

```cpp
// ok(x)가 참인 최소 x 찾기 (답이 [lo, hi] 안에 반드시 있다고 가정)
long long lo = 0, hi = 2'000'000'000;
while (lo < hi) {
    long long mid = lo + (hi - lo) / 2;   // (lo + hi) / 2 의 오버플로 방지 버전
    if (ok(mid)) hi = mid;                 // mid도 답 후보 → 왼쪽 절반(mid 포함)
    else lo = mid + 1;                     // mid는 답 아님 → 오른쪽 절반
}
// 반복이 끝나면 lo == hi == 답
```

> 💡 예: `ok(x) = (x * x >= 1000)`이면 답은 `32`. "최대 x"를 찾을 땐 `mid = lo + (hi - lo + 1) / 2`로 올림하고 `ok(mid)`면 `lo = mid`, 아니면 `hi = mid - 1`로 바꾼다. 올림을 안 하면 무한 루프에 빠진다.

---

## 누적합 (prefix sum)

> "구간 `[l, r]`의 합"을 여러 번 물어보는 문제의 기본 도구. 미리 O(n)으로 누적합을 만들어두면 **각 질문을 O(1)**에 답한다.

```cpp
vector<int> a = {3, 1, 4, 1, 5};
int n = a.size();

vector<long long> pre(n + 1, 0);           // pre[i] = a[0] + ... + a[i-1]  (pre[0] = 0)
for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + a[i];
// pre = {0, 3, 4, 8, 9, 14}

// 구간 [l, r] 합 (0-based, 양 끝 포함)
int l = 1, r = 3;
long long rangeSum = pre[r + 1] - pre[l];  // 1 + 4 + 1 = 6
```

> 💡 앞에 `0`을 하나 둔 **크기 `n + 1`** 배열로 만들면 `l = 0`일 때도 `pre[l - 1]` 같은 예외 처리가 필요 없다.

`<numeric>`의 `partial_sum`으로도 만들 수 있다.

```cpp
vector<long long> ps(n);
partial_sum(a.begin(), a.end(), ps.begin());   // {3, 4, 8, 9, 14}
```

> ⚠️ `partial_sum`은 **입력 원소의 타입(`int`)으로 누적**한다. 출력을 `long long`으로 받아도 중간 합이 21억을 넘으면 이미 오버플로한 값이 들어간다. 합이 클 수 있으면 위의 반복문 방식을 쓰거나 입력 자체를 `vector<long long>`으로 두어라.

---

## 좌표 압축

> 값의 **크기 순서**만 중요하고 실제 값이 매우 클 때(예: 10⁹ 범위), 값을 **0, 1, 2, …의 순위로 바꿔** 배열 인덱스로 쓸 수 있게 만든다. `sort + unique + erase`와 `lower_bound`의 조합이다.

```cpp
vector<int> a = {100, 5, 100, 70000};

vector<int> xs = a;
sort(xs.begin(), xs.end());
xs.erase(unique(xs.begin(), xs.end()), xs.end());        // 정렬된 고유값 {5, 100, 70000}

for (int& v : a)
    v = lower_bound(xs.begin(), xs.end(), v) - xs.begin();  // 각 값을 순위로 바꾼다
// a = {1, 0, 1, 2}
```

---

[← 부록 A. 코딩테스트 준비](A.cpp_coding_test.md) · [📚 목차](README.md)
