# Python Basic

이 디렉토리는 **Python 3.10 이상 기준**으로 정리한 Python 기초 문법 노트입니다.

기본 문법부터 자료구조, 함수, 모듈, 객체 지향, 예외 처리까지 Python 학습 초반에 필요한 내용을 다루며, 알고리즘 문제 풀이에 바로 쓰이는 빠른 입력·`deque`·`heapq`·`bisect` 같은 도구도 함께 정리합니다.

각 문서는 개념을 빠르게 확인하고 예제 코드로 복습할 수 있도록 구성합니다. 특정 버전부터 쓸 수 있는 문법에는 **(3.8+)**, **(3.10+)**처럼 표시를 붙였습니다.

> ⚠️ 채점 환경의 Python 버전이 낮으면 버전 표시가 붙은 문법(`match`, `:=`, `int | None` 등)은 동작하지 않습니다. 제출 전에 버전을 확인하세요.

## 문서 구성

번호 순서대로 읽도록 구성했습니다. 각 문서 맨 아래의 `← 이전 · 목차 · 다음 →` 링크로 이어서 읽을 수 있습니다.

| # | 문서 | 핵심 내용 |
|---|------|-----------|
| 01 | [기초 · 입출력](./01.python_basics.md) | 들여쓰기·변수·동적 타이핑, `print`/`input`, **빠른 입력**, 숫자형(int 무한·float 오차), 형변환, PEP 8 |
| 02 | [연산자](./02.python_operators.md) | 산술(**음수 `//`·`%`, `round` 함정**), 비교 연결, 논리 연산 반환값, **Truthy/Falsy**, **`is` vs `==`**, 비트 연산 |
| 03 | [문자열](./03.python_string.md) | 인덱싱·**슬라이싱**, 불변성, **f-string 포맷**, 문자열 메서드, **split·join 함정** |
| 04 | [리스트 · 튜플](./04.python_list_tuple.md) | 리스트 메서드, **정렬(key·다중 기준)**, 순회 중 삭제 함정, **시간 복잡도**, 컴프리헨션, 튜플, range |
| 05 | [딕셔너리 · 세트](./05.python_dict_set.md) | dict 메서드·`get`, value 기준 정렬, set 연산, **해시 테이블**, `defaultdict` |
| 06 | [참조 · 복사](./06.python_reference_copy.md) | **변수는 이름표**, 가변·불변, 얕은·깊은 복사, **2차원 리스트 함정**, 함수 인자 전달 방식 |
| 07 | [제어문](./07.python_control_flow.md) | if·삼항, **`match`(3.10+)**, for·while, for-else, `enumerate`·`zip`, `:=` |
| 08 | [함수](./08.python_functions.md) | 반환값, 인자 종류, **가변 기본값 함정**, 키워드 전용 인자, 패킹·언패킹, **타입 힌트** |
| 09 | [스코프 · 재귀 · 람다](./09.python_functions_advanced.md) | LEGB·`global`·`nonlocal`·클로저, **재귀 한도**, 람다, 내장 함수, **이터레이터·제너레이터** |
| 10 | [모듈 · 표준 라이브러리](./10.python_modules.md) | import, **`__name__ == "__main__"`**, 패키지, pip·venv, **Counter·deque·heapq·bisect·itertools·lru_cache** |
| 11 | [클래스](./11.python_class.md) | 클래스·인스턴스, 클래스 변수 vs 인스턴스 변수, 인스턴스·클래스·스태틱 메서드 |
| 12 | [객체 지향 심화](./12.python_oop_advanced.md) | 매직 메서드(`__repr__`·`__lt__`), 데코레이터, 캡슐화·`@property`, 상속·`super`·MRO, ABC, **dataclass** |
| 13 | [예외 · 파일](./13.python_exception_file.md) | 예외 종류·계층, try/except/else/finally, `raise`·사용자 정의 예외, **`with`·파일 입출력(`encoding`)** |

## 코딩테스트 빠른 참고

- **매 문제 세팅:** 빠른 입력 `sys.stdin.readline`([01](./01.python_basics.md)) · 재귀 한도 `sys.setrecursionlimit`([09](./09.python_functions_advanced.md))
- **자료구조 선택:** 존재 확인은 `set`([05](./05.python_dict_set.md)) · 큐/BFS는 `deque` · 우선순위 큐는 `heapq`([10](./10.python_modules.md))
- **정렬·탐색:** 다중 기준 정렬([04](./04.python_list_tuple.md)) · 이분 탐색 `bisect`([10](./10.python_modules.md))
- **완전탐색:** `permutations`·`combinations`·`product`([10](./10.python_modules.md))
- **자주 틀리는 것:** 2차원 리스트 `[[0]*m]*n`([06](./06.python_reference_copy.md)) · 음수 `//`·`%`와 `round`([02](./02.python_operators.md)) · `pop(0)`이 O(n)([04](./04.python_list_tuple.md))
