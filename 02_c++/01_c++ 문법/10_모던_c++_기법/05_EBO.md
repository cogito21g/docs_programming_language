#### 10.5. EBO (Empty Base Optimization)

EBO(Empty Base Optimization)는 C++ 컴파일러가 빈(멤버가 없는) 베이스 클래스의 크기를 0으로 최적화하는 기법입니다. 이는 빈 베이스 클래스를 상속받는 클래스의 메모리 사용을 최적화하는 데 유용합니다.

##### EBO의 구현

EBO는 빈 베이스 클래스를 상속받을 때 적용됩니다. 컴파일러는 빈 베이스 클래스의 크기를 0으로 처리하여 파생 클래스의 메모리 사용을 줄일 수 있습니다.

###### 예제

```cpp
#include <iostream>
using namespace std;

// 빈 베이스 클래스
class EmptyBase {};

// 파생 클래스
class Derived : public EmptyBase {
    int value;
};

int main() {
    cout << "Size of EmptyBase: " << sizeof(EmptyBase) << endl; // 보통 1이지만 EBO 적용 시 0
    cout << "Size of Derived: " << sizeof(Derived) << endl;     // 4 (int의 크기)

    return 0;
}
```

이 예제에서 `EmptyBase` 클래스는 빈 클래스이며, `Derived` 클래스는 이를 상속받습니다. EBO가 적용되면 `Derived` 클래스의 크기는 `int` 멤버의 크기만큼만 차지하게 됩니다.

##### EBO의 장점

- **메모리 사용 최적화**: 빈 베이스 클래스의 크기를 0으로 처리하여 파생 클래스의 메모리 사용을 줄입니다.
- **효율성 향상**: 클래스의 메모리 레이아웃이 최적화되어 캐시 효율성이 향상될 수 있습니다.

##### EBO의 단점

- **제한된 적용 범위**: EBO는 빈 베이스 클래스에만 적용되며, 빈 베이스 클래스가 아닌 경우에는 적용되지 않습니다.
- **컴파일러 의존성**: EBO는 컴파일러 최적화 기법이므로 모든 컴파일러에서 동일하게 적용되지 않을 수 있습니다.

##### EBO의 사용 예제

EBO는 템플릿 프로그래밍에서 빈 베이스 클래스를 사용하는 경우 특히 유용합니다. 예를 들어, 정책 기반 설계(policy-based design)에서 빈 정책 클래스를 사용할 때 메모리 최적화를 할 수 있습니다.

###### 예제: 정책 기반 설계에서의 EBO

```cpp
#include <iostream>
using namespace std;

// 빈 정책 클래스
class EmptyPolicy {};

// 정책 기반 클래스
template <typename Policy>
class PolicyBasedClass : public Policy {
    int value;
public:
    PolicyBasedClass(int val) : value(val) {}

    void print() const {
        cout << "Value: " << value << endl;
    }
};

int main() {
    PolicyBasedClass<EmptyPolicy> obj(42);
    cout << "Size of PolicyBasedClass: " << sizeof(obj) << endl; // 4 (int의 크기)
    obj.print(); // Value: 42

    return 0;
}
```

이 예제에서 `EmptyPolicy` 클래스는 빈 클래스이며, `PolicyBasedClass` 템플릿 클래스는 이를 상속받습니다. EBO가 적용되어 `PolicyBasedClass` 객체의 크기는 `int` 멤버의 크기만큼만 차지합니다.

#### 요약

- **EBO (Empty Base Optimization)**: 빈 베이스 클래스의 크기를 0으로 최적화하는 컴파일러 기법입니다.
  - **구현**: 빈 베이스 클래스를 상속받을 때 적용됩니다.
  - **장점**: 메모리 사용 최적화, 효율성 향상
  - **단점**: 제한된 적용 범위, 컴파일러 의존성
- **사용 예제**: 템플릿 프로그래밍에서 빈 정책 클래스를 사용할 때 메모리 최적화를 할 수 있습니다.
