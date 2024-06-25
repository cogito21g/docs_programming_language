#### 7.2. consteval: 컴파일 타임 함수 평가

C++20에서 도입된 `consteval` 키워드는 함수가 컴파일 타임에 평가되어야 함을 보장하는 데 사용됩니다. 이는 컴파일 타임 상수를 생성하고, 컴파일 타임에만 유효한 작업을 수행하는 데 유용합니다.

##### 기본 사용법

`consteval` 키워드는 함수 정의 앞에 사용되며, 해당 함수는 반드시 컴파일 타임에 평가되어야 합니다.

###### 예제

```cpp
#include <iostream>
using namespace std;

// consteval 함수 정의
consteval int square(int n) {
    return n * n;
}

int main() {
    constexpr int value = square(5); // 컴파일 타임에 평가됨
    cout << "Square of 5: " << value << endl;

    // int runtimeValue = square(6); // 컴파일 오류: consteval 함수는 런타임에 호출할 수 없음

    return 0;
}
```

이 예제에서 `square` 함수는 `consteval`로 정의되어 컴파일 타임에만 평가됩니다. `square(5)`는 컴파일 타임에 평가되어 `value` 변수에 할당됩니다. 런타임에 호출하려고 하면 컴파일 오류가 발생합니다.

##### consteval과 constexpr의 차이점

- `consteval` 함수는 반드시 컴파일 타임에 평가되어야 하며, 런타임 호출은 허용되지 않습니다.
- `constexpr` 함수는 컴파일 타임에 평가될 수도 있고, 런타임에 평가될 수도 있습니다.

###### 예제: constexpr 함수

```cpp
#include <iostream>
using namespace std;

// constexpr 함수 정의
constexpr int cube(int n) {
    return n * n * n;
}

int main() {
    constexpr int value = cube(3); // 컴파일 타임에 평가됨
    cout << "Cube of 3: " << value << endl;

    int runtimeValue = cube(4); // 런타임에 평가됨
    cout << "Cube of 4: " << runtimeValue << endl;

    return 0;
}
```

이 예제에서 `cube` 함수는 `constexpr`로 정의되어 컴파일 타임 및 런타임에 호출될 수 있습니다. `cube(3)`는 컴파일 타임에 평가되고, `cube(4)`는 런타임에 평가됩니다.

##### consteval의 실용적인 사용

`consteval` 키워드는 컴파일 타임 상수를 생성하거나 컴파일 타임에만 유효한 작업을 수행하는 데 유용합니다. 예를 들어, 컴파일 타임에만 유효한 상수 테이블을 생성할 수 있습니다.

###### 예제: 상수 테이블 생성

```cpp
#include <iostream>
using namespace std;

// consteval 함수로 팩토리얼 값을 계산하여 상수 테이블 생성
consteval int factorial(int n) {
    return (n <= 1) ? 1 : (n * factorial(n - 1));
}

int main() {
    constexpr int table[5] = {
        factorial(1),
        factorial(2),
        factorial(3),
        factorial(4),
        factorial(5)
    };

    for (int i = 0; i < 5; ++i) {
        cout << "Factorial of " << (i + 1) << " is " << table[i] << endl;
    }

    return 0;
}
```

이 예제에서 `factorial` 함수는 `consteval`로 정의되어 컴파일 타임에 팩토리얼 값을 계산합니다. `table` 배열은 컴파일 타임 상수로 초기화됩니다.

##### consteval 함수와 템플릿

`consteval` 함수는 템플릿과 함께 사용될 수 있습니다. 이를 통해 보다 일반화된 컴파일 타임 상수를 생성할 수 있습니다.

###### 예제: consteval 템플릿 함수

```cpp
#include <iostream>
using namespace std;

// consteval 템플릿 함수 정의
template<typename T>
consteval T add(T a, T b) {
    return a + b;
}

int main() {
    constexpr int result = add(3, 4); // 컴파일 타임에 평가됨
    cout << "Result: " << result << endl;

    return 0;
}
```

이 예제에서 `add` 함수는 `consteval` 템플릿 함수로 정의되어 컴파일 타임에 두 값을 더합니다. `result` 변수는 컴파일 타임 상수로 초기화됩니다.

#### 요약

- **consteval**: 함수가 컴파일 타임에 평가되어야 함을 보장합니다.
  - **사용법**: 함수 정의 앞에 `consteval` 키워드를 사용합니다.
- **consteval vs constexpr**: `consteval` 함수는 반드시 컴파일 타임에 평가되어야 하며, `constexpr` 함수는 컴파일 타임 및 런타임에 평가될 수 있습니다.
- **실용적 사용 예제**: 컴파일 타임 상수 테이블 생성, 템플릿 함수와의 결합 등.
