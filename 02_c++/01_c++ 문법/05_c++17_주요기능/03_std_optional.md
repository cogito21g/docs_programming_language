#### 5.3. std::optional

`std::optional`은 C++17에서 도입된 표준 라이브러리 타입으로, 값이 있을 수도 없을 수도 있는 상황을 표현합니다. 이는 함수가 값을 반환하지 못할 가능성이 있는 경우를 안전하게 처리할 수 있게 해줍니다.

##### 기본 사용법

`std::optional`을 사용하여 값을 저장할 수 있으며, 값이 있는지 여부를 확인할 수 있습니다.

###### 예제

```cpp
#include <iostream>
#include <optional>
using namespace std;

optional<int> getNumber(bool flag) {
    if (flag) {
        return 42; // 값 반환
    } else {
        return {}; // 빈 optional 반환
    }
}

int main() {
    auto result = getNumber(true);
    if (result) {
        cout << "Number: " << *result << endl; // 값이 있는 경우 출력
    } else {
        cout << "No number" << endl;
    }

    result = getNumber(false);
    if (result) {
        cout << "Number: " << *result << endl;
    } else {
        cout << "No number" << endl;
    }

    return 0;
}
```

이 예제에서 `getNumber` 함수는 조건에 따라 값을 반환하거나 빈 `optional` 객체를 반환합니다. `main` 함수에서는 반환된 `optional` 객체에 값이 있는지 확인하고, 값이 있을 경우 이를 출력합니다.

##### optional의 멤버 함수

`std::optional`은 여러 유용한 멤버 함수를 제공합니다.

- **has_value()**: 값이 있는지 여부를 확인합니다.
- **value()**: 값을 반환합니다(값이 없을 경우 예외를 발생시킵니다).
- **value_or()**: 값이 있을 경우 해당 값을, 없을 경우 기본값을 반환합니다.
- **reset()**: `optional` 객체를 비웁니다.

###### 예제

```cpp
#include <iostream>
#include <optional>
using namespace std;

int main() {
    optional<int> opt = 10;

    cout << "Has value: " << opt.has_value() << endl;
    cout << "Value: " << opt.value() << endl;

    opt.reset(); // optional 객체 비우기
    cout << "Has value after reset: " << opt.has_value() << endl;

    cout << "Value or default: " << opt.value_or(42) << endl; // 값이 없을 경우 기본값 반환

    return 0;
}
```

이 예제에서 `optional` 객체의 멤버 함수를 사용하여 값이 있는지 확인하고, 값을 출력하며, 값을 비우고, 기본값을 반환하는 등의 작업을 수행합니다.

##### optional과 함수 반환

`std::optional`은 함수가 실패할 가능성이 있는 경우에 특히 유용합니다. 이를 통해 예외를 던지는 대신 안전하게 값을 반환하지 않는 상황을 처리할 수 있습니다.

###### 예제: 함수 반환

```cpp
#include <iostream>
#include <optional>
#include <string>
using namespace std;

optional<string> findName(int id) {
    if (id == 1) {
        return "Alice";
    } else if (id == 2) {
        return "Bob";
    } else {
        return {};
    }
}

int main() {
    auto name = findName(1);
    if (name) {
        cout << "Found: " << *name << endl;
    } else {
        cout << "Name not found" << endl;
    }

    name = findName(3);
    if (name) {
        cout << "Found: " << *name << endl;
    } else {
        cout << "Name not found" << endl;
    }

    return 0;
}
```

이 예제에서 `findName` 함수는 주어진 `id`에 해당하는 이름을 반환하거나, 이름이 없을 경우 빈 `optional` 객체를 반환합니다. `main` 함수에서는 반환된 `optional` 객체를 확인하여 이름을 출력합니다.

##### std::optional의 장점

- **안전한 값 반환**: 함수가 값을 반환하지 못할 경우를 안전하게 처리할 수 있습니다.
- **명확한 의도 표현**: 값이 있을 수도 없을 수도 있는 상황을 명확하게 표현할 수 있습니다.
- **예외 대체**: 예외를 던지지 않고도 함수의 실패를 처리할 수 있습니다.

#### 요약

- **std::optional**: 값이 있을 수도 없을 수도 있는 상황을 표현하는 타입입니다.
  - **has_value()**: 값이 있는지 여부를 확인합니다.
  - **value()**: 값을 반환합니다(값이 없을 경우 예외 발생).
  - **value_or()**: 값이 없을 경우 기본값을 반환합니다.
  - **reset()**: `optional` 객체를 비웁니다.
- **활용 예제**: 함수 반환 값이 없을 가능성을 안전하게 처리할 수 있습니다.
