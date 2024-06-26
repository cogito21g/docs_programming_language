#### 5.2. Catch2

Catch2는 C++에서 단위 테스트와 BDD(Behavior-Driven Development) 스타일의 테스트를 작성하기 위한 테스트 프레임워크입니다. 간단한 인터페이스와 풍부한 기능을 제공하며, 단일 헤더 파일로 쉽게 포함할 수 있습니다.

##### Catch2 설치 및 설정

1. **Windows, macOS, Linux**:
   - [Catch2 GitHub 페이지](https://github.com/catchorg/Catch2)에서 Catch2를 다운로드합니다.
   - `catch.hpp` 헤더 파일을 프로젝트에 포함합니다.

##### 기본 사용법

Catch2를 사용하여 간단한 테스트 케이스 작성:

1. **테스트 코드 작성**

```cpp
#define CATCH_CONFIG_MAIN
#include <catch2/catch.hpp>

// 테스트할 함수
int add(int a, int b) {
    return a + b;
}

// 테스트 케이스
TEST_CASE("Addition works", "[add]") {
    REQUIRE(add(1, 2) == 3);
    REQUIRE(add(2, 3) == 5);
}

TEST_CASE("Negative addition works", "[add]") {
    REQUIRE(add(-1, -2) == -3);
    REQUIRE(add(-2, -3) == -5);
}
```

2. **CMake 설정**

```cmake
cmake_minimum_required(VERSION 3.10)
project(Catch2Example)

set(CMAKE_CXX_STANDARD 11)

# 테스트 코드 추가
add_executable(runTests test.cpp)
target_include_directories(runTests PRIVATE ${CMAKE_SOURCE_DIR}/path/to/catch)
```

3. **빌드 및 실행**

```bash
mkdir build
cd build
cmake ..
make
./runTests
```

##### 주요 기능

1. **단위 테스트**:
   - 개별 함수나 클래스 메서드를 테스트할 수 있습니다.
2. **BDD 스타일 테스트**:
   - `SCENARIO`, `GIVEN`, `WHEN`, `THEN` 키워드를 사용하여 BDD 스타일의 테스트를 작성할 수 있습니다.
3. **테스트 케이스 및 섹션**:
   - 테스트 케이스와 섹션을 사용하여 테스트 코드를 구조화할 수 있습니다.
4. **매크로 기반 어서션**:
   - 다양한 매크로(`REQUIRE`, `CHECK`, `REQUIRE_FALSE`, `CHECK_FALSE` 등)를 사용하여 어서션을 작성할 수 있습니다.
5. **태그 기반 필터링**:
   - 테스트 케이스에 태그를 붙여 선택적으로 실행할 수 있습니다.
6. **간편한 통합**:
   - 단일 헤더 파일로 쉽게 프로젝트에 통합할 수 있습니다.

##### 예제 코드

간단한 테스트 케이스를 작성하여 함수의 동작을 검증하는 Catch2 코드입니다:

1. **테스트 코드 작성**

```cpp
#define CATCH_CONFIG_MAIN
#include <catch2/catch.hpp>

// 테스트할 함수
int add(int a, int b) {
    return a + b;
}

// 테스트 케이스
TEST_CASE("Addition works", "[add]") {
    REQUIRE(add(1, 2) == 3);
    REQUIRE(add(2, 3) == 5);
}

TEST_CASE("Negative addition works", "[add]") {
    REQUIRE(add(-1, -2) == -3);
    REQUIRE(add(-2, -3) == -5);
}
```

이 예제에서는 Catch2를 사용하여 `add` 함수의 동작을 검증하는 테스트 케이스를 작성하고, 테스트를 실행하는 프로그램을 구현합니다.
