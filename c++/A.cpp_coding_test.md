# 🔷 부록 A. 코딩테스트 준비

> 본문(01~16장)에서 배운 문법을 **코딩테스트(알고리즘 문제 풀이)**에 맞게 쓰는 법을 모았다. 매 문제를 시작할 때의 기본 세팅, 시간 초과를 막는 빠른 입출력, 런타임 에러의 흔한 원인, 자주 하는 실수 목록이다. DFS·BFS 같은 구현 패턴은 [부록 B. 알고리즘 구현 패턴](B.cpp_algorithm_patterns.md)에 있다.

---

## 매 문제 기본 세팅

> 💡 코테 채점 서버는 보통 `bits/stdc++.h` 하나로 모든 표준 헤더를 한 번에 포함할 수 있다. 헤더를 일일이 안 써도 돼서 편하다.
> ```cpp
> #include <bits/stdc++.h>   // 모든 표준 헤더 포함 (GCC 전용)
> using namespace std;
> ```
> ⚠️ 단 컴파일이 느려지고 표준이 아니라서 일부 환경(MSVC 등)에선 안 된다. 개별 헤더(`<vector>` 등)를 쓰는 습관도 알아두자.

### 기본 템플릿

> 아래 내용(빠른 입출력, `long long` 등)을 합친 **매 문제 시작 틀**이다. 각 줄의 이유는 이 문서와 [02. 자료형과 연산자](02.cpp_types_operators.md)에서 하나씩 설명한다.

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n;
    vector<long long> a(n);
    for (auto& x : a) cin >> x;

    long long sum = 0;
    for (long long x : a) sum += x;
    cout << sum << "\n";
    return 0;
}
```

---

## 빠른 입출력

> C++의 `cin`/`cout`은 기본 설정에서 C의 `scanf`/`printf`와 동기화되어 **느리다.** 입력이 수십만 줄 이상이면 이것만으로 시간 초과가 날 수 있다.

```cpp
int main() {
    ios_base::sync_with_stdio(false);   // C 입출력과 동기화 해제
    cin.tie(nullptr);                   // cin↔cout 묶음 해제

    // 이후 cin/cout이 훨씬 빨라진다
}
```

| 설정 | 효과 |
|------|------|
| `sync_with_stdio(false)` | C stdio와 분리 → cin/cout 가속 |
| `cin.tie(nullptr)` | 입력 전 자동 flush 제거 → 추가 가속 |

> 💡 옛 코드에선 `cin.tie(NULL)`이나 `cin.tie(0)`도 자주 보인다. 같은 의미다. C++11 이후로는 `nullptr`가 정석이다([06. 포인터](06.cpp_pointer.md) 참고).

> ⚠️ `sync_with_stdio(false)`를 켜면 **`cin`/`cout`과 `scanf`/`printf`를 섞어 쓰면 안 된다.** 출력 순서가 꼬인다. C++ 스트림만 쓰거나 C 함수만 쓰거나 하나로 통일하라.

> 💡 **출력엔 `endl` 대신 `"\n"`**을 써라. `endl`은 매번 버퍼를 비워(flush) 느리다. 위 가속 설정을 하고도 `endl`을 남발하면 효과가 반감된다.

```cpp
cout << ans << endl;   // ❌ 매번 flush → 느림
cout << ans << "\n";   // ✅ 빠름
```

---

## 런타임 에러(RE)가 날 때

> ⚠️ **코테에서 "런타임 에러(RE)" 판정의 흔한 원인:**
> - 배열/벡터 **범위 초과** 접근 (`v[i]`에서 `i`가 범위 밖)
> - **0으로 나누기** (`a / 0`, `a % 0`)
> - **스택 오버플로** (너무 깊은 재귀)
> - 빈 컨테이너에 `top()`/`front()`/`pop()`
> - 널 포인터 역참조
> - **잡지 않은 예외** (`stoi("abc")`, `v.at(100)` 등에서 던진 예외를 `catch`하지 않으면 프로그램이 종료된다)
>
> 반대로 **정수 오버플로는 RE가 아니다.** 프로그램은 멀쩡히 끝나고 틀린 답(WA)만 낸다. 그래서 더 찾기 어렵다([02. 자료형과 연산자](02.cpp_types_operators.md) 참고).

> 💡 어느 줄에서 터졌는지 찾으려면 [15. 예외 처리](15.cpp_exception.md)의 디버깅용 컴파일 옵션(`-D_GLIBCXX_DEBUG`, `-fsanitize`)으로 로컬에서 돌려 보자.

---

## 자주 하는 실수 체크리스트

| 증상 | 흔한 원인 | 관련 내용 |
|------|-----------|-----------|
| 답이 음수이거나 이상한 값 (WA) | `int` 오버플로 → `long long` 필요, `int * int` 중간 결과 오버플로 | [02. 자료형과 연산자](02.cpp_types_operators.md) |
| 합계가 틀림 | `accumulate(..., 0)`의 초기값 → `0LL` | [13. STL 알고리즘](13.cpp_algorithm.md) |
| `getline`이 빈 줄을 읽음 | `cin >>` 뒤에 남은 줄바꿈 → `cin.ignore()` | [14. 입출력 스트림](14.cpp_stream_io.md) |
| 시간 초과 (TLE) | 빠른 입출력 미설정, `endl` 남발, 큰 `vector`를 값으로 전달 | 이 문서 · [04. 함수](04.cpp_functions.md) |
| `set`에서 이분 탐색이 느림 | 전역 `lower_bound(s.begin(), ...)` → `s.lower_bound(x)` | [13. STL 알고리즘](13.cpp_algorithm.md) |
| 정렬 중 런타임 에러 | 비교 함수에 `<=` 사용 (strict weak ordering 위반) | [13. STL 알고리즘](13.cpp_algorithm.md) |
| 전역 `vector`에서 범위 초과 | 전역에서 `vector<int> v(n);` (그 시점의 `n`은 0) | [부록 B. 알고리즘 구현 패턴](B.cpp_algorithm_patterns.md) |
| 큰 지역 배열에서 바로 종료 | 스택 오버플로 → 전역 배열이나 `vector` | [07. 참조와 메모리 관리](07.cpp_memory.md) |
| 알 수 없는 `ambiguous` 컴파일 에러 | 전역 변수 이름이 `count`, `left` 등 std 이름과 충돌 | [16. 프로그램 구조](16.cpp_program_structure.md) |
| 내 PC와 채점 결과가 다름 | `long`이 Windows에서 4바이트, 채점 환경이 C++17 미지원 | [02. 자료형과 연산자](02.cpp_types_operators.md) |

> 💡 제출 코드에 디버그 출력이 남지 않게 하려면 `#ifdef LOCAL` 디버그 매크로를 쓴다([16. 프로그램 구조](16.cpp_program_structure.md)).

---

[← 16. 프로그램 구조](16.cpp_program_structure.md) · [📚 목차](README.md) · [부록 B. 알고리즘 구현 패턴 →](B.cpp_algorithm_patterns.md)
