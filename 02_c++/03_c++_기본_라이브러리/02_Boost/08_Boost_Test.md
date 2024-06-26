#### 2.8. Boost.Test

Boost.Test 라이브러리는 C++ 프로그램의 단위 테스트를 작성하고 실행하는 데 사용됩니다. 이 라이브러리는 테스트 케이스, 테스트 스위트, 단정문 등의 기능을 제공하여 테스트 작성과 관리를 용이하게 합니다.

##### Boost.Test 설치
Boost 라이브러리가 설치되어 있어야 합니다. 설치 방법은 이전 섹션을 참조하세요.

##### 주요 클래스 및 함수

- `BOOST_AUTO_TEST_SUITE`: 테스트 스위트를 정의합니다.
- `BOOST_AUTO_TEST_CASE`: 테스트 케이스를 정의합니다.
- `BOOST_CHECK`: 조건을 확인하고 실패하면 오류를 기록합니다.
- `BOOST_REQUIRE`: 조건을 확인하고 실패하면 테스트를 중단합니다.

##### 예제 1: 기본적인 테스트 케이스

```cpp
#define BOOST_TEST_MODULE MyTest
#include <boost/test/included/unit_test.hpp>

int add(int a, int b) {
    return a + b;
}

BOOST_AUTO_TEST_CASE(add_test) {
    BOOST_CHECK(add(2, 2) == 4);
    BOOST_CHECK(add(0, 0) == 0);
    BOOST_CHECK(add(-1, 1) == 0);
}
```

이 예제는 간단한 `add` 함수를 테스트하는 테스트 케이스를 정의합니다. `BOOST_CHECK`는 조건이 참인지 확인하고, 그렇지 않으면 실패를 기록합니다.

##### 예제 2: 테스트 스위트 사용

```cpp
#define BOOST_TEST_MODULE MyTest
#include <boost/test/included/unit_test.hpp>

struct MyFixture {
    MyFixture() {
        BOOST_TEST_MESSAGE("Setup fixture");
    }
    ~MyFixture() {
        BOOST_TEST_MESSAGE("Teardown fixture");
    }
};

BOOST_AUTO_TEST_SUITE(my_test_suite)

BOOST_FIXTURE_TEST_CASE(test1, MyFixture) {
    BOOST_CHECK_EQUAL(1 + 1, 2);
}

BOOST_FIXTURE_TEST_CASE(test2, MyFixture) {
    BOOST_REQUIRE_EQUAL(2 * 2, 4);
    BOOST_CHECK_EQUAL(2 - 2, 0);
}

BOOST_AUTO_TEST_SUITE_END()
```

이 예제는 `MyFixture`라는 픽스처를 사용하여 테스트 스위트를 정의합니다. 픽스처는 각 테스트 케이스 전에 설정되고, 테스트 케이스 후에 정리됩니다.

##### 예제 3: 예외 테스트

```cpp
#define BOOST_TEST_MODULE MyTest
#include <boost/test/included/unit_test.hpp>

void throwException() {
    throw std::runtime_error("Test exception");
}

BOOST_AUTO_TEST_CASE(exception_test) {
    BOOST_CHECK_THROW(throwException(), std::runtime_error);
}
```

이 예제는 `throwException` 함수가 `std::runtime_error` 예외를 던지는지 확인하는 테스트 케이스를 정의합니다. `BOOST_CHECK_THROW`는 예외가 발생하는지 확인합니다.

##### 예제 4: 병렬 테스트 실행

Boost.Test는 여러 테스트를 병렬로 실행할 수 있습니다. 이를 위해 `--run_test` 옵션을 사용할 수 있습니다.

```cpp
#define BOOST_TEST_MODULE MyTest
#include <boost/test/included/unit_test.hpp>

BOOST_AUTO_TEST_CASE(test1) {
    BOOST_CHECK_EQUAL(1 + 1, 2);
}

BOOST_AUTO_TEST_CASE(test2) {
    BOOST_CHECK_EQUAL(2 * 2, 4);
}

BOOST_AUTO_TEST_CASE(test3) {
    BOOST_CHECK_EQUAL(3 - 3, 0);
}
```

다음 명령을 사용하여 병렬로 테스트를 실행할 수 있습니다:

```bash
./MyTest --run_test=*
```

##### 예제 5: 사용자 정의 단정문

Boost.Test는 사용자 정의 단정문을 작성할 수 있습니다.

```cpp
#define BOOST_TEST_MODULE MyTest
#include <boost/test/included/unit_test.hpp>

bool is_even(int n) {
    return n % 2 == 0;
}

BOOST_AUTO_TEST_CASE(custom_assertion) {
    BOOST_CHECK(is_even(2));
    BOOST_CHECK(!is_even(3));
}
```

이 예제는 `is_even` 함수를 테스트하는 사용자 정의 단정문을 포함합니다.

##### 예제 6: XML 보고서 생성

Boost.Test는 XML 형식의 테스트 보고서를 생성할 수 있습니다.

```cpp
#define BOOST_TEST_MODULE MyTest
#include <boost/test/included/unit_test.hpp>

BOOST_AUTO_TEST_CASE(test1) {
    BOOST_CHECK_EQUAL(1 + 1, 2);
}

BOOST_AUTO_TEST_CASE(test2) {
    BOOST_CHECK_EQUAL(2 * 2, 4);
}
```

다음 명령을 사용하여 XML 보고서를 생성할 수 있습니다:

```bash
./MyTest --report_format=XML --report_level=detailed --log_level=all --result_code=no --log_sink=test_report.xml
```

이 예제에서는 Boost.Test를 사용하여 XML 형식의 상세한 테스트 보고서를 생성하는 방법을 보여줍니다.
