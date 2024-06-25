### 10. 모던 C++ 기법

#### 10.1. RAII (Resource Acquisition Is Initialization)

RAII(Resource Acquisition Is Initialization)는 리소스 관리 기법으로, 리소스를 소유하는 객체가 생성될 때 리소스를 획득하고, 객체가 소멸될 때 리소스를 해제하는 패턴입니다. 이를 통해 메모리 누수와 리소스 누수를 방지할 수 있습니다.

##### RAII의 구현

RAII 패턴을 구현하려면 리소스를 관리하는 클래스가 생성자에서 리소스를 획득하고, 소멸자에서 리소스를 해제하도록 해야 합니다.

###### 예제

```cpp
#include <iostream>
using namespace std;

class File {
private:
    FILE* file;

public:
    // 생성자에서 파일을 열어 리소스를 획득
    File(const char* filename, const char* mode) {
        file = fopen(filename, mode);
        if (!file) {
            throw runtime_error("Failed to open file");
        }
        cout << "File opened" << endl;
    }

    // 소멸자에서 파일을 닫아 리소스를 해제
    ~File() {
        if (file) {
            fclose(file);
            cout << "File closed" << endl;
        }
    }

    // 파일에 데이터를 쓰는 메서드
    void write(const char* data) {
        if (file) {
            fputs(data, file);
        }
    }
};

int main() {
    try {
        File file("example.txt", "w");
        file.write("Hello, RAII!");

        // 파일은 스코프를 벗어날 때 자동으로 닫힘
    } catch (const exception& e) {
        cerr << "Error: " << e.what() << endl;
    }

    return 0;
}
```

이 예제에서 `File` 클래스는 생성자에서 파일을 열고, 소멸자에서 파일을 닫습니다. 이를 통해 파일 리소스를 안전하게 관리할 수 있습니다.

##### RAII의 장점

- **자원 관리 자동화**: 자원의 획득과 해제를 객체의 생명 주기와 함께 관리하여, 자원 누수를 방지합니다.
- **예외 안전성**: 예외가 발생하더라도 소멸자가 호출되어 자원이 해제되므로, 자원 누수를 방지합니다.
- **코드 가독성 향상**: 자원 관리 코드가 분리되어 가독성이 향상됩니다.

##### RAII의 단점

- **사용성 제한**: RAII는 객체의 생명 주기에 맞춘 자원 관리를 하기 때문에, 객체 생명 주기와 자원 관리 시점이 일치하지 않을 경우 적합하지 않을 수 있습니다.
- **오버헤드**: 자원을 관리하는 객체를 생성하고 소멸하는 과정에서 약간의 오버헤드가 발생할 수 있습니다.

##### std::unique_ptr와 std::shared_ptr를 이용한 RAII

C++ 표준 라이브러리의 `std::unique_ptr`와 `std::shared_ptr` 스마트 포인터는 RAII를 사용하여 메모리 관리를 자동화합니다.

###### 예제: std::unique_ptr

```cpp
#include <iostream>
#include <memory>
using namespace std;

class Resource {
public:
    Resource() {
        cout << "Resource acquired" << endl;
    }
    ~Resource() {
        cout << "Resource released" << endl;
    }
    void sayHello() const {
        cout << "Hello, Resource!" << endl;
    }
};

int main() {
    {
        unique_ptr<Resource> res = make_unique<Resource>();
        res->sayHello();

        // 리소스는 스코프를 벗어날 때 자동으로 해제됨
    }

    return 0;
}
```

이 예제에서 `std::unique_ptr`는 RAII를 사용하여 `Resource` 객체를 안전하게 관리합니다. `Resource` 객체는 스코프를 벗어날 때 자동으로 해제됩니다.

###### 예제: std::shared_ptr

```cpp
#include <iostream>
#include <memory>
using namespace std;

class Resource {
public:
    Resource() {
        cout << "Resource acquired" << endl;
    }
    ~Resource() {
        cout << "Resource released" << endl;
    }
    void sayHello() const {
        cout << "Hello, Resource!" << endl;
    }
};

int main() {
    shared_ptr<Resource> res1;
    {
        shared_ptr<Resource> res2 = make_shared<Resource>();
        res1 = res2;

        res2->sayHello();
        cout << "res2 use count: " << res2.use_count() << endl;
    }

    res1->sayHello();
    cout << "res1 use count: " << res1.use_count() << endl;

    // 리소스는 마지막 shared_ptr가 파괴될 때 자동으로 해제됨
    return 0;
}
```

이 예제에서 `std::shared_ptr`는 참조 횟수를 사용하여 `Resource` 객체를 관리합니다. 마지막 `shared_ptr`가 파괴될 때 `Resource` 객체가 자동으로 해제됩니다.

#### 요약

- **RAII (Resource Acquisition Is Initialization)**: 리소스를 소유하는 객체가 생성될 때 리소스를 획득하고, 객체가 소멸될 때 리소스를 해제하는 패턴입니다.
  - **구현**: 생성자에서 리소스를 획득하고, 소멸자에서 리소스를 해제합니다.
  - **장점**: 자원 관리 자동화, 예외 안전성, 코드 가독성 향상
  - **단점**: 사용성 제한, 오버헤드
- **std::unique_ptr와 std::shared_ptr**: C++ 표준 라이브러리의 스마트 포인터로, RAII를 사용하여 메모리 관리를 자동화합니다.
