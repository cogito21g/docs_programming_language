#### 12. Catch2

Catch2는 C++ 언어로 작성된 테스트 프레임워크로, 단순하고 직관적인 인터페이스를 제공합니다. 이 프레임워크는 단위 테스트, BDD 스타일의 테스트 등을 쉽게 작성할 수 있도록 도와줍니다.

#### 12.1. Catch2 설치 및 설정

Catch2를 사용하려면 먼저 설치 및 설정 과정이 필요합니다. Catch2는 헤더 온리 라이브러리로, 단순히 프로젝트에 헤더 파일을 포함하는 것만으로 사용할 수 있습니다.

##### Catch2 설치

Catch2는 GitHub에서 제공하는 소스 코드를 직접 다운로드하거나, 패키지 관리자를 통해 설치할 수 있습니다.

**패키지 관리자 사용 (Linux 및 macOS)**:

1. Homebrew (macOS):
   ```bash
   brew install catch2
   ```

2. APT (Ubuntu):
   ```bash
   sudo apt-get install catch2
   ```

**소스 코드 다운로드**:
1. GitHub에서 Catch2 소스 코드를 다운로드합니다:
   ```bash
   git clone https://github.com/catchorg/Catch2.git
   ```

2. `Catch2/single_include/catch2` 디렉토리의 내용을 프로젝트의 인클루드 디렉토리에 복사합니다.

##### 프로젝트 설정

Catch2를 사용하여 테스트를 작성하기 위해 CMake 설정 파일을 작성합니다.

**프로젝트 구조**:
```
Catch2Example/
├── CMakeLists.txt
├── main.cpp
├── tests/
│   └── test_example.cpp
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.10)

project(Catch2Example)

# C++ 표준 설정
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Catch2 라이브러리 포함 경로 설정
include_directories(${CMAKE_SOURCE_DIR}/Catch2/single_include)

# 테스트 파일 추가
add_executable(tests tests/test_example.cpp)

# Catch2 라이브러리 링크
target_link_libraries(tests)
```

##### 테스트 작성

Catch2를 사용하여 간단한 테스트를 작성합니다.

**tests/test_example.cpp**:
```cpp
#define CATCH_CONFIG_MAIN
#include "catch2/catch.hpp"

unsigned int Factorial(unsigned int number) {
    return number <= 1 ? number : Factorial(number - 1) * number;
}

TEST_CASE("Factorials are computed", "[factorial]") {
    REQUIRE(Factorial(0) == 1);
    REQUIRE(Factorial(1) == 1);
    REQUIRE(Factorial(2) == 2);
    REQUIRE(Factorial(3) == 6);
    REQUIRE(Factorial(10) == 3628800);
}
```

이 예제에서는 `Factorial` 함수를 테스트합니다. `REQUIRE` 매크로를 사용하여 예상 결과와 실제 결과를 비교합니다.

##### 프로젝트 빌드 및 실행

1. 빌드 디렉터리를 생성하고 이동합니다:
   ```bash
   mkdir build
   cd build
   ```

2. CMake를 실행하여 빌드 시스템을 생성합니다:
   ```bash
   cmake ..
   ```

3. 테스트를 빌드합니다:
   ```bash
   make
   ```

4. 빌드된 테스트 실행 파일을 실행합니다:
   ```bash
   ./tests
   ```
