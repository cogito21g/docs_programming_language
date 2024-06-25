#### 10.2. SFINAE (Substitution Failure Is Not An Error)

SFINAE는 "Substitution Failure Is Not An Error"의 약자로, 템플릿 프로그래밍에서 템플릿 인수의 치환이 실패할 경우 컴파일러가 에러를 발생시키지 않고, 대체 가능한 템플릿을 찾도록 하는 기법입니다. 이는 주로 함수 오버로딩과 템플릿 특수화를 통해 조건부로 코드를 활성화하거나 비활성화하는 데 사용됩니다.

##### SFINAE의 기본 사용법

SFINAE는 주로 `std::enable_if`와 함께 사용되어 조건부로 템플릿을 활성화하거나 비활성화합니다. 이를 통해 특정 조건에 맞는 템플릿만 인스턴스화되도록 할 수 있습니다.

###### 예제

```cpp
#include <iostream>
#include <type_traits>
using namespace std;

// 기본 템플릿: 타입 T가 정수 타입일 때만 활성화됨
template <typename T>
typename enable_if<is_integral<T>::value, void>::type print(T value) {
    cout << "Integral value: " << value << endl;
}

// 특수화: 타입 T가 정수 타입이 아닐 때 활성화됨
template <typename T>
typename enable_if<!is_integral<T>::value, void>::type print(T value) {
    cout << "Non-integral value: " << value << endl;
}

int main() {
    print(42);        // 정수 타입이므로 첫 번째 템플릿 활성화
    print(3.14);      // 정수 타입이 아니므로 두 번째 템플릿 활성화
    print("Hello");   // 정수 타입이 아니므로 두 번째 템플릿 활성화

    return 0;
}
```

이 예제에서 `print` 함수는 `std::enable_if`를 사용하여 타입 T가 정수 타입일 때와 아닐 때 각각 다른 버전의 템플릿이 활성화되도록 합니다.

##### 고급 SFINAE 기법

SFINAE를 사용하여 보다 복잡한 조건부 템플릿을 정의할 수 있습니다. 예를 들어, 특정 멤버 함수나 타입이 존재하는지 검사하여 템플릿을 활성화할 수 있습니다.

###### 예제: 특정 멤버 함수 존재 여부 검사

```cpp
#include <iostream>
#include <type_traits>
using namespace std;

// helper: T가 foo 멤버 함수를 가지고 있는지 확인
template <typename T>
class has_foo {
    typedef char one;
    typedef long two;

    template <typename C> static one test(decltype(&C::foo));
    template <typename C> static two test(...);

public:
    enum { value = sizeof(test<T>(0)) == sizeof(one) };
};

// foo 멤버 함수가 있는 경우 활성화되는 템플릿
template <typename T>
typename enable_if<has_foo<T>::value, void>::type callFoo(T& obj) {
    cout << "Calling foo()" << endl;
    obj.foo();
}

// foo 멤버 함수가 없는 경우 활성화되는 템플릿
template <typename T>
typename enable_if<!has_foo<T>::value, void>::type callFoo(T& obj) {
    cout << "No foo() function available" << endl;
}

// 예제 클래스
class WithFoo {
public:
    void foo() {
        cout << "WithFoo::foo()" << endl;
    }
};

class WithoutFoo {};

int main() {
    WithFoo obj1;
    WithoutFoo obj2;

    callFoo(obj1);  // WithFoo::foo()가 호출됨
    callFoo(obj2);  // foo()가 없다는 메시지가 출력됨

    return 0;
}
```

이 예제에서 `has_foo`는 템플릿 클래스 T가 `foo` 멤버 함수를 가지고 있는지 검사합니다. 이를 바탕으로 `callFoo` 함수는 T가 `foo` 멤버 함수를 가지고 있는지 여부에 따라 다르게 동작합니다.

##### SFINAE의 장점

- **유연성**: 다양한 조건에 따라 템플릿을 활성화하거나 비활성화할 수 있습니다.
- **타입 안전성**: 컴파일 타임에 조건을 검사하여 타입 안전성을 보장합니다.
- **코드 재사용성**: 조건부로 활성화되는 템플릿을 통해 코드 재사용성을 높일 수 있습니다.

##### SFINAE의 단점

- **복잡성**: SFINAE를 이해하고 사용하는 것은 다소 복잡할 수 있습니다.
- **디버깅 어려움**: 템플릿 인수 치환 실패로 인해 발생하는 문제를 디버깅하는 것이 어려울 수 있습니다.

##### SFINAE와 함께 사용되는 주요 도구들

- **std::enable_if**: 조건부로 템플릿을 활성화하거나 비활성화하는 데 사용됩니다.
- **std::is_integral, std::is_floating_point 등**: 타입 특성을 검사하는 데 사용됩니다.
- **decltype**: 표현식의 타입을 추론하는 데 사용됩니다.

###### 예제: std::is_floating_point와 SFINAE

```cpp
#include <iostream>
#include <type_traits>
using namespace std;

template <typename T>
typename enable_if<is_floating_point<T>::value, void>::type print(T value) {
    cout << "Floating point value: " << value << endl;
}

template <typename T>
typename enable_if<!is_floating_point<T>::value, void>::type print(T value) {
    cout << "Non-floating point value: " << value << endl;
}

int main() {
    print(42);       // 정수 타입이므로 두 번째 템플릿 활성화
    print(3.14);     // 부동 소수점 타입이므로 첫 번째 템플릿 활성화

    return 0;
}
```

이 예제에서 `print` 함수는 `std::is_floating_point`를 사용하여 타입 T가 부동 소수점 타입인지 여부에 따라 다른 버전의 템플릿이 활성화되도록 합니다.

#### 요약

- **SFINAE (Substitution Failure Is Not An Error)**: 템플릿 인수의 치환이 실패할 경우 컴파일러가 에러를 발생시키지 않고 대체 가능한 템플릿을 찾도록 하는 기법입니다.
  - **구현**: `std::enable_if`와 같은 도구를 사용하여 조건부로 템플릿을 활성화하거나 비활성화합니다.
  - **장점**: 유연성, 타입 안전성, 코드 재사용성
  - **단점**: 복잡성, 디버깅 어려움
- **주요 도구들**: `std::enable_if`, `std::is_integral`, `std::is_floating_point`, `decltype` 등
