#### 2.6. Boost.Signals2

Boost.Signals2는 C++에서 시그널-슬롯 메커니즘을 구현한 라이브러리로, 이벤트 기반 프로그래밍을 지원합니다. 이는 시그널을 발행하고, 슬롯을 등록하여 시그널에 반응하는 기능을 제공합니다.

##### Boost.Signals2 설치
Boost 라이브러리가 설치되어 있어야 합니다. 설치 방법은 이전 섹션을 참조하세요.

##### 주요 클래스 및 함수

- `boost::signals2::signal`: 시그널 클래스를 정의하는 데 사용됩니다.
- `boost::signals2::slot`: 슬롯을 정의하는 데 사용됩니다.

##### 예제 1: 기본적인 시그널과 슬롯

```cpp
#include <iostream>
#include <boost/signals2.hpp>

// 슬롯 함수
void printHello() {
    std::cout << "Hello, world!" << std::endl;
}

int main() {
    // 시그널 선언
    boost::signals2::signal<void()> sig;

    // 슬롯 연결
    sig.connect(&printHello);

    // 시그널 발행
    sig();

    return 0;
}
```

##### 예제 2: 슬롯에 인수 전달

```cpp
#include <iostream>
#include <boost/signals2.hpp>

// 슬롯 함수
void printNumber(int num) {
    std::cout << "Number: " << num << std::endl;
}

int main() {
    // 시그널 선언
    boost::signals2::signal<void(int)> sig;

    // 슬롯 연결
    sig.connect(&printNumber);

    // 시그널 발행
    sig(42);

    return 0;
}
```

##### 예제 3: 여러 슬롯 연결 및 순서 지정

```cpp
#include <iostream>
#include <boost/signals2.hpp>

// 슬롯 함수
void printHello() {
    std::cout << "Hello, world!" << std::endl;
}

void printGoodbye() {
    std::cout << "Goodbye, world!" << std::endl;
}

int main() {
    // 시그널 선언
    boost::signals2::signal<void()> sig;

    // 슬롯 연결
    sig.connect(&printHello);
    sig.connect(&printGoodbye);

    // 시그널 발행
    sig();

    return 0;
}
```

##### 예제 4: 슬롯의 연결과 해제

```cpp
#include <iostream>
#include <boost/signals2.hpp>

// 슬롯 함수
void printMessage(const std::string& msg) {
    std::cout << "Message: " << msg << std::endl;
}

int main() {
    // 시그널 선언
    boost::signals2::signal<void(const std::string&)> sig;

    // 슬롯 연결
    boost::signals2::connection conn = sig.connect(&printMessage);

    // 시그널 발행
    sig("First message");

    // 슬롯 해제
    conn.disconnect();

    // 슬롯 해제 후 시그널 발행
    sig("Second message"); // 출력되지 않음

    return 0;
}
```

##### 예제 5: 슬롯을 가진 클래스

```cpp
#include <iostream>
#include <boost/signals2.hpp>

class Printer {
public:
    void printHello() {
        std::cout << "Hello, world!" << std::endl;
    }

    void printGoodbye() {
        std::cout << "Goodbye, world!" << std::endl;
    }
};

int main() {
    Printer printer;

    // 시그널 선언
    boost::signals2::signal<void()> sig;

    // 슬롯 연결
    sig.connect(boost::bind(&Printer::printHello, &printer));
    sig.connect(boost::bind(&Printer::printGoodbye, &printer));

    // 시그널 발행
    sig();

    return 0;
}
```

##### 예제 6: 시그널 반환값 처리

```cpp
#include <iostream>
#include <boost/signals2.hpp>

// 슬롯 함수
int add(int a, int b) {
    return a + b;
}

int multiply(int a, int b) {
    return a * b;
}

int main() {
    // 시그널 선언
    boost::signals2::signal<int(int, int)> sig;

    // 슬롯 연결
    sig.connect(&add);
    sig.connect(&multiply);

    // 시그널 발행 및 반환값 처리
    for (auto result : sig(2, 3)) {
        std::cout << "Result: " << result << std::endl;
    }

    return 0;
}
```

##### 예제 7: 커스텀 시그널 연결

```cpp
#include <iostream>
#include <boost/signals2.hpp>

// 슬롯 함수
void printMessage(const std::string& msg) {
    std::cout << "Message: " << msg << std::endl;
}

int main() {
    // 커스텀 시그널 선언
    boost::signals2::signal<void(const std::string&)> sig;

    // 슬롯 연결
    sig.connect(&printMessage);

    // 시그널 발행
    sig("Hello, Boost.Signals2!");

    return 0;
}
```

이 예제에서는 Boost.Signals2를 사용하여 시그널과 슬롯을 구현하고, 시그널 발행 시 슬롯이 호출되는 과정을 보여줍니다.
