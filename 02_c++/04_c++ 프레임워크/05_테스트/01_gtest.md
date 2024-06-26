### 5. 테스트 프레임워크 (Testing Frameworks)

#### 5.1. Google Test (gtest)

Google Test는 C++ 애플리케이션을 테스트하기 위한 강력한 테스트 프레임워크입니다. 단위 테스트, 통합 테스트 등 다양한 테스트를 지원하며, 깔끔한 인터페이스와 다양한 기능을 제공합니다.

##### Google Test 설치 및 설정

1. **Windows, macOS, Linux**:
   - [Google Test GitHub 페이지](https://github.com/google/googletest)에서 Google Test를 다운로드합니다.
   - CMake를 사용하여 Google Test를 빌드하고 설치합니다:
     ```bash
     git clone https://github.com/google/googletest.git
     cd googletest
     mkdir build
     cd build
     cmake ..
     make
     sudo make install
     ```

##### 기본 사용법

Google Test를 사용하여 간단한 테스트 케이스 작성:

1. **테스트 코드 작성**

```cpp
#include <gtest/gtest.h>

// 테스트할 함수
int add(int a, int b) {
    return a + b;
}

// 테스트 케이스
TEST(AddTest, PositiveNumbers) {
    EXPECT_EQ(add(1, 2), 3);
    EXPECT_EQ(add(2, 3), 5);
}

TEST(AddTest, NegativeNumbers) {
    EXPECT_EQ(add(-1, -2), -3);
    EXPECT_EQ(add(-2, -3), -5);
}

// 메인 함수
int main(int argc, char **argv) {
    ::testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}
```

2. **CMake 설정**

```cmake
cmake_minimum_required(VERSION 3.10)
project(GoogleTestExample)

set(CMAKE_CXX_STANDARD 11)

# Google Test 라이브러리 추가
find_package(GTest REQUIRED)
include_directories(${GTEST_INCLUDE_DIRS})

# 테스트 코드 추가
add_executable(runTests test.cpp)
target_link_libraries(runTests ${GTEST_LIBRARIES} pthread)
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
2. **ASSERT 및 EXPECT 매크로**:
   - 다양한 매크로를 사용하여 테스트 결과를 확인할 수 있습니다. (예: `ASSERT_EQ`, `EXPECT_EQ`, `ASSERT_TRUE`, `EXPECT_TRUE` 등)
3. **테스트 픽스처**:
   - 반복적으로 사용되는 설정 코드를 공유하기 위해 테스트 픽스처를 사용할 수 있습니다.
4. **파라미터화 테스트**:
   - 다양한 입력 값에 대해 동일한 테스트를 반복할 수 있습니다.
5. **테스트 필터링**:
   - 특정 테스트를 선택하거나 제외할 수 있습니다.
6. **사용자 정의 매처**:
   - 복잡한 조건을 확인하기 위해 사용자 정의 매처를 작성할 수 있습니다.

##### 예제 코드

간단한 테스트 케이스를 작성하여 함수의 동작을 검증하는 Google Test 코드입니다:

1. **테스트 코드 작성**

```cpp
#include <gtest/gtest.h>

// 테스트할 함수
int add(int a, int b) {
    return a + b;
}

// 테스트 케이스
TEST(AddTest, PositiveNumbers) {
    EXPECT_EQ(add(1, 2), 3);
    EXPECT_EQ(add(2, 3), 5);
}

TEST(AddTest, NegativeNumbers) {
    EXPECT_EQ(add(-1, -2), -3);
    EXPECT_EQ(add(-2, -3), -5);
}

// 메인 함수
int main(int argc, char **argv) {
    ::testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}
```

이 예제에서는 Google Test를 사용하여 `add` 함수의 동작을 검증하는 테스트 케이스를 작성하고, 테스트를 실행하는 프로그램을 구현합니다.
