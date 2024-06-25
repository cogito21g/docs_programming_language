#### 7.6. std::generator: 범위 기반 코루틴 지원

C++23에서는 `std::generator`가 도입되어 코루틴을 사용하여 데이터를 생성하고, 이를 범위 기반 for 루프로 쉽게 처리할 수 있습니다. `std::generator`는 특히 비동기 프로그래밍과 데이터 스트림 처리에 유용합니다.

##### std::generator의 기본 사용법

`std::generator`를 사용하면 코루틴을 통해 데이터를 생성하고, 이를 범위 기반 for 루프로 처리할 수 있습니다. 이는 데이터를 순차적으로 생성하고 처리하는 데 유용합니다.

###### 기본 예제

```cpp
#include <iostream>
#include <generator>
#include <ranges>
using namespace std;

std::generator<int> count(int max) {
    for (int i = 0; i < max; ++i) {
        co_yield i;
    }
}

int main() {
    for (int value : count(10)) {
        cout << value << " ";
    }
    cout << endl;

    return 0;
}
```

이 예제에서 `count` 함수는 코루틴을 사용하여 0부터 `max`까지의 값을 생성하고, 범위 기반 for 루프를 사용하여 각 값을 출력합니다.

##### std::generator와 범위 라이브러리

`std::generator`는 범위 라이브러리와 결합하여 강력한 데이터 처리 파이프라인을 구축할 수 있습니다.

###### 예제: std::ranges와 결합

```cpp
#include <iostream>
#include <generator>
#include <ranges>
using namespace std;
namespace ranges = std::ranges;

std::generator<int> count(int max) {
    for (int i = 0; i < max; ++i) {
        co_yield i;
    }
}

int main() {
    auto gen = count(10);
    
    // std::ranges를 사용하여 변환 및 필터링
    auto transformed = gen | ranges::views::transform([](int n) { return n * 2; })
                          | ranges::views::filter([](int n) { return n % 4 == 0; });

    for (int value : transformed) {
        cout << value << " ";
    }
    cout << endl;

    return 0;
}
```

이 예제에서 `count` 코루틴은 0부터 `max`까지의 값을 생성하고, `std::ranges::views::transform`과 `std::ranges::views::filter`를 사용하여 변환 및 필터링합니다. 결과는 범위 기반 for 루프로 출력됩니다.

##### std::generator의 장점

- **범위 기반 처리**: 데이터를 순차적으로 생성하고, 범위 기반 for 루프로 처리할 수 있습니다.
- **유연성**: 범위 라이브러리와 결합하여 강력한 데이터 처리 파이프라인을 구축할 수 있습니다.
- **비동기 프로그래밍**: 비동기 데이터 스트림 처리에 유용합니다.

#### 요약

- **std::generator**: C++23에서 도입된 기능으로, 코루틴을 사용하여 데이터를 생성하고 범위 기반 for 루프로 처리할 수 있습니다.
  - **기본 사용법**: 코루틴을 사용하여 데이터를 생성하고, 범위 기반 for 루프로 처리합니다.
  - **범위 라이브러리와 결합**: `std::ranges`와 결합하여 강력한 데이터 처리 파이프라인을 구축할 수 있습니다.
- **장점**: 범위 기반 처리, 유연성, 비동기 프로그래밍에 유용합니다.
