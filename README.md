# Programming Language

summary programming languages

## Index
- [C](#c)
- [C++:23](#c23)
- [Java](#java)
- [Python:3.10](#python-310)
- [JavaScript:ES14](#javascript-es14)

## C


## [C++23](./02_c++/README.md)

#### C++98/03 (ISO/IEC 14882:1998, 2003)
- **표준 템플릿 라이브러리 (STL)**: 벡터, 리스트, 덱, 맵, 셋 등의 컨테이너와 알고리즘 제공
- **네임스페이스**: 코드 충돌을 피하기 위한 네임스페이스 사용 (`namespace`)
- **예외 처리**: 예외를 던지고 잡는 구조 (`try`, `catch`, `throw`)
- **함수 오버로딩 및 오퍼레이터 오버로딩**: 같은 이름의 함수나 연산자를 여러 번 정의할 수 있음
- **템플릿**: 일반화된 코드를 작성하기 위한 템플릿 제공 (`template<typename T>`)

#### C++11 (ISO/IEC 14882:2011)
- **자동 타입 추론 (auto)**: 변수를 선언할 때 타입을 자동으로 추론 (`auto`)
- **범위 기반 for 루프**: 컨테이너를 쉽게 순회 (`for(auto &x : container)`)
- **람다 표현식**: 익명 함수 작성 (`[](int x) { return x + 1; }`)
- **스마트 포인터**: `std::unique_ptr`, `std::shared_ptr` 등 안전한 메모리 관리를 위한 스마트 포인터 도입
- **nullptr**: NULL 포인터를 대체하는 새로운 키워드
- **정적 어서션 (static_assert)**: 컴파일 시간에 조건 검사를 수행
- **이동 시맨틱 및 R값 참조**: 리소스의 효율적 이동을 위한 `std::move`, `&&` 연산자 도입
- **컨스턴트 표현식 (constexpr)**: 컴파일 시간에 계산되는 상수 표현식

#### C++14 (ISO/IEC 14882:2014)
- **일반화된 람다 캡처**: 람다에서 변수 캡처를 더 자유롭게 할 수 있음 (`[=, &x]{}`)
- **decltype(auto)**: `auto`와 `decltype`의 결합으로 더 정확한 타입 추론
- **바이너리 리터럴**: 바이너리 형식의 숫자 리터럴 (`0b1010`)
- **std::make_unique**: `unique_ptr` 생성 헬퍼 함수

#### C++17 (ISO/IEC 14882:2017)
- **구조화된 바인딩**: 튜플이나 구조체의 멤버를 직접 변수에 바인딩 (`auto [x, y] = std::make_tuple(1, 2);`)
- **if constexpr**: 컴파일 타임 조건부 실행
- **std::optional**: 값이 있을 수도 없을 수도 있는 객체를 표현
- **std::variant**: 여러 타입 중 하나의 값을 가질 수 있는 객체
- **std::any**: 임의의 타입을 가질 수 있는 타입 안전 컨테이너
- **파일 시스템 라이브러리**: 파일 시스템을 다루는 표준 라이브러리 (`<filesystem>`)

#### C++20 (ISO/IEC 14882:2020)
- **코루틴**: 비동기 프로그래밍을 위한 코루틴 지원 (`co_await`, `co_yield`, `co_return`)
- **컨셉과 제약**: 템플릿의 타입 요구사항을 명시 (`concept`)
- **범위 라이브러리**: 범위를 다루는 새로운 라이브러리 (`<ranges>`)
- **module**: 모듈 시스템 도입으로 컴파일 시간 단축 및 코드 관리 개선 (`module`)
- **삼항 연산자 개선**: 삼항 연산자의 결과 타입 추론 개선
- **초기화된 if/switch**: 조건문 안에서 변수 초기화 가능 (`if (int x = f(); x < 0)`)

#### C++23 (ISO/IEC 14882:2023)
- **코루틴 개선**: 코루틴의 성능 및 사용성 향상
- **consteval**: 컴파일 타임에 평가되는 함수 정의
- **다중 패턴 매칭**: 복잡한 조건문을 간단하게 작성할 수 있는 패턴 매칭
- **라이브러리 개선**: 다양한 표준 라이브러리의 기능 확장 및 성능 개선


## Java


## [Python 3.10](./04_python/README.md)

#### Python 3.0 (2008년)
- **문법 및 라이브러리 호환성 변경**: 많은 라이브러리와 문법이 변경되었으며, Python 2와 호환되지 않음.
- **print 함수**: `print`가 함수로 변경되어 `print("Hello, World!")` 형식을 사용.
- **Integer division**: `/` 연산자가 부동 소수점 나눗셈을 수행하고, `//` 연산자가 정수 나눗셈을 수행.
- **문자열 처리**: 기본 문자열 타입이 유니코드이며, `bytes` 타입이 도입됨.
- **예외 처리**: `as` 키워드를 사용하여 `except` 구문을 개선 (`except Exception as e`).

#### Python 3.1 (2009년)
- **ordered dictionary**: `collections.OrderedDict` 클래스 도입.
- **faster I/O**: I/O 속도 개선.
- **기타 성능 개선**: 많은 내장 함수와 모듈의 성능이 개선됨.

#### Python 3.2 (2011년)
- **futures 모듈**: 동시성 작업을 위한 `concurrent.futures` 모듈 도입.
- **new syntax**: `yield from` 문법 도입.
- **보안 강화**: `hashlib` 모듈의 보안 기능 강화.

#### Python 3.3 (2012년)
- **가상 환경 내장**: `venv` 모듈 도입.
- **namespace packages**: `pkgutil` 및 `pkg_resources`를 사용하지 않고도 네임스페이스 패키지 사용 가능.
- **key-sharing dictionaries**: 메모리 사용량 감소를 위한 딕셔너리 구현 개선.

#### Python 3.4 (2014년)
- **asyncio 모듈**: 비동기 I/O를 위한 `asyncio` 모듈 도입.
- **pathlib 모듈**: 객체 지향적 파일 시스템 경로를 위한 `pathlib` 모듈 도입.
- **enum 모듈**: 열거형을 위한 `enum` 모듈 도입.
- **tracemalloc 모듈**: 메모리 할당 추적을 위한 `tracemalloc` 모듈 도입.

#### Python 3.5 (2015년)
- **async/await 구문**: 비동기 프로그래밍을 위한 `async` 및 `await` 구문 도입.
- **matrix multiplication operator**: 행렬 곱셈을 위한 `@` 연산자 도입.
- **type hints**: 함수 인자와 반환 값의 타입을 명시하기 위한 타입 힌트 도입 (PEP 484).

#### Python 3.6 (2016년)
- **f-string**: 포매팅을 위한 f-string 도입 (`f"Hello, {name}!"`).
- **underscore in numeric literals**: 숫자 리터럴 내에 밑줄 사용 가능 (`1_000_000`).
- **dict 유지 순서**: 딕셔너리가 삽입 순서를 유지하도록 변경.
- **async/await 확장**: 비동기 제너레이터와 비동기 컴프리헨션 도입.

#### Python 3.7 (2018년)
- **데이터 클래스**: `dataclasses` 모듈 도입으로 간단한 클래스 생성 가능 (`@dataclass` 데코레이터 사용).
- **asyncio 개선**: 비동기 기능의 성능 및 사용성 개선.
- **breakpoint()**: 디버깅을 쉽게 하기 위해 내장 `breakpoint()` 함수 도입.
- **모듈 수준에서 __getattr__() 및 __dir__()**: 모듈에 대한 동적 속성 접근 및 디렉토리 목록 제공.

#### Python 3.8 (2019년)
- **walrus operator**: 할당 표현식 도입 (`:=`).
- **positional-only parameters**: 위치 전용 인자 구문 추가 (`/`).
- **f-string improvements**: f-string 내에서 `=` 연산자 사용 가능.
- **typing 개선**: `TypedDict` 및 프로토콜 등의 타입 힌팅 개선.

### Python 3.9 이후 주요 변경 사항

#### Python 3.9 (2020년)
- **dictionary merge & update operators**: 딕셔너리 병합 (`|`) 및 업데이트 (`|=`) 연산자 도입.
- **type hinting generics**: 리스트와 같은 내장 컬렉션 타입에 대한 제너릭 지원 (`list[int]`).
- **string methods**: `removeprefix` 및 `removesuffix` 문자열 메소드 추가.
- **zoneinfo 모듈**: 시간대 정보 지원을 위한 `zoneinfo` 모듈 도입.

#### Python 3.10 (2021년)
- **pattern matching**: 구조적 패턴 매칭 기능 도입 (`match`/`case` 구문).
- **precise error messages**: 구문 오류 메시지의 정확도 및 가독성 향상.
- **parenthesized context managers**: 컨텍스트 관리자 구문에서 괄호 사용 가능.

#### Python 3.11 (2022년)
- **faster CPython**: 성능 최적화 및 실행 속도 향상.
- **exception groups**: 예외 그룹 및 예외 그룹 핸들링 도입.
- **fine-grained error locations**: 구문 오류에 대한 세밀한 위치 정보 제공.

#### Python 3.12 (2023년)
- **syntax enhancements**: 새로운 문법 개선 및 최적화.
- **standard library updates**: 표준 라이브러리의 다양한 업데이트 및 개선.
- **additional performance improvements**: 더 많은 성능 최적화 및 개선.


## [JavaScript ES14](./05_javascript/README.md)

#### ECMAScript 2015 (ES6)
1. **let, const**: 블록 스코프 변수 선언
2. **화살표 함수**: 간결한 함수 표현식
3. **템플릿 리터럴**: 백틱(`)을 사용한 문자열
4. **디스트럭처링 할당**: 배열, 객체의 값을 개별 변수에 할당
5. **클래스**: 객체 지향 프로그래밍 지원
6. **모듈**: import/export를 사용한 모듈화
7. **프라미스**: 비동기 처리를 위한 객체

#### ECMAScript 2016 (ES7)
1. **Array.prototype.includes**: 배열에 특정 값 포함 여부 확인
2. **지수 연산자**: 거듭제곱 연산자 (`**`)

#### ECMAScript 2017 (ES8)
1. **async/await**: 비동기 코드를 더 간결하게 작성
2. **Object.entries, Object.values**: 객체의 키-값 쌍, 값들을 배열로 반환

#### ECMAScript 2018 (ES9)
1. **Rest/Spread properties**: 객체의 나머지/확산 연산자
2. **Asynchronous Iteration**: `for-await-of` 문법

#### ECMAScript 2019 (ES10)
1. **Array.prototype.flat, Array.prototype.flatMap**: 배열 평탄화, 맵핑 후 평탄화
2. **Object.fromEntries**: 키-값 쌍 배열을 객체로 변환

#### ECMAScript 2020 (ES11)
1. **Optional Chaining (?.)**: 체이닝 중 null/undefined 여부 확인
2. **Nullish Coalescing (??)**: null 또는 undefined일 때만 대체값 사용
3. **BigInt**: 큰 정수를 다루기 위한 데이터 타입
4. **Dynamic Import**: 동적 모듈 로딩

#### ECMAScript 2021 (ES12)
1. **String.prototype.replaceAll**: 모든 일치하는 문자열 대체
2. **Logical Assignment Operators**: 논리 연산을 통한 할당 (`&&=`, `||=`, `??=`)
3. **Numeric Separators**: 숫자 가독성을 위한 구분자 (`1_000_000`)

#### ECMAScript 2022 (ES13)
1. **Top-level await**: 모듈의 최상위 수준에서 await 사용 가능
2. **Class Fields**: 클래스 필드 정의

#### ECMAScript 2023 (ES14)
1. **Array.prototype.findLast, Array.prototype.findLastIndex**: 배열의 마지막 요소 찾기
2. **Hashbang Syntax**: 스크립트 파일의 첫 줄에 해시뱅(#!) 사용 가능
3. **Array.prototype.at**: 배열의 특정 인덱스에 접근 (음수 인덱스 지원)
4. **RegExp Match Indices**: 정규 표현식 매칭 시 인덱스 반환

#### ECMAScript 2024 (예상, ES15)
1. **Temporal**: 날짜와 시간을 다루기 위한 새로운 API
2. **Record & Tuple**: 불변 데이터 구조

JavaScript는 계속해서 발전하고 있으며, 새로운 기능들이 지속적으로 추가되고 있습니다. 최신 버전의 ECMAScript 사양과 새로운 기능에 대한 자세한 정보를 확인하려면 [TC39 제안](https://github.com/tc39/proposals)을 참고하는 것이 좋습니다.


