#### 5.3. Boost.Test

Boost.Test는 C++ 애플리케이션을 테스트하기 위한 Boost 라이브러리의 일부분으로, 단위 테스트, 통합 테스트 등을 지원합니다. 다양한 어서션 매크로와 테스트 스위트를 제공하여 테스트 작성과 관리를 용이하게 합니다.

##### Boost.Test 설치 및 설정

1. **Windows, macOS, Linux**:
   - Boost 라이브러리를 다운로드하고 설치합니다. Boost.Test는 Boost 라이브러리의 일부이므로 별도의 설치가 필요 없습니다.
   - [Boost 다운로드 페이지](https://www.boost.org/users/download/)에서 Boost 라이브러리를 다운로드합니다.
   - 압축을 풀고, `bootstrap.bat` (Windows) 또는 `bootstrap.sh` (macOS, Linux)를 실행하여 Boost 라이브러리를 빌드합니다:
     ```bash
     ./bootstrap.sh
     ./b2
     ```

##### 기본 사용법

Boost.Test를 사용하여 간단한 테스트 케이스 작성:

1. **테스트 코드 작성**

```cpp
#define BOOST_TEST_MODULE MyTest
#include <boost/test/included/unit_test.hpp>

// 테스트할 함수
int add(int a, int b) {
    return a + b;
}

// 테스트 케이스
BOOST_AUTO_TEST_CASE(AdditionTest) {
    BOOST_CHECK(add(1, 2) == 3);
    BOOST_CHECK(add(2, 3) == 5);
}

BOOST_AUTO_TEST_CASE(NegativeAdditionTest) {
    BOOST_CHECK(add(-1, -2) == -3);
    BOOST_CHECK(add(-2, -3) == -5);
}
```

2. **CMake 설정**

```cmake
cmake_minimum_required(VERSION 3.10)
project(BoostTestExample)

set(CMAKE_CXX_STANDARD 11)

# Boost 라이브러리 설정
find_package(Boost REQUIRED COMPONENTS unit_test_framework)
include_directories(${Boost_INCLUDE_DIRS})

# 테스트 코드 추가
add_executable(runTests test.cpp)
target_link_libraries(runTests ${Boost_LIBRARIES})
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
2. **어셉션 매크로**:
   - 다양한 어셉션 매크로(`BOOST_CHECK`, `BOOST_REQUIRE`, `BOOST_CHECK_EQUAL`, `BOOST_REQUIRE_EQUAL` 등)를 사용하여 어서션을 작성할 수 있습니다.
3. **테스트 스위트**:
   - 테스트 스위트를 사용하여 테스트 케이스를 그룹화하고 구조화할 수 있습니다.
4. **예외 테스트**:
   - 예외가 발생하는지 테스트할 수 있는 매크로를 제공합니다 (`BOOST_CHECK_THROW`, `BOOST_CHECK_NO_THROW` 등).
5. **출력 포맷**:
   - 다양한 출력 포맷(XML, JUnit 등)을 지원하여 테스트 결과를 다양한 형식으로 출력할 수 있습니다.

##### 예제 코드

간단한 테스트 케이스를 작성하여 함수의 동작을 검증하는 Boost.Test 코드입니다:

1. **테스트 코드 작성**

```cpp
#define BOOST_TEST_MODULE MyTest
#include <boost/test/included/unit_test.hpp>

// 테스트할 함수
int add(int a, int b) {
    return a + b;
}

// 테스트 케이스
BOOST_AUTO_TEST_CASE(AdditionTest) {
    BOOST_CHECK(add(1, 2) == 3);
    BOOST_CHECK(add(2, 3) == 5);
}

BOOST_AUTO_TEST_CASE(NegativeAdditionTest) {
    BOOST_CHECK(add(-1, -2) == -3);
    BOOST_CHECK(add(-2, -3) == -5);
}
```

이 예제에서는 Boost.Test를 사용하여 `add` 함수의 동작을 검증하는 테스트 케이스를 작성하고, 테스트를 실행하는 프로그램을 구현합니다.
