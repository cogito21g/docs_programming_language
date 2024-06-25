#### 5.4. std::variant

`std::variant`는 C++17에서 도입된 표준 라이브러리 타입으로, 여러 타입 중 하나를 저장할 수 있는 유니온(variant) 타입입니다. 이는 다양한 타입을 하나의 변수에 저장하고 처리할 수 있게 해줍니다.

##### 기본 사용법

`std::variant`를 사용하면 여러 타입 중 하나를 저장할 수 있으며, 현재 저장된 타입을 안전하게 확인하고 접근할 수 있습니다.

###### 예제

```cpp
#include <iostream>
#include <variant>
#include <string>
using namespace std;

int main() {
    variant<int, double, string> var;

    var = 42;
    cout << "int: " << get<int>(var) << endl;

    var = 3.14;
    cout << "double: " << get<double>(var) << endl;

    var = "Hello, World!";
    cout << "string: " << get<string>(var) << endl;

    return 0;
}
```

이 예제에서 `variant<int, double, string>` 타입의 `var` 변수가 각각 정수, 실수, 문자열 값을 저장하고 출력합니다.

##### std::get

`std::get` 함수는 `variant`에 저장된 값을 가져옵니다. `std::get`은 저장된 타입을 정확히 지정해야 합니다. 잘못된 타입을 지정하면 `std::bad_variant_access` 예외가 발생합니다.

###### 예제

```cpp
#include <iostream>
#include <variant>
using namespace std;

int main() {
    variant<int, float> var = 42;

    try {
        cout << "int: " << get<int>(var) << endl;
        cout << "float: " << get<float>(var) << endl; // std::bad_variant_access 예외 발생
    } catch (const bad_variant_access& e) {
        cout << "Exception: " << e.what() << endl;
    }

    return 0;
}
```

이 예제에서 `var` 변수는 `int` 값을 저장하고 있습니다. `std::get<float>`를 호출하면 `std::bad_variant_access` 예외가 발생합니다.

##### std::holds_alternative

`std::holds_alternative` 함수는 `variant`에 특정 타입의 값이 저장되어 있는지 확인합니다.

###### 예제

```cpp
#include <iostream>
#include <variant>
using namespace std;

int main() {
    variant<int, float> var = 42;

    if (holds_alternative<int>(var)) {
        cout << "var holds an int" << endl;
    } else {
        cout << "var does not hold an int" << endl;
    }

    var = 3.14f;

    if (holds_alternative<float>(var)) {
        cout << "var holds a float" << endl;
    } else {
        cout << "var does not hold a float" << endl;
    }

    return 0;
}
```

이 예제에서 `holds_alternative` 함수를 사용하여 `var` 변수에 저장된 타입이 `int`인지 `float`인지 확인합니다.

##### std::visit

`std::visit` 함수는 `variant`에 저장된 값에 대해 다형적(variant)으로 작업을 수행할 수 있게 합니다. 이를 통해 `variant`에 저장된 값의 타입에 따라 적절한 작업을 수행할 수 있습니다.

###### 예제

```cpp
#include <iostream>
#include <variant>
using namespace std;

struct Visitor {
    void operator()(int i) const {
        cout << "int: " << i << endl;
    }
    void operator()(double d) const {
        cout << "double: " << d << endl;
    }
    void operator()(const string& s) const {
        cout << "string: " << s << endl;
    }
};

int main() {
    variant<int, double, string> var;

    var = 42;
    visit(Visitor{}, var);

    var = 3.14;
    visit(Visitor{}, var);

    var = "Hello, World!";
    visit(Visitor{}, var);

    return 0;
}
```

이 예제에서 `Visitor` 구조체는 여러 타입에 대해 다형적(variant)으로 작업을 수행할 수 있는 호출 연산자를 정의합니다. `std::visit` 함수는 `Visitor` 객체를 사용하여 `variant`에 저장된 값에 대해 적절한 작업을 수행합니다.

##### std::variant의 장점

- **안전한 타입 관리**: 여러 타입을 하나의 변수에 안전하게 저장하고 관리할 수 있습니다.
- **다형적 작업**: `std::visit`을 통해 저장된 타입에 따라 다형적 작업을 수행할 수 있습니다.
- **명확한 의도 표현**: 코드의 의도를 명확하게 표현할 수 있으며, 타입 안정성을 보장합니다.

#### 요약

- **std::variant**: 여러 타입 중 하나를 저장할 수 있는 유니온(variant) 타입입니다.
  - **std::get**: `variant`에 저장된 값을 가져옵니다. 잘못된 타입을 지정하면 예외가 발생합니다.
  - **std::holds_alternative**: `variant`에 특정 타입의 값이 저장되어 있는지 확인합니다.
  - **std::visit**: `variant`에 저장된 값에 대해 다형적 작업을 수행합니다.
- **장점**: 안전한 타입 관리, 다형적 작업, 명확한 의도 표현을 지원합니다.
