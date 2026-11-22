# C++ Basic

이 디렉토리는 **C++17 기준**으로 정리한 C++ 문법 노트입니다.

**실제 코딩테스트 활용**에 초점을 맞춰, 자주 쓰이는 문법과 STL을 특히 자세히 정리했습니다. 빠른 입출력, 오버플로 회피, 정렬 비교 기준, 이분 탐색, 우선순위 큐, 격자 탐색처럼 실전에서 매번 나오는 패턴을 중심으로 구성합니다. 포인터·메모리·프로그램 구조처럼 C++을 제대로 이해하는 데 필요한 핵심 개념도 함께 다룹니다.

각 문서는 **C++17 기준**에서 개념을 빠르게 확인하고 예제 코드로 복습할 수 있도록 구성합니다. 구조적 바인딩, 초기화문이 있는 `if`, CTAD, `std::gcd`·`std::lcm`·`std::clamp`처럼 코딩테스트와 실무에서 바로 쓰이는 C++17 문법을 포함하며, 해당 부분에는 **(C++17)** 표시를 붙였습니다.

> ⚠️ 채점 환경이 C++14 이하만 지원하면 **(C++17)** 표시가 붙은 문법은 컴파일되지 않습니다. 제출 전에 언어 버전 설정(`-std=c++17`)을 확인하세요.

## 문서 구성

번호 순서대로 읽도록 구성했습니다. 각 문서 맨 아래의 `← 이전 · 목차 · 다음 →` 링크로 이어서 읽을 수 있습니다.

| # | 문서 | 핵심 내용 |
|---|------|-----------|
| 01 | [기초 · 입출력](./01.cpp_basics_io.md) | **코테 기본 템플릿**, 빠른 입출력, cin/cout, 실수 출력 형식, 형변환 |
| 02 | [자료형 · 연산자](./02.cpp_types_operators.md) | 자료형, **오버플로(long long)**, `auto`, `enum class`, 상수·`constexpr`, 연산자 우선순위, 비트 연산, `<cmath>` 함정 |
| 03 | [제어문](./03.cpp_control_flow.md) | 조건문, 초기화문 `if`, 반복문, 범위 기반 for, **격자 순회·방향 배열(dx/dy)** |
| 04 | [함수](./04.cpp_functions.md) | 참조 전달(`const&`), **재귀·DFS·백트래킹**, 람다·**재귀 람다**, 여러 값 반환, `optional` |
| 05 | [문자열](./05.cpp_string.md) | `string`, **문자 산술**, getline·버퍼, **stringstream 파싱**, `string_view`, `from_chars` |
| 06 | [컨테이너](./06.cpp_containers.md) | **vector**, pair·tuple, **map·set**, **구조적 바인딩**, `insert` 결과 받기 |
| 07 | [컨테이너 어댑터](./07.cpp_container_adapters.md) | stack·queue·deque·**BFS**, **priority_queue·다익스트라**, 반복자 무효화 |
| 08 | [알고리즘](./08.cpp_algorithm.md) | **sort·비교 기준**, **이분 탐색·파라메트릭 서치**, 순열, **누적합**, erase-remove, `gcd`·`lcm`·`clamp` |
| 09 | [포인터](./09.cpp_pointer.md) | 메모리와 주소, **포인터 기초**, 배열·문자열 리터럴과 포인터, 반복자, 연결 리스트·트리 |
| 10 | [참조 · 메모리](./10.cpp_memory.md) | 참조, lvalue/rvalue, **메모리 영역·수명**, 메모리 버그, 스마트 포인터, RAII, 이동 |
| 11 | [객체 지향](./11.cpp_oop.md) | **struct·정렬 기준(operator<)**, 클래스, 상속, 다형성 |
| 12 | [템플릿](./12.cpp_template.md) | 함수·클래스 템플릿, CTAD, `if constexpr`, fold expression |
| 13 | [예외 처리](./13.cpp_exception.md) | try/catch/throw, **런타임 에러(RE) 원인**, 디버깅용 컴파일 옵션 |
| 14 | [프로그램 구조](./14.cpp_program_structure.md) | 전처리·컴파일·링크, 매크로·**로컬 디버그 출력**, 헤더/소스 분리, 네임스페이스·**이름 충돌** |

## 코딩테스트 빠른 참고

- **가장 자주 보는 문서:** [05 문자열](./05.cpp_string.md) · [06 컨테이너](./06.cpp_containers.md) · [07 컨테이너 어댑터](./07.cpp_container_adapters.md) · [08 알고리즘](./08.cpp_algorithm.md)
- **매 문제 세팅:** 빠른 입출력([01](./01.cpp_basics_io.md)), `long long` 오버플로 판단([02](./02.cpp_types_operators.md))
- **탐색 문제:** 방향 배열([03](./03.cpp_control_flow.md)) · DFS/백트래킹([04](./04.cpp_functions.md)) · BFS([07](./07.cpp_container_adapters.md))
- **정렬/탐색:** sort 비교 람다·lower_bound 이분 탐색([08](./08.cpp_algorithm.md)) · struct operator<([11](./11.cpp_oop.md))
- **최단 거리:** BFS·다익스트라([07](./07.cpp_container_adapters.md))
- **로컬에선 맞는데 RE/WA:** RE 원인 목록과 디버깅 옵션([13](./13.cpp_exception.md)) · 오버플로([02](./02.cpp_types_operators.md)) · 전역 `vector` 크기([04](./04.cpp_functions.md))
- **알 수 없는 컴파일 에러:** `count`·`left` 같은 전역 변수 이름 충돌([14](./14.cpp_program_structure.md))
