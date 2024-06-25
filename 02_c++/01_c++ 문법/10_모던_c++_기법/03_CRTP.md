#### 10.3. CRTP (Curiously Recurring Template Pattern)

CRTP(Curiously Recurring Template Pattern)는 파생 클래스가 자신을 기반 클래스의 템플릿 인수로 사용하는 디자인 패턴입니다. 이 패턴은 정적 다형성을 구현하거나, 기반 클래스에서 파생 클래스의 메서드를 호출할 때 유용합니다.

##### CRTP의 구현

CRTP를 구현하려면 기반 클래스가 템플릿 클래스여야 하며, 파생 클래스가 자신을 기반 클래스의 템플릿 인수로 사용하도록 합니다.

###### 예제

```cpp
#include <iostream>
using namespace std;

// CRTP 기반 클래스
template <typename Derived>
class Base {
public:
    void interface() {
        // 파생 클래스의 메서드 호출
        static_cast<Derived*>(this)->implementation();
    }

    void implementation() {
        cout << "Base implementation" << endl;
    }
};

// 파생 클래스
class Derived : public Base<Derived> {
public:
    void implementation() {
        cout << "Derived implementation" << endl;
    }
};

int main() {
    Derived d;
    d.interface(); // Derived implementation 호출

    return 0;
}
```

이 예제에서 `Base` 클래스는 `Derived` 클래스를 템플릿 인수로 사용합니다. `interface` 메서드는 `static_cast`를 사용하여 파생 클래스의 `implementation` 메서드를 호출합니다. 이를 통해 파생 클래스의 메서드가 호출됩니다.

##### CRTP의 장점

- **정적 다형성**: 컴파일 타임에 다형성을 구현할 수 있습니다.
- **성능 향상**: 가상 함수 호출의 오버헤드가 없어 성능이 향상됩니다.
- **코드 재사용성**: 공통 기능을 기반 클래스에 구현하여 코드 재사용성을 높일 수 있습니다.

##### CRTP의 단점

- **복잡성 증가**: 템플릿을 사용하여 코드를 작성하는 것이 복잡할 수 있습니다.
- **디버깅 어려움**: 컴파일 타임에 발생하는 오류를 디버깅하는 것이 어려울 수 있습니다.

##### 사용 예제: 정적 다형성

CRTP는 정적 다형성을 구현할 때 유용합니다. 이를 통해 컴파일 타임에 다형성을 제공하여 성능을 향상시킬 수 있습니다.

###### 예제: 정적 다형성

```cpp
#include <iostream>
using namespace std;

// CRTP 기반 클래스
template <typename Derived>
class Shape {
public:
    void draw() {
        static_cast<Derived*>(this)->drawImpl();
    }
};

// 파생 클래스: 원
class Circle : public Shape<Circle> {
public:
    void drawImpl() {
        cout << "Drawing Circle" << endl;
    }
};

// 파생 클래스: 사각형
class Square : public Shape<Square> {
public:
    void drawImpl() {
        cout << "Drawing Square" << endl;
    }
};

int main() {
    Circle circle;
    Square square;

    circle.draw(); // Drawing Circle
    square.draw(); // Drawing Square

    return 0;
}
```

이 예제에서 `Shape` 클래스는 `Derived` 클래스를 템플릿 인수로 사용합니다. `draw` 메서드는 `static_cast`를 사용하여 파생 클래스의 `drawImpl` 메서드를 호출합니다. 이를 통해 정적 다형성을 구현합니다.

##### 사용 예제: 믹스인(mixin) 클래스

CRTP는 믹스인 클래스를 구현할 때도 유용합니다. 믹스인 클래스는 파생 클래스에 추가 기능을 제공하는 데 사용됩니다.

###### 예제: 믹스인 클래스

```cpp
#include <iostream>
using namespace std;

// CRTP 기반 클래스 (믹스인 클래스)
template <typename Derived>
class Logger {
public:
    void log(const string& message) {
        static_cast<Derived*>(this)->writeLog(message);
    }
};

// 파생 클래스
class FileLogger : public Logger<FileLogger> {
public:
    void writeLog(const string& message) {
        cout << "Logging to file: " << message << endl;
    }
};

int main() {
    FileLogger logger;
    logger.log("Hello, CRTP!"); // Logging to file: Hello, CRTP!

    return 0;
}
```

이 예제에서 `Logger` 클래스는 믹스인 클래스로 사용됩니다. `log` 메서드는 `static_cast`를 사용하여 파생 클래스의 `writeLog` 메서드를 호출합니다. 이를 통해 파생 클래스에 추가 기능을 제공합니다.

#### 요약

- **CRTP (Curiously Recurring Template Pattern)**: 파생 클래스가 자신을 기반 클래스의 템플릿 인수로 사용하는 디자인 패턴입니다.
  - **구현**: 기반 클래스가 템플릿 클래스여야 하며, 파생 클래스가 자신을 기반 클래스의 템플릿 인수로 사용합니다.
  - **장점**: 정적 다형성, 성능 향상, 코드 재사용성
  - **단점**: 복잡성 증가, 디버깅 어려움
- **사용 예제**: 정적 다형성 구현, 믹스인 클래스 구현
