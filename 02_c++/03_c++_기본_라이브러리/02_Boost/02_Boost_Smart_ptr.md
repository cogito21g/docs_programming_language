#### 2.2. Boost.Smart_ptr

Boost.Smart_ptr 라이브러리는 C++ 표준 라이브러리에 추가된 스마트 포인터의 원형으로, 메모리 관리를 간단하고 안전하게 만들어 줍니다. 주요 스마트 포인터로는 `shared_ptr`, `weak_ptr`, `scoped_ptr`, `intrusive_ptr` 등이 있습니다.

##### shared_ptr
`shared_ptr`은 참조 계수를 사용하여 여러 소유자가 동일한 객체를 공유할 수 있도록 합니다. 마지막 소유자가 사라지면 객체가 삭제됩니다.

**사용법 예제**:

```cpp
#include <iostream>
#include <boost/shared_ptr.hpp>

int main() {
    boost::shared_ptr<int> ptr1(new int(42));
    std::cout << "ptr1: " << *ptr1 << std::endl;

    {
        boost::shared_ptr<int> ptr2 = ptr1;
        std::cout << "ptr2: " << *ptr2 << std::endl;
        std::cout << "Use count: " << ptr1.use_count() << std::endl;
    }

    std::cout << "Use count after ptr2 goes out of scope: " << ptr1.use_count() << std::endl;

    return 0;
}
```

##### weak_ptr
`weak_ptr`은 `shared_ptr`의 순환 참조 문제를 해결하기 위해 사용됩니다. `weak_ptr`은 객체를 소유하지 않으며, 객체의 생존 여부를 확인할 수 있습니다.

**사용법 예제**:

```cpp
#include <iostream>
#include <boost/shared_ptr.hpp>
#include <boost/weak_ptr.hpp>

int main() {
    boost::shared_ptr<int> ptr1 = boost::make_shared<int>(42);
    boost::weak_ptr<int> weakPtr = ptr1;

    std::cout << "Use count before reset: " << ptr1.use_count() << std::endl;

    if (boost::shared_ptr<int> sharedPtr = weakPtr.lock()) {
        std::cout << "Weak pointer is valid, value: " << *sharedPtr << std::endl;
    } else {
        std::cout << "Weak pointer is expired" << std::endl;
    }

    ptr1.reset();
    std::cout << "Use count after reset: " << ptr1.use_count() << std::endl;

    if (boost::shared_ptr<int> sharedPtr = weakPtr.lock()) {
        std::cout << "Weak pointer is valid, value: " << *sharedPtr << std::endl;
    } else {
        std::cout << "Weak pointer is expired" << std::endl;
    }

    return 0;
}
```

##### scoped_ptr
`scoped_ptr`은 객체의 유일한 소유권을 가지며, 소유자가 범위를 벗어날 때 객체를 자동으로 삭제합니다. 복사할 수 없고, 이동할 수 없습니다.

**사용법 예제**:

```cpp
#include <iostream>
#include <boost/scoped_ptr.hpp>

int main() {
    boost::scoped_ptr<int> ptr(new int(42));
    std::cout << "ptr: " << *ptr << std::endl;

    // boost::scoped_ptr은 복사할 수 없습니다.
    // boost::scoped_ptr<int> ptr2 = ptr; // 오류 발생

    return 0;
}
```

##### intrusive_ptr
`intrusive_ptr`은 사용자 정의 참조 계수 메커니즘을 사용하는 스마트 포인터입니다. 이는 객체가 참조 계수의 증가와 감소를 제어할 수 있게 합니다.

**사용법 예제**:

```cpp
#include <iostream>
#include <boost/intrusive_ptr.hpp>

class MyClass {
public:
    MyClass() : ref_count(0) {
        std::cout << "MyClass Constructor" << std::endl;
    }

    ~MyClass() {
        std::cout << "MyClass Destructor" << std::endl;
    }

    friend void intrusive_ptr_add_ref(MyClass* p) {
        ++p->ref_count;
    }

    friend void intrusive_ptr_release(MyClass* p) {
        if (--p->ref_count == 0) {
            delete p;
        }
    }

private:
    int ref_count;
};

int main() {
    boost::intrusive_ptr<MyClass> ptr1(new MyClass());
    {
        boost::intrusive_ptr<MyClass> ptr2 = ptr1;
        std::cout << "Use count: " << ptr2.use_count() << std::endl;
    }
    std::cout << "Use count: " << ptr1.use_count() << std::endl;

    return 0;
}
```

##### 예제: 스마트 포인터를 사용한 클래스 관리

스마트 포인터를 사용하여 클래스를 안전하게 관리하는 예제입니다.

```cpp
#include <iostream>
#include <boost/shared_ptr.hpp>
#include <boost/scoped_ptr.hpp>

class MyClass {
public:
    MyClass() { std::cout << "MyClass Constructor" << std::endl; }
    ~MyClass() { std::cout << "MyClass Destructor" << std::endl; }
    void display() { std::cout << "MyClass display()" << std::endl; }
};

void sharedPtrExample() {
    boost::shared_ptr<MyClass> sharedPtr1 = boost::make_shared<MyClass>();
    {
        boost::shared_ptr<MyClass> sharedPtr2 = sharedPtr1;
        sharedPtr2->display();
        std::cout << "Use count inside block: " << sharedPtr1.use_count() << std::endl;
    }
    std::cout << "Use count outside block: " << sharedPtr1.use_count() << std::endl;
}

void scopedPtrExample() {
    boost::scoped_ptr<MyClass> scopedPtr(new MyClass());
    scopedPtr->display();
}

int main() {
    std::cout << "Shared Pointer Example" << std::endl;
    sharedPtrExample();

    std::cout << "Scoped Pointer Example" << std::endl;
    scopedPtrExample();

    return 0;
}
```

이 예제에서는 `shared_ptr`과 `scoped_ptr`을 사용하여 `MyClass` 객체를 관리하고, 객체가 적절하게 생성되고 파괴되는지 확인합니다.
