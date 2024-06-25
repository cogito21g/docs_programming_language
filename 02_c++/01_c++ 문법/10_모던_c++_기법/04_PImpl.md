#### 10.4. PImpl (Pointer to Implementation)

PImpl(Pointer to Implementation) 패턴은 클래스의 구현 세부 사항을 감추기 위해 사용하는 디자인 패턴입니다. 이 패턴을 사용하면 클래스의 인터페이스와 구현을 분리할 수 있으며, 코드 컴파일 시간을 단축하고, 구현 세부 사항을 숨겨서 캡슐화를 강화할 수 있습니다.

##### PImpl 패턴의 구현

PImpl 패턴을 구현하려면 클래스의 구현 세부 사항을 별도의 클래스로 분리하고, 원래 클래스에서 이 구현 클래스를 포인터로 참조합니다. 이를 통해 클래스의 인터페이스는 구현 세부 사항에 대한 정보를 가지지 않게 됩니다.

###### 예제

```cpp
#include <iostream>
#include <memory>
using namespace std;

// 구현 클래스 (Implementation Class)
class MyClassImpl {
public:
    void doSomething() {
        cout << "Doing something in MyClassImpl" << endl;
    }
};

// 인터페이스 클래스 (Interface Class)
class MyClass {
public:
    MyClass();
    ~MyClass();
    void doSomething();

private:
    unique_ptr<MyClassImpl> pImpl; // 구현 클래스에 대한 포인터
};

// 생성자
MyClass::MyClass() : pImpl(make_unique<MyClassImpl>()) {}

// 소멸자
MyClass::~MyClass() = default;

// 메서드
void MyClass::doSomething() {
    pImpl->doSomething();
}

int main() {
    MyClass myClass;
    myClass.doSomething(); // Doing something in MyClassImpl

    return 0;
}
```

이 예제에서 `MyClass`는 구현 세부 사항을 `MyClassImpl` 클래스에 위임합니다. `MyClass`는 `MyClassImpl`에 대한 포인터 `pImpl`을 가지고 있으며, 생성자에서 `MyClassImpl` 객체를 생성합니다. `doSomething` 메서드는 `MyClassImpl` 객체의 `doSomething` 메서드를 호출합니다.

##### PImpl 패턴의 장점

- **캡슐화 강화**: 클래스의 구현 세부 사항을 숨겨서 캡슐화를 강화할 수 있습니다.
- **컴파일 시간 단축**: 클래스의 인터페이스가 변경되지 않으면 구현 세부 사항이 변경되더라도 클래스의 사용자는 다시 컴파일할 필요가 없습니다.
- **의존성 감소**: 클래스의 구현 세부 사항이 변경되더라도 인터페이스가 변경되지 않으므로 의존성이 감소합니다.

##### PImpl 패턴의 단점

- **간접 접근 비용**: 구현 클래스에 대한 간접 접근이 추가되므로 성능 오버헤드가 발생할 수 있습니다.
- **복잡성 증가**: 구현 클래스를 별도로 관리해야 하므로 코드의 복잡성이 증가할 수 있습니다.

##### 사용 예제: 대규모 프로젝트에서의 PImpl 패턴

PImpl 패턴은 대규모 프로젝트에서 클래스의 구현 세부 사항을 숨기고 컴파일 시간을 단축하는 데 유용합니다. 이를 통해 코드의 유지 보수성을 높일 수 있습니다.

###### 예제: 대규모 프로젝트에서의 PImpl 패턴

```cpp
// MyClass.h
#include <memory>

class MyClassImpl; // 전방 선언

class MyClass {
public:
    MyClass();
    ~MyClass();
    void doSomething();

private:
    std::unique_ptr<MyClassImpl> pImpl; // 구현 클래스에 대한 포인터
};

// MyClass.cpp
#include "MyClass.h"
#include <iostream>
using namespace std;

class MyClassImpl {
public:
    void doSomething() {
        cout << "Doing something in MyClassImpl" << endl;
    }
};

MyClass::MyClass() : pImpl(make_unique<MyClassImpl>()) {}

MyClass::~MyClass() = default;

void MyClass::doSomething() {
    pImpl->doSomething();
}

// main.cpp
#include "MyClass.h"

int main() {
    MyClass myClass;
    myClass.doSomething(); // Doing something in MyClassImpl

    return 0;
}
```

이 예제에서 `MyClass`는 헤더 파일에 구현 세부 사항을 포함하지 않으며, 구현 클래스는 소스 파일에만 정의됩니다. 이를 통해 헤더 파일을 포함하는 파일들이 구현 세부 사항에 의존하지 않게 됩니다.

#### 요약

- **PImpl (Pointer to Implementation) 패턴**: 클래스의 구현 세부 사항을 감추기 위해 사용하는 디자인 패턴입니다.
  - **구현**: 클래스의 구현 세부 사항을 별도의 클래스로 분리하고, 원래 클래스에서 이 구현 클래스를 포인터로 참조합니다.
  - **장점**: 캡슐화 강화, 컴파일 시간 단축, 의존성 감소
  - **단점**: 간접 접근 비용, 복잡성 증가
- **사용 예제**: 대규모 프로젝트에서 클래스의 구현 세부 사항을 숨기고 컴파일 시간을 단축하는 데 유용합니다.
