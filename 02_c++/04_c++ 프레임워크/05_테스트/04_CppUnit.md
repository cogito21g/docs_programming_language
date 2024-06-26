#### 5.4. CppUnit

CppUnit은 C++에서 단위 테스트를 작성하기 위한 프레임워크로, JUnit 스타일의 테스트 케이스와 스위트를 제공합니다. CppUnit을 사용하면 테스트 케이스를 작성하고, 실행하고, 결과를 확인할 수 있습니다.

##### CppUnit 설치 및 설정

1. **Windows**:
   - [CppUnit 다운로드 페이지](https://sourceforge.net/projects/cppunit/files/cppunit/)에서 CppUnit을 다운로드합니다.
   - Visual Studio 프로젝트에서 CppUnit의 include 디렉토리와 lib 디렉토리를 설정합니다.
   - 필요한 라이브러리를 링크합니다: `cppunit.lib`.

2. **macOS**:
   - Homebrew를 사용하여 CppUnit을 설치합니다:
     ```bash
     brew install cppunit
     ```

3. **Linux**:
   - 패키지 관리자를 사용하여 CppUnit을 설치합니다:
     ```bash
     sudo apt-get install libcppunit-dev
     ```

##### 기본 사용법

CppUnit을 사용하여 간단한 테스트 케이스 작성:

1. **테스트 코드 작성**

```cpp
#include <cppunit/TestCase.h>
#include <cppunit/extensions/HelperMacros.h>
#include <cppunit/ui/text/TestRunner.h>

class AddTest : public CppUnit::TestCase {
    CPPUNIT_TEST_SUITE(AddTest);
    CPPUNIT_TEST(testPositiveNumbers);
    CPPUNIT_TEST(testNegativeNumbers);
    CPPUNIT_TEST_SUITE_END();

public:
    void testPositiveNumbers() {
        CPPUNIT_ASSERT(add(1, 2) == 3);
        CPPUNIT_ASSERT(add(2, 3) == 5);
    }

    void testNegativeNumbers() {
        CPPUNIT_ASSERT(add(-1, -2) == -3);
        CPPUNIT_ASSERT(add(-2, -3) == -5);
    }

private:
    int add(int a, int b) {
        return a + b;
    }
};

int main(int argc, char* argv[]) {
    CppUnit::TextUi::TestRunner runner;
    runner.addTest(AddTest::suite());
    runner.run();
    return 0;
}
```

2. **CMake 설정**

```cmake
cmake_minimum_required(VERSION 3.10)
project(CppUnitExample)

set(CMAKE_CXX_STANDARD 11)

# CppUnit 라이브러리 설정
find_package(CppUnit REQUIRED)
include_directories(${CPPUNIT_INCLUDE_DIRS})

# 테스트 코드 추가
add_executable(runTests test.cpp)
target_link_libraries(runTests ${CPPUNIT_LIBRARIES})
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
2. **테스트 케이스 및 스위트**:
   - 테스트 케이스와 테스트 스위트를 사용하여 테스트 코드를 구조화할 수 있습니다.
3. **어셉션 매크로**:
   - 다양한 어셉션 매크로(`CPPUNIT_ASSERT`, `CPPUNIT_ASSERT_EQUAL`, `CPPUNIT_ASSERT_THROW` 등)를 사용하여 어서션을 작성할 수 있습니다.
4. **출력 포맷**:
   - 다양한 출력 포맷을 지원하여 테스트 결과를 다양한 형식으로 출력할 수 있습니다.
5. **텍스트 및 GUI 인터페이스**:
   - 텍스트 기반 및 GUI 기반의 테스트 러너를 제공하여 테스트를 실행할 수 있습니다.

##### 예제 코드

간단한 테스트 케이스를 작성하여 함수의 동작을 검증하는 CppUnit 코드입니다:

1. **테스트 코드 작성**

```cpp
#include <cppunit/TestCase.h>
#include <cppunit/extensions/HelperMacros.h>
#include <cppunit/ui/text/TestRunner.h>

class AddTest : public CppUnit::TestCase {
    CPPUNIT_TEST_SUITE(AddTest);
    CPPUNIT_TEST(testPositiveNumbers);
    CPPUNIT_TEST(testNegativeNumbers);
    CPPUNIT_TEST_SUITE_END();

public:
    void testPositiveNumbers() {
        CPPUNIT_ASSERT(add(1, 2) == 3);
        CPPUNIT_ASSERT(add(2, 3) == 5);
    }

    void testNegativeNumbers() {
        CPPUNIT_ASSERT(add(-1, -2) == -3);
        CPPUNIT_ASSERT(add(-2, -3) == -5);
    }

private:
    int add(int a, int b) {
        return a + b;
    }
};

int main(int argc, char* argv[]) {
    CppUnit::TextUi::TestRunner runner;
    runner.addTest(AddTest::suite());
    runner.run();
    return 0;
}
```

이 예제에서는 CppUnit을 사용하여 `add` 함수의 동작을 검증하는 테스트 케이스를 작성하고, 테스트를 실행하는 프로그램을 구현합니다.
