### 3. Google Test (gtest)

#### 3.1. Google Test 설치 및 설정

Google Test는 C++ 프로젝트를 위한 유닛 테스트 프레임워크입니다. 널리 사용되며 다양한 기능을 제공하여 테스트 작성과 실행을 쉽게 합니다.

##### 설치

**1. Google Test 다운로드**
   - [Google Test GitHub 페이지](https://github.com/google/googletest)에서 최신 버전을 다운로드합니다.
   - 또는 Git을 사용하여 클론합니다:
     ```bash
     git clone https://github.com/google/googletest.git
     ```

**2. 빌드 및 설치**
   - Google Test는 CMake를 사용하여 빌드합니다:
     ```bash
     cd googletest
     mkdir build
     cd build
     cmake ..
     make
     sudo make install
     ```

##### 설정

**1. CMakeLists.txt 설정**
   - 프로젝트의 CMakeLists.txt 파일에 Google Test를 포함시킵니다:
     ```cmake
     cmake_minimum_required(VERSION 3.10)
     project(MyProject)

     # Google Test
     enable_testing()
     find_package(GTest REQUIRED)
     include_directories(${GTEST_INCLUDE_DIRS})

     add_executable(MyTest main.cpp)
     target_link_libraries(MyTest ${GTEST_LIBRARIES} pthread)
     add_test(NAME MyTest COMMAND MyTest)
     ```

##### 예제

다음은 Google Test를 사용하여 간단한 테스트를 작성하고 실행하는 예제입니다.

**main.cpp**:

```cpp
#include <gtest/gtest.h>

// 테스트할 함수
int add(int a, int b) {
    return a + b;
}

// 테스트 케이스
TEST(AddTest, PositiveNumbers) {
    EXPECT_EQ(add(1, 2), 3);
    EXPECT_EQ(add(5, 5), 10);
}

TEST(AddTest, NegativeNumbers) {
    EXPECT_EQ(add(-1, -1), -2);
    EXPECT_EQ(add(-5, -5), -10);
}

int main(int argc, char **argv) {
    ::testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}
```

**빌드 및 실행**:

```bash
mkdir build
cd build
cmake ..
make
./MyTest
```

이 예제에서는 `add` 함수를 테스트하는 두 개의 테스트 케이스(`PositiveNumbers`, `NegativeNumbers`)를 작성합니다. `main` 함수에서 `RUN_ALL_TESTS()`를 호출하여 모든 테스트를 실행합니다.

##### Google Test의 주요 기능

- **EXPECT_EQ(a, b)**: `a`와 `b`가 같은지 확인합니다.
- **EXPECT_NE(a, b)**: `a`와 `b`가 다른지 확인합니다.
- **EXPECT_LT(a, b)**: `a`가 `b`보다 작은지 확인합니다.
- **EXPECT_LE(a, b)**: `a`가 `b`보다 작거나 같은지 확인합니다.
- **EXPECT_GT(a, b)**: `a`가 `b`보다 큰지 확인합니다.
- **EXPECT_GE(a, b)**: `a`가 `b`보다 크거나 같은지 확인합니다.

- **ASSERT_EQ(a, b)**: `a`와 `b`가 같지 않으면 테스트를 중단합니다.
- **ASSERT_NE(a, b)**: `a`와 `b`가 같으면 테스트를 중단합니다.
- **ASSERT_LT(a, b)**: `a`가 `b`보다 크거나 같으면 테스트를 중단합니다.
- **ASSERT_LE(a, b)**: `a`가 `b`보다 크면 테스트를 중단합니다.
- **ASSERT_GT(a, b)**: `a`가 `b`보다 작거나 같으면 테스트를 중단합니다.
- **ASSERT_GE(a, b)**: `a`가 `b`보다 작으면 테스트를 중단합니다.

##### 예제 2: 고급 기능

Google Test는 더 복잡한 테스트 시나리오를 지원합니다. 예를 들어, 테스트 픽스처를 사용하여 공통 설정 및 정리 작업을 수행할 수 있습니다.

**Advanced Example**:

```cpp
#include <gtest/gtest.h>

// 클래스 정의
class MyClass {
public:
    MyClass(int a, int b) : a(a), b(b) {}
    int add() { return a + b; }
private:
    int a;
    int b;
};

// 테스트 픽스처
class MyClassTest : public ::testing::Test {
protected:
    void SetUp() override {
        // 테스트 전에 수행할 작업
        obj = new MyClass(3, 4);
    }

    void TearDown() override {
        // 테스트 후에 수행할 작업
        delete obj;
    }

    MyClass* obj;
};

// 테스트 케이스
TEST_F(MyClassTest, AddTest) {
    EXPECT_EQ(obj->add(), 7);
}

int main(int argc, char **argv) {
    ::testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}
```

이 예제에서는 `MyClass`의 인스턴스를 생성하고, 테스트 픽스처 `MyClassTest`를 사용하여 공통 설정 및 정리 작업을 수행합니다.
