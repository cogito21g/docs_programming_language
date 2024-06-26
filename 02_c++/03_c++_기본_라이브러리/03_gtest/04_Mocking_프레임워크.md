#### 3.4. Mocking 프레임워크

Google Mock은 Google Test의 일부로 제공되는 프레임워크로, Mock 객체를 생성하고 함수 호출을 시뮬레이션하는 데 사용됩니다. 이를 통해 의존성을 격리하고, 함수 호출의 인수와 반환 값을 검증할 수 있습니다.

##### Mock 객체 생성

Mock 객체를 생성하려면 먼저 가상 함수가 포함된 인터페이스를 정의해야 합니다. 그런 다음, Google Mock을 사용하여 해당 인터페이스를 상속하는 Mock 클래스를 생성합니다.

**인터페이스 및 Mock 클래스 정의**:

```cpp
#include <gtest/gtest.h>
#include <gmock/gmock.h>

// 인터페이스 정의
class MyInterface {
public:
    virtual ~MyInterface() = default;
    virtual int foo(int x) = 0;
    virtual std::string bar(const std::string& str) = 0;
};

// Mock 클래스 정의
class MockMyInterface : public MyInterface {
public:
    MOCK_METHOD(int, foo, (int), (override));
    MOCK_METHOD(std::string, bar, (const std::string&), (override));
};
```

##### 예제 1: 기본적인 Mock 객체 사용

Mock 객체를 사용하여 함수 호출을 시뮬레이션하고, 호출된 함수의 인수와 반환 값을 검증할 수 있습니다.

```cpp
#include <gtest/gtest.h>
#include <gmock/gmock.h>

// 테스트할 함수
int useFoo(MyInterface& interface, int x) {
    return interface.foo(x);
}

// 테스트 케이스
TEST(MockTest, UseFooTest) {
    MockMyInterface mock;
    EXPECT_CALL(mock, foo(5))
        .Times(1)
        .WillOnce(testing::Return(10));

    EXPECT_EQ(useFoo(mock, 5), 10);
}

int main(int argc, char **argv) {
    ::testing::InitGoogleMock(&argc, argv);
    return RUN_ALL_TESTS();
}
```

##### 예제 2: 인수와 반환 값 검증

Google Mock을 사용하여 함수 호출의 인수와 반환 값을 검증할 수 있습니다.

```cpp
#include <gtest/gtest.h>
#include <gmock/gmock.h>

// 테스트할 함수
std::string useBar(MyInterface& interface, const std::string& str) {
    return interface.bar(str);
}

// 테스트 케이스
TEST(MockTest, UseBarTest) {
    MockMyInterface mock;
    EXPECT_CALL(mock, bar("hello"))
        .Times(1)
        .WillOnce(testing::Return("world"));

    EXPECT_EQ(useBar(mock, "hello"), "world");
}

int main(int argc, char **argv) {
    ::testing::InitGoogleMock(&argc, argv);
    return RUN_ALL_TESTS();
}
```

##### 예제 3: 호출 횟수와 시퀀스 검증

Google Mock을 사용하여 함수 호출 횟수와 호출 순서를 검증할 수 있습니다.

```cpp
#include <gtest/gtest.h>
#include <gmock/gmock.h>

// 인터페이스 정의
class Calculator {
public:
    virtual ~Calculator() = default;
    virtual int add(int a, int b) = 0;
    virtual int subtract(int a, int b) = 0;
};

// Mock 클래스 정의
class MockCalculator : public Calculator {
public:
    MOCK_METHOD(int, add, (int, int), (override));
    MOCK_METHOD(int, subtract, (int, int), (override));
};

// 테스트할 함수
int calculateSumAndDifference(Calculator& calculator, int x, int y) {
    return calculator.add(x, y) + calculator.subtract(x, y);
}

// 테스트 케이스
TEST(MockTest, CallSequenceTest) {
    MockCalculator mock;
    testing::InSequence seq;

    EXPECT_CALL(mock, add(3, 2))
        .Times(1)
        .WillOnce(testing::Return(5));

    EXPECT_CALL(mock, subtract(3, 2))
        .Times(1)
        .WillOnce(testing::Return(1));

    EXPECT_EQ(calculateSumAndDifference(mock, 3, 2), 6);
}

int main(int argc, char **argv) {
    ::testing::InitGoogleMock(&argc, argv);
    return RUN_ALL_TESTS();
}
```

##### 예제 4: 함수 호출 인수 조건 검증

Google Mock을 사용하여 함수 호출 인수에 조건을 적용할 수 있습니다.

```cpp
#include <gtest/gtest.h>
#include <gmock/gmock.h>

// 인터페이스 정의
class Validator {
public:
    virtual ~Validator() = default;
    virtual bool validate(int value) = 0;
};

// Mock 클래스 정의
class MockValidator : public Validator {
public:
    MOCK_METHOD(bool, validate, (int), (override));
};

// 테스트할 함수
bool isValid(Validator& validator, int value) {
    return validator.validate(value);
}

// 테스트 케이스
TEST(MockTest, ValidateTest) {
    MockValidator mock;
    EXPECT_CALL(mock, validate(testing::Gt(0))) // 0보다 큰 값
        .Times(1)
        .WillOnce(testing::Return(true));

    EXPECT_TRUE(isValid(mock, 10));
}

int main(int argc, char **argv) {
    ::testing::InitGoogleMock(&argc, argv);
    return RUN_ALL_TESTS();
}
```

이 예제에서는 `validate` 함수가 호출될 때 인수가 0보다 큰지 확인합니다.
