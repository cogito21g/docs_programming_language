#### 5.5. std::any

`std::any`는 C++17에서 도입된 표준 라이브러리 타입으로, 모든 타입의 값을 저장할 수 있는 유니버설 컨테이너입니다. 이를 통해 컴파일 타임에 타입을 알 수 없는 값을 안전하게 저장하고 관리할 수 있습니다.

##### 기본 사용법

`std::any`를 사용하여 다양한 타입의 값을 저장할 수 있습니다.

###### 예제

```cpp
#include <iostream>
#include <any>
using namespace std;

int main() {
    any a = 10;
    cout << "a contains: " << any_cast<int>(a) << endl;

    a = 3.14;
    cout << "a contains: " << any_cast<double>(a) << endl;

    a = string("Hello, World!");
    cout << "a contains: " << any_cast<string>(a) << endl;

    return 0;
}
```

이 예제에서 `any` 타입의 `a` 변수에 각각 `int`, `double`, `string` 값을 저장하고, `any_cast`를 사용하여 값을 출력합니다.

##### std::any_cast

`std::any_cast`는 `std::any`에 저장된 값을 원래 타입으로 변환하여 반환합니다. 잘못된 타입으로 캐스팅을 시도하면 `std::bad_any_cast` 예외가 발생합니다.

###### 예제

```cpp
#include <iostream>
#include <any>
using namespace std;

int main() {
    any a = 10;

    try {
        cout << "a contains: " << any_cast<int>(a) << endl;
        cout << "a contains: " << any_cast<double>(a) << endl; // std::bad_any_cast 예외 발생
    } catch (const bad_any_cast& e) {
        cout << "Exception: " << e.what() << endl;
    }

    return 0;
}
```

이 예제에서 `any_cast<int>`는 성공적으로 값을 반환하지만, `any_cast<double>`는 잘못된 타입으로 캐스팅을 시도하여 예외가 발생합니다.

##### std::any의 멤버 함수

`std::any`는 여러 유용한 멤버 함수를 제공합니다.

- **has_value()**: `any` 객체가 값을 가지고 있는지 확인합니다.
- **type()**: `any` 객체에 저장된 값의 타입 정보를 반환합니다.
- **reset()**: `any` 객체를 비웁니다.

###### 예제

```cpp
#include <iostream>
#include <any>
using namespace std;

int main() {
    any a = 10;
    cout << "Has value: " << a.has_value() << endl;
    cout << "Type: " << a.type().name() << endl;

    a.reset();
    cout << "Has value after reset: " << a.has_value() << endl;

    return 0;
}
```

이 예제에서 `any` 객체의 멤버 함수를 사용하여 값이 있는지 확인하고, 타입 정보를 출력하며, 값을 비웁니다.

##### std::any와 함수 반환

`std::any`는 함수가 여러 타입의 값을 반환해야 하는 경우에 유용합니다.

###### 예제: 함수 반환

```cpp
#include <iostream>
#include <any>
#include <string>
using namespace std;

any getValue(bool flag) {
    if (flag) {
        return 42;
    } else {
        return string("Hello");
    }
}

int main() {
    auto value = getValue(true);
    if (value.type() == typeid(int)) {
        cout << "Value is int: " << any_cast<int>(value) << endl;
    } else if (value.type() == typeid(string)) {
        cout << "Value is string: " << any_cast<string>(value) << endl;
    }

    value = getValue(false);
    if (value.type() == typeid(int)) {
        cout << "Value is int: " << any_cast<int>(value) << endl;
    } else if (value.type() == typeid(string)) {
        cout << "Value is string: " << any_cast<string>(value) << endl;
    }

    return 0;
}
```

이 예제에서 `getValue` 함수는 조건에 따라 `int` 또는 `string` 값을 반환합니다. `main` 함수에서는 반환된 `any` 객체의 타입을 확인하고, 적절한 타입으로 값을 출력합니다.

##### std::any의 장점

- **유연성**: 모든 타입의 값을 저장할 수 있습니다.
- **안전성**: 타입 안정성을 보장하며, 잘못된 타입 캐스팅을 방지합니다.
- **명확한 의도 표현**: 타입을 미리 알 수 없는 상황에서 유용합니다.

#### 요약

- **std::any**: 모든 타입의 값을 저장할 수 있는 유니버설 컨테이너입니다.
  - **std::any_cast**: `any`에 저장된 값을 원래 타입으로 변환합니다. 잘못된 타입 캐스팅 시 예외가 발생합니다.
  - **has_value()**: 값이 있는지 확인합니다.
  - **type()**: 저장된 값의 타입 정보를 반환합니다.
  - **reset()**: `any` 객체를 비웁니다.
- **장점**: 유연성, 안전성, 명확한 의도 표현을 제공합니다.
