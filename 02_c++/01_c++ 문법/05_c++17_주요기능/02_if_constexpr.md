#### 5.2. if constexpr

C++17에서 도입된 `if constexpr`은 컴파일 타임에 조건을 검사하여 코드를 컴파일할 수 있도록 해주는 기능입니다. 이를 통해 템플릿 메타프로그래밍과 조건부 컴파일을 보다 간결하고 명확하게 작성할 수 있습니다.

##### 기본 사용법

`if constexpr`의 기본 구조는 다음과 같습니다:

```cpp
if constexpr (condition) {
    // 조건이 참일 때 컴파일할 코드
} else {
    // 조건이 거짓일 때 컴파일할 코드
}
```

- **condition**: 컴파일 타임에 평가되는 조건식입니다. 이 조건식이 참일 경우 해당 블록의 코드가 컴파일되고, 거짓일 경우 다른 블록의 코드가 컴파일됩니다.

###### 예제

```cpp
#include <iostream>
#include <type_traits>
using namespace std;

template <typename T>
void printType(T value) {
    if constexpr (is_integral<T>::value) {
        cout << "Integral type: " << value << endl;
    } else {
        cout << "Non-integral type: " << value << endl;
    }
}

int main() {
    printType(10);       // 정수형 타입
    printType(3.14);     // 실수형 타입
    printType("Hello");  // 문자열 타입

    return 0;
}
```

이 예제에서 `printType` 함수는 `if constexpr`을 사용하여 템플릿 매개변수 `T`가 정수형인지 여부에 따라 다른 코드를 컴파일합니다. `is_integral<T>::value`는 `T`가 정수형일 경우 `true`를 반환합니다.

##### 컴파일 타임 조건 검사

`if constexpr`을 사용하면 컴파일 타임에 조건을 검사하여 불필요한 코드를 배제할 수 있습니다. 이는 템플릿 메타프로그래밍에서 특히 유용합니다.

###### 예제: 컴파일 타임 조건 검사

```cpp
#include <iostream>
#include <type_traits>
using namespace std;

template <typename T>
void process(T value) {
    if constexpr (is_pointer<T>::value) {
        cout << "Pointer type: " << *value << endl;
    } else {
        cout << "Non-pointer type: " << value << endl;
    }
}

int main() {
    int a = 10;
    int* p = &a;

    process(a);  // Non-pointer type
    process(p);  // Pointer type

    return 0;
}
```

이 예제에서 `process` 함수는 `if constexpr`을 사용하여 `T`가 포인터 타입인지 여부에 따라 다른 코드를 컴파일합니다. `is_pointer<T>::value`는 `T`가 포인터 타입일 경우 `true`를 반환합니다.

##### 조건부 멤버 함수

`if constexpr`을 사용하면 조건부로 멤버 함수를 제공할 수 있습니다. 이는 템플릿 클래스에서 특정 조건에 따라 멤버 함수를 정의할 때 유용합니다.

###### 예제: 조건부 멤버 함수

```cpp
#include <iostream>
#include <type_traits>
using namespace std;

template <typename T>
class Container {
public:
    T value;

    Container(T val) : value(val) {}

    void print() {
        if constexpr (is_pointer<T>::value) {
            cout << "Pointer: " << *value << endl;
        } else {
            cout << "Value: " << value << endl;
        }
    }
};

int main() {
    Container<int> c1(10);
    c1.print();  // Value: 10

    int a = 20;
    Container<int*> c2(&a);
    c2.print();  // Pointer: 20

    return 0;
}
```

이 예제에서 `Container` 클래스는 `if constexpr`을 사용하여 `T`가 포인터 타입인지 여부에 따라 `print` 함수의 동작을 달리합니다.

##### 상수 식과 `if constexpr`

`if constexpr`은 상수 식을 사용하여 컴파일 타임에 조건을 평가합니다. 이는 조건부 컴파일과 최적화에 매우 유용합니다.

###### 예제: 상수 식과 `if constexpr`

```cpp
#include <iostream>
using namespace std;

constexpr bool isEven(int n) {
    return n % 2 == 0;
}

template <int N>
void checkNumber() {
    if constexpr (isEven(N)) {
        cout << N << " is even" << endl;
    } else {
        cout << N << " is odd" << endl;
    }
}

int main() {
    checkNumber<4>();  // 4 is even
    checkNumber<5>();  // 5 is odd

    return 0;
}
```

이 예제에서 `isEven` 함수는 상수 식을 사용하여 주어진 숫자가 짝수인지 확인합니다. `checkNumber` 함수는 `if constexpr`을 사용하여 컴파일 타임에 조건을 평가하고, 주어진 숫자가 짝수인지 홀수인지에 따라 다른 코드를 컴파일합니다.

#### 요약

- **if constexpr**: 컴파일 타임에 조건을 검사하여 코드를 컴파일할 수 있도록 해주는 기능입니다.
  - 기본 구조: `if constexpr (condition) { ... } else { ... }`
  - 템플릿 메타프로그래밍에서 유용하게 사용됩니다.
  - 조건부 멤버 함수와 상수 식과 함께 사용하여 조건부 컴파일과 최적화를 지원합니다.
